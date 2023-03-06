**一个使用叫做“action”的事件来管理和更新应用状态的模式和工具库** 它以集中式Store（centralized store）的方式对整个应用中使用的状态进行集中管理，其规则确保状态只能以可预测的方式更新。

三个功能：获取当前状态；更新状态；监听状态变化。
- 通过 `store.getState()` 来获取当前状态，
- 通过 `store.dispatch()` 和 `reducer` 来更新状态，
- 通过 `store.subscribe()` 来监听状态变化。