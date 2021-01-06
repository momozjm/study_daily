### 一、memo
在 class component 时代，为了性能优化我们经常会选择使用 PureComponent,每次对 props 进行一次浅比较，当然，除了 PureComponent 外，我们还可以在 shouldComponentUpdate 中进行更深层次的控制。

在 Function Component 的使用中， **React 贴心的提供了 `React.memo` 这个 HOC（高阶组件），与 PureComponent 很相似**，但是是专门给 Function Component 提供的，对 Class Component 并不适用。

但是相比于 PureComponent ，React.memo() 可以支持指定一个参数，可以相当于 shouldComponentUpdate 的作用，因此 React.memo() 相对于 PureComponent 来说，用法更加方便。

问题1：当在一个父组件中调用一个子组件的时候,由于父组件的state发生了改变导致父组件更新,虽然子组件没有发生改变,但是也会进行更新
```
const parentComponent = () => {
    const [count, setcount] = useState(0)
    console.log('----parent render----')
    return (
      <div>
        <h1>count: {count}</h1>
        <ChildComponent />
        <button onClick={() => {setcount(count + 1)}}>change</button>
        <button onClick={() => {setcount(0)}}>reset</button>
      </div>
    )
}

export default function childComponent(props) {
    console.log('----child render-----')
    return (
        <h1>测试memo</h1>
    )
}
```
效果: 当点击父组件的button的时候，就会打印
```
----parent render----
----child render-----
```
解决方案: 使用memo对子组件进行包裹
```
import React from 'react'
export default React.memo(function childComponent(props) {
    console.log('----child render-----')
    return (
        <h1>测试memo</h1>
    )
})
```
效果: 当点击父组件的button的时候，就会打印
```
----parent render----
```
 
 问题2:当父组件传递给子组件是一个对象时，子组件重复渲染。需要将自定义的比较函数通过第二个参数传入来实现
 ```
 import React from 'react'

function areEqual(prevProps, nextProps) {
    return prevProps.value === nextProps.value // 返回true阻止子组件渲染
}

function childComponent(props) {
    console.log('----child render-----')
    return (
        <h1>测试memo{props.count.value}</h1>
    )
}

export default React.memo(childComponent, areEqual)
 ```
>**注意：**
>与 class 组件中 shouldComponentUpdate() 方法不同的是，如果 props 相等，areEqual 会返回 true；如果 props 不相等，则返回 false。这与 shouldComponentUpdate 方法的返回>值相反。