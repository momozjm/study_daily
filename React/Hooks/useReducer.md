### useReducer
---
#### 概览：
1. 某些场景useReducer 会比 useState 更适用
2. useReducer 还能给那些会触发深更新的组件做性能优化，因为你可以向子组件传递 dispatch 而不是回调函数 。原因见useCallback
3. 使用useReducer时，子组件使用父组件state会带来性能问题
4. 使用 useMemo() 解决 state Context 透传的性能问题


#### 一、优势
useReducer 会比 useState 更适用，例如 state 逻辑较复杂且包含多个子值，或者下一个 state 依赖于之前的 state 等。并且，使用 useReducer 还能给那些会触发深更新的组件做性能优化，因为你可以向子组件传递 dispatch 而不是回调函数 

例子可以查看Context内容。

#### 二、useReducer中，使用父组件state带来的性能问题
改在context的例子，我们在ComponentB组件引入ComponentC，ComponentC中会使用到父组件state里一个新的属性count。
```
import ComponentC from './ComponentC'
function ComponentB(props) {
    // 子组件触发dispatch，父组件reducer里计算得到新的值，更新组件（省略了useState里的计算）。
    const { state, dispatch } = useContext(ThemeContext)
    return (
        <div>
            孙组件获取传递的值：{state.value}
            <button onClick={() => {dispatch({ type: 'add' })}}>add</button>
            <button onClick={() => {dispatch({ type: 'reduce' })}}>reduce</button>
            <ComponentC />
        </div>
    )
}

function ComponentC(props) {
    console.log('----ComponentC render-----')
    const { state } = useContext(ThemeContext)
    return (
        <div>
            无需渲染的组件： {state.count}
        </div>
    )
}
```
上述代码我们在点击按钮时，都会导致ComponentC的render。

为了让ComponentC不进行不必要的渲染，我们有以下思路：
1. ComponentC使用React.memo()包装：
```
function ComponentC(props) {
    console.log('----ComponentC render-----')
    const { state } = useContext(ThemeContext)
    return (
        <div>
            无需渲染的组件： {state.count}
        </div>
    )
}

export default React.memo(ComponentC)
```
上述代码还是无法解决ComponentC的重新渲染的问题，原因是 **React.memo 只会对 props 进行浅比较，而通过 Context 我们直接将 state 注入到了组件内部，因此 state 的变化必然会触发 re-render，整个 state 变化是绕过了 memo**。React.memo()第二个参数进行深比较是需要获取props的，这里不适用。

2. **使用 useMemo() 解决 state Context 透传的性能问题**
```
function ComponentC(props) {
    console.log('----ComponentC render-----')
    const { state } = useContext(ThemeContext)
    return useMemo(() => {
        console.log('----ComponentC 局部 render-----')
        return (
            <div>
                无需渲染的组件： {state.count}
            </div>
        )
    }, [state.count])
}
```

