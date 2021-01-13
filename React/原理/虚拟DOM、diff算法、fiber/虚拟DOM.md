## 虚拟DOM

### 一、关于DOM的谣言
DOM操作慢？虚拟DOM快？

- 这句话类比于刘翔矮（对比于姚明）。 
- DOM操作慢是对比于js原生api，如操作数组。
- 任何基于DOM的库（react/vue）都不可能在操作DOM时比DOM自身快。

### 二、虚拟DOM的优点

**减少DOM的操作**：

- 虚拟DOM可以将多次操作合并为一次操作
- 虚拟DOM借助DOM diff可以把多余的dom操作节省掉。

**跨平台**
- 虚拟DOM是抽象的真实DOM，可以转换为APP（RN），小程序等。


### 三、虚拟DOM的缺点
需要额外的创建函数，如createElement和h，但react可通过jsx来简化成xml语法。
- react可以用babel转义成createElement形式。
- vue用vue-loader解析，不能用babel，因为vue是.vue结尾的文件，babel只能解析js文件。

### 四、如何创建虚拟DOM
**React.createElement()**

react内部使用jsx(React.createElement()语法糖)创建虚拟dom，但是这样写法太麻烦，所以我们在页面编写XML后，由babel编译为React.createElement(...)。

![image](747A353F6FC54559887CDFBBE32DA084)

XML - babel编译为React.createElement(...) - 虚拟DOM - 真实DOM

**Vue(只能在render函数里得到h函数)**

![image](E4CDB43EFBCF454C9251FA47079020C1)

