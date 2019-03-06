## Excel2JSON

在浏览器端将 `Excel` 文件解释成 `JSON` 格式数据。

### 项目依赖

- [SheetJS](https://github.com/SheetJS/js-xlsx) JS实现的Excel文件 Parser 及 Writer。
- [Plupload](https://www.plupload.com/) 跨浏览器上传组件封装。


### 安装

```bash
npm install mm-excel2json
```


### 使用

- Vue文件使用
```html
<template>
<excel2json @parsed="handleParsed"></excel2json>
</template>
<script>
import Excel2JSON from 'mm-excel2json';

export default {
  components: {
    "excel-json": Excel2JSON
  },
  methods: {
      handleParsed(args) {
          //your json data
          console.log(args);
      }
  }
}
</script>
```

- HTML使用
```html
<meta charset="utf-8">
<title>Excel2JSON demo</title>
<script src="https://unpkg.com/vue"></script>
<script src="./Excel2JSON.umd.js"></script>


<div id="app">
  <excel2json></excel2json>
</div>

<script>
new Vue({
  components: {
    excel2json: Excel2JSON
  },
  methods: {
      handleParsed(args) {
          //your json data
          console.log(args);
      }
  }
}).$mount('#app')
</script>
```

### 参数

组件带有两个处理事件
- `parsed` 事件，当成功读取Excel文件内容时发出，携带Excel内容的 JSON 数组。
- `error` 事件，当读取失败时抛出，携带`Error`内容。