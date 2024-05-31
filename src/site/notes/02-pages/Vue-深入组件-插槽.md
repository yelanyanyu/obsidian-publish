---
{"dg-publish":true,"permalink":"/02-pages/Vue-深入组件-插槽/","tags":["personal/blog","program/frontend/vue"]}
---

# 简介
**核心问题：如何在子组件中引用父组件的内容？** 通过 vue 的 slot（插槽）机制实现。

插槽的作用就是替换元素，所谓插槽就是规定元素替换的位置（**替换到哪里？**）。例如，有一个名为 FancyButton 的组件：

```js
<button class="fancy-btn">
  <slot></slot> <!-- 插槽出口 -->
</button>
```

```js
<FancyButton>
  Click me! <!-- 插槽内容 -->
</FancyButton>
```
我们想要将 “Click me!”传到 FancyButton 的组件内部，也就是将 `<slot></slot>` 替换成 `Click me!`。如下图：
![image.png|600](https://yelanyanyu-img-bed.oss-cn-hangzhou.aliyuncs.com/img/blog/2024/02/20240204224012.png)
我们在组件中定义了插槽，也就是 slot，其规定了内容替换的位置。

# 具名插槽

