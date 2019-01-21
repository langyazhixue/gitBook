最近项目中遇到一个问题，让前端自己做excel表格的导出，我表示以前项目这个功能都是后端做的。so 我决定研究一下！！
最后通过度娘，解决了这个问题！！！引入2个库 `js-xlsx` 和  `file-saver` 妥妥解决这个难题
直接上代码吧！！！

- JS-XLSX是一个简单的JavaScript插件，能够读取和写入Excel表格文件，并且能够支持最新版本的XLSX文件
- 关于FileSaver.js 是在没有原生支持saveAs()的浏览器上实现了saveAs()接口。有一个[FileSaver.js]()示例，演示如何保存各种媒体类型
FileSaver.js 是在客户端保存文件的解决方案，非常适合需要生成文件，或者保存不应该发送到外部服务器的敏感信息的 web App

```html
<template>
  <div id="app">
    <h2>这是一个导出excel表格的例子</h2>
    <table id="rebateSetTable" style="display: none">
      <tr>
        <td>日期</td>
        <td>外呼量</td>
        <td>接听量</td>
        <td>未接听</td>
        <td>接听率</td>
      </tr>
      <tr v-for="datas in dataPhone" :key='datas.id'>
        <td>{{datas.date}}</td>
        <td>{{datas.exhale}}</td>
        <td>{{datas.answer}}</td>
        <td>{{datas.noAnswer}}</td>
        <td>{{datas.rate}}</td>
      </tr>
    </table>
    <button @click='exportExcel'>导出excel表格</button>
  </div>
</template>
```
```js
import XLSX from 'xlsx'
import FileSaver from 'file-saver'
export default {
  data () {
    return {
      dataPhone: [{
        id: 1,
        date: 1,
        exhale: 2,
        answer: 3,
        noAnswer: 4,
        rate: 5
      }, {
        id: 2,
        date: 1,
        exhale: 2,
        answer: 3,
        noAnswer: 4,
        rate: 6
      }]
    }
  },
  methods: {
    exportExcel () { // 导出数据
      /* generate workbook object from table */
      let x = XLSX

      let wb = x.utils.table_to_book(document.querySelector('#rebateSetTable'))
      // table_to_book 把html中的table 生成 excel 表格所需要的数据对象
      /* get binary string as output */
      let wbout = x.write(wb, { bookType: 'xlsx', bookSST: true, type: 'array' })
      // wbout 是一个arrayBuffer
      try {
        FileSaver.saveAs(new Blob([wbout], { type: 'application/octet-stream' }), '数据统计表.xlsx')
        // application/octet-stream 指的二进制流的类型
      } catch (e) {
        if (typeof console !== 'undefined') {
          console.log(e, wbout)
        }
      }
      return wbout
    }
  }
}
```
保存结果如下图
![excel](./img/excel.jpg)

参考文章 
- [Vue 实现表格的导入导出功能](https://www.jianshu.com/p/7c2475b35460?utm_campaign=maleskine&utm_content=note&utm_medium=seo_notes&utm_source=recommendation)
- [js-xlsx读取Excel文件中文API帮助文档](http://www.uedsc.com/js-xlsx-api.html)
- [FileSaver.js 介绍](https://www.cnblogs.com/yunser/p/7629399.html)

