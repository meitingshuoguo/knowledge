# 核心

React 的核心设计是维护一个虚拟或影子 DOM，渲染 DOM 树的副本，其中每一个独立的节点都代表一个 React 元素。在对 UI 做更新后，React 都会递归更新两个树之间的差异，并将累计的变更传递到渲染通道。

> React 16 中引入了一套新算法来完成这段流程，也就是[React Fiber](https://github.com/acdlite/react-fiber-architecture)，取代了原先基于堆栈的算法。所有 React 元素或者说是组件都是一个 Fiber，每个 Fiber 的子和兄弟都可以延迟渲染，React 通过对这些 Fiber 的延迟渲染实现数量级更高、效果更好的 UI 响应。具体观感对比可见[这里](https://claudiopro.github.io/react-fiber-vs-stack-demo/fiber.html)。
>
> React 17 以此为基础构建，而 React 18 则为这套流程带来了更多可控性。
>
> React 18 所做的是在所有子树被评估之前暂停 DOM 树之后的差异化渲染传递。最终结果？每个渲染通道现在都可以中断。React 可以有选择地在后台更新部分 UI，暂停、恢复或者放弃正在进行的渲染过程，同时保证 UI 不会崩溃，不会掉帧，或帧数时间一致（如，60 FPS 的 UI 应该需要 16.67 毫秒来渲染每一帧）。
>
> React 18 功能背后的核心概念是并发渲染，其中包括悬念、流式 HTML、过渡 API，等等。每次这些新功能都是并发式的，用户不用具体了解其背后的机制原理。

> 💡 随着 React 18 加入 React Native，移动设备的游戏规则将彻底改变。

 

