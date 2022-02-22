# Snabbdom源码学习

## 如何学习源码
- 宏观了解
- 带着目标看源码
- 看源码的过程要不求甚解
- 调试
- 参考资料

## Snabbdom和核心
- init() 设置模块，创建patch()函数
- 使用h()函数创建JS对象(VNode)描述真实DOM
- patch()比较新旧两个VNode
- 把变化的内容更新到真实DOM树

## Snabbdom源码
- 官方源码地址
    - [https://github.com/snabbdom/snabbdom](https://github.com/snabbdom/snabbdom)
    - 版本：v2.1.0
- 克隆代码
    - git clone -b v2.1.0 --depth=1 https://github.com/snabbdom/snabbdom.git

## h函数介绍
- 作用：创建VNode对象
- Vue中的h函数
    ```js
    new Vue({
        render: h => h(App)
    }).$mount('#app')
    ```
- h函数最早见于`hyperscript`，使用JS创建超文本

### 函数的重载
- 参数个数或参数类型不同的函数
- js中没有重载的概念
- TS 中有重载，不过重载的实现还是通过代码调整参数
#### 函数的重载-参数个数
```ts
function add (a: number, b: number) {
    console.log(a + b)
}
function add (a: number, b: number, c: number) {
    console.log(a + b + c)
}
add(1, 2) // 3
add(1, 2, 3) // 6
```
#### 函数的重载-参数类型
```ts
function add (a: number, b: number) {
    console.log(a + b)
}
function add (a: number, b: string) {
    console.log(a + b)
}
add(1, 2) // 3
add(1, '2') // 12
```

## patch整体过程分析
- patch(oldVnode, newVnode)
- 把新节点中变化的内容渲染到真实DOM，最后返回新节点作为下一次处理的旧节点
- 对比新旧VNode是否相同节点（节点的key和sel相同）
- 如果不是相同节点，删除之前的内容，重新渲染
- 如果是相同节点，再判断新的VNode是否有text，如果有并且和oldVnode的text不同，直接更新文本内容
- 如果新的VNode有children，判断子节点是否有变化
