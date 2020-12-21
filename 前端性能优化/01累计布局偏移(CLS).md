### 一：概念
CLS，全称是 Cumulative Layout Shift，中文名是累计布局偏移，是 Google Search Console 额一个核心网页指标，是指网页布局在加载期间的偏移量。得分范围是 0–1，其中 0 表示没有偏移，1 表示最大偏移。
简称**页面视觉稳定性**。

### 二：为什么 CLS 重要
当页面在加载过程中，用户想与页面有交互（如点击按钮），但其实网页还没有加载好，用户本想点击按钮A，可点到了按钮B上。

### 三：造成 CLS 问题的原因
1. images without dimensions：未指定尺寸的图片
2. Ads, embeds, and iframes without dimensions：未指定尺寸的广告、嵌入元素、iframe
3. Dynamically injected content：动态注入内容
4. Web Fonts causing FOIT/FOUT：自定义字体（引发FOIT/FOUT）
5. Actions waiting for a network response before updating DOM：动画

### 四：如何查看CLS
在performce中，如果有红色`Layout Shift`，说明需要对页面视觉稳定性进行优化

### 五：总结
1. 图片的尺寸，以及其他嵌入元素的尺寸，最开始就设定好，或者预留足够空间，这样可以有效避免布局偏移。
2. 利用图片宽高比的属性，可以在优化CLS的同时，做响应式布局。
3. 尽可能不要往已存在内容上方添加新内容。
4. web字体尽可能早的加载，避免产生FOIT和FOUT
5. 尽可能的在网络请求时，给一个loading，或者占位符提示，避免用户在这段时间内进行操作。
6. 与UI同事配合在交互上避免布局偏移

### 六：参考链接
[https://web.dev/optimize-cls/](https://web.dev/optimize-cls/)

[https://blog.csdn.net/wuchen092832/article/details/108313270](https://blog.csdn.net/wuchen092832/article/details/108313270)