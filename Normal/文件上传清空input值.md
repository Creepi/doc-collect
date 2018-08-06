### 文件上传清空input值

js不可修改input file的值为只读属性 不可修改

解决方案：

在外部套一个form，通过对表单的重置来进行初始化file

```js
document.getElementById('formid')&&document.getElementById('formid').reset();

```

