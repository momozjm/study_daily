### 自定义hook
通过自定义 Hook，可以将组件逻辑提取到可重用的函数中。

自定义 Hook 是一个函数，其名称以 “use” 开头，函数内部可以调用其他的 Hook。

自定义 Hook 是一种自然遵循 Hook 设计的约定，而并不是 React 的特性。

自定义 Hook 必须以 “use” 开头。

在两个组件中使用相同的 Hook ，所有 state 和副作用都是完全隔离的。

举例：两个组件根据传入状态显示在线或者离线：
```
  function useStatus(status) {
    const [isOnline, setIsOnline] = useState(null);
  
    useEffect(() => {
      switch (status) {
        case 'online':
          setIsOnline('在线')
          break;
      
        default:
          setIsOnline('离线')
          break;
      }
    });
  
    return isOnline;
  }

  const FunA = (status) => {
    const status_text = useStatus(status)
    return (
      <h1>状态A:{status_text}</h1>
      // <h1>状态A:{useStatus(status)}</h1>  直接使用自定义hooks表达式会报错
    )
  }

  const FunB = (status) => {
    const status_text = useStatus(status)
    return (
      <h2>状态B:{status_text}</h2>
    )
  }
```
