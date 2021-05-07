---
title: 前端处理后端返回文件流实现导出Excel
date: 2021-05-07 17:04:22
---
实现功能：

> 前端发送请求后，接收后端返回的文件流（一般是乱码），实现导出Excel的方法。



js代码(**封装的promise对象**)：

```javascript
import Vue from 'vue'
import notification from 'ant-design-vue/es/notification'

// 实例化Vue对象
const vm = new Vue()

/**
 * 前端处理后端返回文件流实现导出Excel的方法
 *
 * @export
 * @param {String} url 接口地址
 * @param {Object} params 传递参数
 * @param {String} fileName 文件名称
 * @return {Promise} Promise对象
 */
export function getExcelExport (url, params, fileName) {
  return new Promise((resolve, reject) => {
    vm.$http.post(url, params, { responseType: 'blob' }).then(res => {
      const link = document.createElement('a')
      const blob = new Blob([res], { type: 'application/vnd.ms-excel' })
      link.style.display = 'none'
      link.href = URL.createObjectURL(blob)
      link.setAttribute('download', `${fileName || '导出文件'}.xls`)
      document.body.appendChild(link)
      link.click()
      document.body.removeChild(link)
      resolve({ message: '导出成功' })
    }).catch(err => {
      notification.error({
        message: '错误',
        description: '导出接口出错'
      })
      reject(err)
    })
  })
}
```





*如有错误，请多指教，谢谢！*
