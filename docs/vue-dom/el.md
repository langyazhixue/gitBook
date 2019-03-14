### 谈谈el
  * 类型: `string| Element`
  * 限制: 只在由 `new`创建的实例中遵守
  * 详细:

  `el` 是提供一个在页面上已存在的`DOM`元素作为`Vue`实例的挂载目标，可以是css选择器，页可以是一个`HTMLElement` 实例

  在实例挂载之后，元素可以用 `vm.$el` 访问。

  如果在实例化时候存在这个选项，实例将立即进入编译过程，否则需要调用`vm.$mount()`手动开启编译

  >提供的元素只能作为挂载点，不同于`Vue 1.x`,所有的挂载元素会被`Vue`生成 的 DOM 替换。因此不推荐挂载root实例到 `<html>或者<body>`

