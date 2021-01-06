### useCallback

#### 一、 问题引发
1. 子组件onChange调用了父组件的handleOnChange
2. 父组件handleOnChange内部会执行setText(e.target.value)引起父组件更新
3. 父组件更新会得到新的handleOnChange，传递给子组件，对于子组件来说接收到一个新的props
4. 子组件进行不必要更新

简而言之：子组件调用父组件的方法，父组件更新会得到新的方法，导致子组件进行了不必要的更新。

####  二、使用useCallback解决
```
import React, { useState, memo, useMemo, useCallback } from 'react'

const Child = memo((props) => {
  console.log(props);

  return (
    <div>
      <input type="text" onChange={props.onChange}/>
    </div>
  )
})

const Parent = () => {
  const [count, setCount] = useState(0)
  const [text, setText] = useState('')

  const handleOnChange = useCallback((e) => {
    setText(e.target.value)
  },[])

  return (
    <div>
      <div>count: {count}</div>
      <div>text: {text}</div>
      <button onClick={() => {
        setCount(count + 1)
      }}>+1</button>
      <Child onChange={handleOnChange} />
    </div>
  )
}

function App() {
  return <div><Parent /></div>
}

export default App
```

#### 三、总结
1. handleOnChange被缓存了下来，尽管父组件更新了，但是拿到的handleOnChange还是同一个
2. 对比useMemo，useMemo缓存的是一个值，useCallback缓存的是一个函数，是对一个单独的props值进行缓存
3. memo缓存的是组件本身，是站在全局的角度进行优化
4. `useCallback(fn, deps)` 相当于 `useMemo(() => fn, deps)`
