---
{"dg-publish":true,"permalink":"/02-pages/vue3 定时更新数据实践/","tags":["personal/blog","program/frontend/vue3","program/backend/framework/jeesite"]}
---

# 基本步骤
1. 定义请求的 api；


# 注意事项
```ts
function startTimer() {
  if (!intervalId) {
    intervalId = window.setInterval(() => {
      handleLedDataQuery();
    }, 5000);
  }
}

```

我们最好在使用方法 `setInterval` 的时候，加上 window，将其注册到全局，不然可能会出现计时器无法清除的问题。
