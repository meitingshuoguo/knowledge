# vue3 响应式

```js
const isObject = val => val !== null && typeof val === 'object'
const hasOwn = (target, key) => Object.prototype.hasOwnProperty.call(target, key)

/**
 * dep的结构  
 * WeakMap -> Set
 * 
 * 触发步骤
 * effect > track > trigger > effect()
 * 
 * track 
 * 根据activeEffect, 触发依赖的收集   key-----activeEffect
 * 
 * trigger
 * 触发key的activeEffect
 */
export function reactive (target) {
  if (!isObject(target)) return target

  const handler = {
    get (target, key, receiver) {
      track(target, key)

      const result = Reflect.get(target, key, receiver)
      if (isObject(result)) {
        return reactive(result)
      }
      return result
    },

    set (target, key, value, receiver) {
      const oldValue = Reflect.get(target, key, reactive)

      let result = true
      if (oldValue !== value) {
        result = Reflect.set(target, key, value, receiver)
        trigger(target, key)
      }
      return result
    },

    deleteProperty (target, key) {
      const hadKey = hasOwn(target, key)
      const result = Reflect.deleteProperty(target, key)

      if (hadKey && result) {
        target(target, key)
      }
      return result
    },
  }
  return new Proxy(target, handler)
}

let activeEffect = null
export function effect (callback) {
  activeEffect = callback
  callback()
  activeEffect = null
}

let targetMap = new WeakMap()

export function track (target, key) {
  if (!activeEffect) return
  let depsMap = targetMap.get(target)
  if (!depsMap) {
    targetMap.set(target, (depsMap = new Map()))
  }
  let dep = depsMap.get(key)
  if (!dep) {
    depsMap.set(key, (dep = new Set()))
  }
  if (!dep.has(activeEffect)) {
    dep.add(activeEffect)
  }
}

export function trigger (target, key) {
  const depsMap = targetMap.get(target)
  if (!depsMap) {
    return
  }
  const dep = depsMap.get(key)

  if (dep) {
    dep.forEach(effect => {
      effect()
    })
  }
}

const convert = val => (isObject(val) ? reactive(val) : val)

class RefImpl {
  constructor(_rawValue) {
    this._rawValue = _rawValue
    this.__v_isRef = true
    this._value = convert(_rawValue)
  }
  get value () {
    track(this, 'value')
    return this._value
  }
  set value (newValue) {
    if (newValue !== this._value) {
      this._rawValue = newValue
      this._value = convert(this._rawValue)
      trigger(this, 'value')
    }
  }
}

export function ref (rawValue) {
  if (isObject(rawValue) && rawValue.__v_isRef) return

  return new RefImpl(rawValue)
}

class ObjectRefImpl {
  constructor(proxy, _key) {
    this._proxy = proxy
    this._key = _key
    this.__v_isRef = true
  }
  get value () {
    return this._proxy[this._key]
  }
  set value (newVal) {
    this._proxy[this._key] = newVal
  }
}

export function toRef (proxy, key) {
  return new ObjectRefImpl(proxy, key)
}

export function toRefs (proxy) {
  const ret = proxy instanceof Array ? new Array(proxy.length) : {}
  for (const key in proxy) {
    ret[key] = toRef(proxy, key)
  }
  return ret
}
```



# 前端路由原理

```js
class MyRouter {
  constructor(config) {
    // 路由配置列表
    this._routes = config.routes;
    // 路由历史栈
    this.routeHistory = [];
    this.currentUrl = '';
    this.currentIndex = -1;

    // 跳转中间变量
    this.changeFlag = false;

    this.init();
  }
  init () {
    window.addEventListener(
      'hashchange',
      this.refresh.bind(this),
      false
    );

    window.addEventListener(
      'load',
      this.refresh.bind(this),
      false
    );
  }
  // 单页更新
  refresh () {
    // 1. 路由参数处理
    if (this.changeFlag) {
      this.changeFlag = false;
    } else {
      this.currentUrl = location.hash.slice(1) || '/';
      // 去除分叉路径
      this.routeHistory = this.routeHistory.slice(0, this.currentIndex + 1);
      this.routeHistory.push(this.currentUrl);
      this.currentIndex++;
    }

    // 2. 切换模块
    let path = MyRouter.getPath();
    let currentComponentName = '';
    let nodeList = document.querySelectorAll('[data-component-name]');

    // 查找当前路由名称对应
    // find()
    for (let i = 0; i < this._routes.length; i++) {
      if (this._routes[i].path === path) {
        currentComponentName = this._routes[i].name;
        break;
      }
    }

    // 遍历控制节点模块展示
    nodeList.forEach(item => {
      if (item.dataset.componentName === currentComponentName) {
        item.style.display = 'block';
      } else {
        item.style.display = 'none';
      }
    })
  }

  push (option) {
    if (option.path) {
      MyRouter.changeHash(option.path, option.query);
    } else if (option.name) {
      let path = '';

      for (let i = 0; i < this._routes.length; i++) {
        if (this._routes[i].name === option.name) {
          path = this._routes[i].path;
          break;
        }
      }

      if (path) {
        MyRouter.changeHash(path, option.query);
      }
    }
  }

  back () {
    this.changeFlag = true;
    // ……
  }

  front () {
    this.changeFlag = true;
  }

  static getPath () {
    let href = window.location.href;
    let index = href.indexOf('#');
    if (index < 0) {
      return '';
    }
    href = href.slice(index + 1);

    let searchIndex = href.indexOf("?");
    if (searchIndex < 0) {
      return href;
    } else {
      return href.slice(0, searchIndex);
    }
  }

  static changeHash (path, query) {
    if (query) {
      let str = '';
      for (let i in query) {
        str += '&' + i + '=' + query[i];
      }

      window.location.hash
        = str
          ? path + '?' + str.slice(1)
          : path;
    } else {
      window.location.hash = path;
    }
  }
}
```



# redux原理

流程：createStore传入reducer，返回的store的dispatch触发action执行reducer对应type的纯函数，更新store内的state，渲染更新视图。

```js
function createStore(reducer, enhancer) {
  if(enhancer && typeof enhancer === 'function') {
    const newCreateStore = enhancer(createStore)  //这里用了中间件对其做增强
    const newStore = newCreateStore(reducer)
    return newStore
  }
  let state;             
  let listeners = []; 

  function subscribe(callback) {
    listeners.push(callback);       
  }

  // 1. reducer 执行   2. 将subscribe执行
  function dispatch(action) {
    state = reducer(state, action)

    for (let i = 0; i < listeners.length; i++) {
      const listener = listeners[i];
      listener();
    }
  }

  // getState直接返回state
  function getState() {
    return state;
  }

  // store包装一下前面的方法直接返回
  const store = {
    subscribe,
    dispatch,
    getState
  }

  return store;
}
```

# react的hooks的原理

hooks基于fiber、会在 fiber 节点上放一个链表，每个节点的 `memorizedState` 属性上存放了对应的不同hooks依赖的数据，通过next指向下一个memorizedState，注意这里会区分mount还是update阶段，同时为了顺序执行hooks不能出现在条件、函数、循环中执行。useCallback、useMemo、useRef无非处理不同阶段的缓存处理。重点是useState和useEffect的调度实现。

useEffect核心：通过pushEffect生成effect对象的环状的单向链表、并会存在`fiber.updateQueue`中, 根据依赖项做更新，对上次的effect对象做比对做异步更新。相比useLayoutEffect是异步，在commit阶段的beforeMutation不会阻塞渲染。

```js
function pushEffect(tag, create, destroy, deps) {
  // 创建 effect 对象
  var effect = {
    tag: tag,	// effect的类型，区分是 useEffect 还是 useLayoutEffect
    create: create,	// 传入use（Layout）Effect函数的第一个参数，即回调函数
    destroy: destroy,	// 销毁函数
    deps: deps,	// 依赖项
    // Circular
    next: null
  };
  // 获取 fiber 的 updateQueue
  var componentUpdateQueue = currentlyRenderingFiber$1.updateQueue;

  if (componentUpdateQueue === null) {
    componentUpdateQueue = createFunctionComponentUpdateQueue();
    currentlyRenderingFiber$1.updateQueue = componentUpdateQueue;
    // 如果前面没有 effect，则将componentUpdateQueue.lastEffect指针指向effect环状链表的最后一个
    componentUpdateQueue.lastEffect = effect.next = effect;
  } else {
    var lastEffect = componentUpdateQueue.lastEffect;

    if (lastEffect === null) {
      componentUpdateQueue.lastEffect = effect.next = effect;
    } else {
      // 如果前面已经有 effect，将当前生成的 effect 插入链表尾部
      var firstEffect = lastEffect.next;
      lastEffect.next = effect;
      effect.next = firstEffect;
      // 把最后收集到的 effect 放到 lastEffect 上面
      componentUpdateQueue.lastEffect = effect;
    }
  }

  return effect;
}

function createFunctionComponentUpdateQueue() {
  return {
    lastEffect: null,
    stores: null
  };
}
```

