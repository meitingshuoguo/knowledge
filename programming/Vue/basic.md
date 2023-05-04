# 重点概念

## 响应式系统（变化侦测）

1. 侦测一个对象的变化

   - `Object.defineProperty`
   - `Proxy`

2. 侦测一个数组的变化

   - 采用拦截器，将 7 个可以改变数组自身内容的方法进行覆盖

3. 收集依赖

   - 需要将对象的所有属性（包括子属性）都转成 getter/setter 的形式
   - 在 getter 中收集依赖，在 setter 中触发依赖。
   - 收集谁，就是当属性变化后，通知谁。就是`Watcher`
   - ES6 之前，只能侦测属性是否有修改，不能侦测属性被添加或删除。需要用`vm.$set`和`vm.$delete`

## 例子

```javascript
import{ ref, reactive, watch, nextTick }from'vue'
//定义4种响应式数据/状态

//1、ref值为基本类型
const simplePerson =ref('张三')
//2、ref值为引用类型，等价于：person.value = reactive({ name: '张三' })
const person =ref({   
    name:'张三'
})
//3、ref值包含嵌套的引用类型，等价于：complexPerson.value = reactive({ name: '张三', info: { age: 18 } })
const complexPerson =ref({ name:'张三', info:{ age:18}})
//4、reactive
const reactivePerson =reactive({ name:'张三', info:{ age:18}})

//改变属性，观察以下不同情景下的监听结果
nextTick(()=>{ 
    simplePerson.value ='李四' 
    person.value.name ='李四' 
    complexPerson.value.info.age =20
    reactivePerson.info.age =22
})

//情景一：数据源为RefImpl
watch(simplePerson,(newVal)=>{
    console.log(newVal)//输出：李四
})
//情景二：数据源为'张三'
watch(simplePerson.value,(newVal)=>{ 
    console.log(newVal)//非法数据源，监听不到且控制台告警 
})
//情景三：数据源为RefImpl，但是.value才是响应式对象，所以要加deep
watch(person,(newVal)=>{ 
    console.log(newVal)//输出：{name: '李四'}
},{
    deep:true//必须设置，否则监听不到内部变化
})
//情景四：数据源为响应式对象
watch(person.value,(newVal)=>{ 
    console.log(newVal)//输出：{name: '李四'}
})
//情景五：数据源为'张三'
watch(person.value.name,(newVal)=>{ 
    console.log(newVal)//非法数据源，监听不到且控制台告警 
})
//情景六：数据源为getter函数，返回基本类型
watch(
    ()=> person.value.name, 
    (newVal)=>{ 
        console.log(newVal)//输出：李四
    }
)
//情景七：数据源为响应式对象（在Vue3中状态都是默认深层响应式的）
watch(complexPerson.value.info,(newVal, oldVal)=>{ 
    console.log(newVal)//输出：Proxy {age: 20} 
    console.log(newVal === oldVal)//输出：true
})
//情景八：数据源为getter函数，返回响应式对象
watch( 
    ()=> complexPerson.value.info, 
    (newVal)=>{ 
        console.log(newVal)//除非设置deep: true或info属性被整体替换，否则监听不到
    }
)
//情景九：数据源为响应式对象
watch(reactivePerson,(newVal)=>{ 
    console.log(newVal)//不设置deep: true也可以监听到 
})
```

> 总结：
>
> 1.在Vue3中状态都是默认深层响应式的（情景七），嵌套的引用类型在取值（get）时一定是返回Proxy响应式对象；
>
> 2.watch数据源为响应式对象时（情景四、七、九），会隐式的创建一个深层侦听器，不需要再显示设置deep: true；
>
> 3.情景三和情景八两种情况下，必须显示设置deep: true，强制转换为深层侦听器；
>
> 4.情景五和情景七对比下，虽然写法完全相同，但是如果属性值为基本类型时是监听不到的，尤其是ts类型声明为any时，ide也不会提示告警，导致排查问题比较费力；
>
> 5.所以精确的ts类型声明很重要，否则经常会出现莫名其妙的watch不生效的问题；
>
> 6.ref值为基本类型时通过get\set拦截实现响应式；ref值为引用类型时通过将.value属性转换为reactive响应式对象实现；
>
> 7.deep会影响性能，而reactive会隐式的设置deep: true，所以只有明确状态数据结构比较简单且数据量不大时使用reactive，其他一律使用ref。
