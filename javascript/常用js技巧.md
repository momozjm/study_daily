## 常用的JavaScript简写技巧

### 1. 声明变量
```
//Longhand 
let x; 
let y = 20; 
//Shorthand 
let x, y = 20;
```

### 2. 给多个变量赋值
```
//Longhand 
let a, b, c; 
a = 5; 
b = 8; 
c = 12;

//Shorthand 
let [a, b, c] = [5, 8, 12];
```

### 3. 交换两个变量
为了交换两个变量，我们通常使用第三个变量。我们可以使用数组解构赋值来交换两个变量。
```
let x = 'Hello', y = 55; 
//Longhand 
const temp = x; 
x = y; 
y = temp; 
//Shorthand 
[x, y] = [y, x];
```

### 4. 多条件检查
对于多个值匹配，我们可以将所有的值放到数组中，然后使用indexOf()或includes()方法。
```
//Longhand 
if (value === 1 || value === 'one' || value === 2 || value === 'two') { 
     // Execute some code 
} 
// Shorthand 1
if ([1, 'one', 2, 'two'].indexOf(value) >= 0) { 
    // Execute some code 
}
// Shorthand 2
if ([1, 'one', 2, 'two'].includes(value)) { 
    // Execute some code 
}
```

### 5. 找出数组中的最大和最小数字
```
// Shorthand 
const arr = [2, 8, 15, 4]; 
Math.max(...arr); // 15 
Math.min(...arr); // 2
```

### 6. 获取字符串中的字符
```
let str = 'jscurious.com'; 
//Longhand 
str.charAt(2); // c 
//Shorthand 
str[2]; // c
```