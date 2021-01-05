### 一、memo
在 class component 时代，为了性能优化我们经常会选择使用 PureComponent,每次对 props 进行一次浅比较，当然，除了 PureComponent 外，我们还可以在 shouldComponentUpdate 中进行更深层次的控制。

在 Function Component 的使用中， **React 贴心的提供了 `React.memo` 这个 HOC（高阶组件），与 PureComponent 很相似**，但是是专门给 Function Component 提供的，对 Class Component 并不适用。

但是相比于 PureComponent ，React.memo() 可以支持指定一个参数，可以相当于 shouldComponentUpdate 的作用，因此 React.memo() 相对于 PureComponent 来说，用法更加方便。
```
function MyComponent(props) {
  /* render using props */
}
function areEqual(prevProps, nextProps) {
  /*
  return true if passing nextProps to render would return
  the same result as passing prevProps to render,
  otherwise return false
  */
}
export default React.memo(MyComponent, areEqual);
```

使用方式很简单，在 Function Component 之外，在声明一个 areEqual 方法来判断两次 props 有什么不同，如果第二个参数不传递，则默认只会进行 props 的浅比较。

### 二、useMemo()
React.memo() 的使用我们可以发现，最终都是在最外层包装了整个组件，并且需要手动写一个方法比较那些具体的 props 不相同才进行 re-render。

而在某些场景下，我们只是希望 component 的部分不要进行 re-render，而不是整个 component 不要 re-render，也就是要实现 局部 Pure 功能。


把“创建”函数和依赖项数组作为参数传入 useMemo，它仅会在某个依赖项改变时才重新计算 memoized 值。这种优化有助于避免在每次渲染时都进行高开销的计算。**和vue的computed属性类似**

基本用法如下：
```
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```
举例：
```
import React, { useMemo } from 'react';

export default (props = {}) => {
    console.log(`--- component re-render ---`);
    return useMemo(() => {
        console.log(`--- useMemo re-render ---`);
        return <div>
            {/* <p>step is : {props.step}</p> */}
            {/* <p>count is : {props.count}</p> */}
            <p>number is : {props.number}</p>
        </div>
    }, [props.number]);
}
```
注意：
1. useMemo() 是在 render 期间执行的，所以不能进行一些额外的副操作，比如网络请求等。
2. 如果没有提供依赖数组（上面的 [a,b]）则每次都会重新计算 memoized 值，也就会 re-redner