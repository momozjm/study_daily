### 一、useMemo()
React.memo() 的使用我们可以发现，最终都是在最外层包装了整个组件，并且需要手动写一个方法比较那些具体的 props 不相同才进行 re-render。

而在某些场景下，我们只是希望 component 的部分不要进行 re-render，而不是整个 component 不要 re-render，也就是要实现 **局部 Pure 功能**。


把“创建”函数和依赖项数组作为参数传入 useMemo，它仅会在某个依赖项改变时才重新计算 memoized 值。这种优化有助于避免在每次渲染时都进行高开销的计算。**和vue的computed属性类似**

基本用法如下：
```
const memoizedValue = useMemo(() => computeExpensiveValue(a, b), [a, b]);
```
举例：改造React.Memo的例子，渲染子组件的局部。
```
import { useMemo } from 'react'

function childComponent(props) {
    console.log('----child render-----')
    return useMemo(() => {
        console.log(`--- useMemo re-render ---`);
        return <h1>测试memo{props.count.value}</h1>
    }, [props.count.value]);
}

export default childComponent
```
注意：
1. useMemo() 是在 render 期间执行的，所以不能进行一些额外的副操作，比如网络请求等。
2. 如果没有提供依赖数组（上面的 [a,b]）则每次都会重新计算 memoized 值，也就会 re-redner；提供的数组是空数组，只会渲染一遍，不管props里的值有没有变；**与useEffect第二个参数的用法一致**