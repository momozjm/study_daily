### Context
---
#### 一、何时使用 Context
在一个典型的 React 应用中，**数据是通过 props 属性自上而下（由父及子）进行传递的**，但这种做法对于某些类型的属性而言是极其繁琐的（例如：地区偏好，UI 主题），这些属性是应用程序中许多组件都需要的。Context 提供了一种在组件之间共享此类值的方式，而不必显式地通过组件树的逐层传递 props。

举例：
```
function App() {
    const [value, setvalue] = useState(1)
    const addValue = () => {
        setvalue(value + 1)
    }
    return (
        <div>
            <ComponentA addValue={addValue} value={value} />
        </div>
    )
}

function componentA(props){
    return (
        <div>
            <ComponentB addValue={props.addValue} value={props.value} />
        </div>
    )
}

function componentB(props){
    return (
        <div>
            孙组件获取传递的值：{props.value}
            <button onClick={props.addValue}>add</button>
        </div>
    )
}
```
上面的代码中，最底层组件想使用顶层组件的属性或者方法，需要一层一层传递。我们可以用Context让所有子组件都可以共享到顶层组件的属性或者方法。
```
import ThemeContext from './components/ThemeContext'
function App() {
    const [value, setvalue] = useState(1)
    const addValue = () => {
        setvalue(value + 1)
    }
    const params = {
        value,
        addValue
    }
    return (
        <div>
            <ThemeContext.Provider value={params}>
                <ComponentA />
            </ThemeContext.Provider>
        </div>
    )
}

import ThemeContext from './ThemeContext'
function ComponentB(props) {
    const { value, addValue } = useContext(ThemeContext)
    return (
        <div>
            孙组件获取传递的值：{value}
            <button onClick={addValue}>add</button>
        </div>
    )
}
```
新建`ThemeContext.js`。
```
import React from 'react';
const ThemeContext = React.createContext({});
export default ThemeContext
```

#### 二、使用 Context 之前的考虑
Context 主要应用场景在于很多不同层级的组件需要访问同样一些的数据。请谨慎使用，因为**这会使得组件的复用性变差**。

上面的示例虽然实现了多级组件方法共享，但是暴露出一个问题：所有的方法和属性都放在了 Context.Provider.value 属性中传递，必然造成整个 Context Provider 提供的方法越来越多，也会臃肿。我们可以通过 `useReducer` 包装。
```

const reducer = (state, action) => {
    switch (action.type) {
        case 'add': return Object.assign({}, state, { value: state.value + 1 });
        case 'reduce': return Object.assign({}, state, { value: state.value - 1 });
        default: return state;
    }
}

function App() {
    // useState里的操作放在了reducer里。
    const [state, dispatch] = useReducer(reducer, {value: 0})
    const params = {
        state,
        dispatch
    }
    return (
        <div>
            <ThemeContext.Provider value={params}>
                <ComponentA />
            </ThemeContext.Provider>
        </div>
    )
}

function ComponentB(props) {
    // 子组件触发dispatch，父组件reducer里计算得到新的值，更新组件（省略了useState里的计算）。
    const { state, dispatch } = useContext(ThemeContext)
    return (
        <div>
            孙组件获取传递的值：{state.value}
            <button onClick={() => {dispatch({ type: 'add' })}}>add</button>
            <button onClick={() => {dispatch({ type: 'reduce' })}}>reduce</button>
        </div>
    )
}
```

#### 三、替代方案
1. 如果你只是想避免层层传递一些属性，组件组合（component composition）有时候是一个比 context 更好的解决方案。
2. 一种无需 context 的解决方案是将 组件自身传递下去

#### 四、context的使用场景
使用 context 的通用的场景包括管理当前的 locale，theme，或者一些缓存数据，这比替代方案要简单的多。