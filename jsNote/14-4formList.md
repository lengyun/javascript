## 表单序列化

随着 Ajax 的出现，表单序列化已经成为一种常见需求（第 21 章将讨论 Ajax）。在JavaScript 中，可以利用表单字段的 type 属性，连同 name 和 value 属性一起实现对表单的序列化。在编写代码之前，有必须先搞清楚在表单提交期间，浏览器是怎样将数据发送给服务器的。

* 对表单字段的名称和值进行 URL 编码，使用和号（&）分隔。
* 不发送禁用的表单字段。
* 只发送勾选的复选框和单选按钮。
* 不发送 type 为"reset"和"button"的按钮。
* 多选选择框中的每个选中的值单独一个条目。
* 在单击提交按钮提交表单的情况下，也会发送提交按钮；否则，不发送提交按钮。也包括 type为"image"的<input>元素。
* <select>元素的值，就是选中的<option>元素的 value 特性的值。如果<option>元素没有value 特性，则是<option>元素的文本值。

```js
function serialize(form) {
  var parts = [],
    field = null,
    i,
    len,
    j,
    optLen,
    option,
    optValue;
  for (i = 0, len = form.elements.length; i < len; i++) {
    field = form.elements[i];
    switch (field.type) {
      case "select-one":
      case "select-multiple":
        if (field.name.length) {
          for (j = 0, optLen = field.options.length; j < optLen; j++) {
            option = field.options[j];
            if (option.selected) {
              optValue = "";
              if (option.hasAttribute) {
                optValue = (option.hasAttribute("value") ? option.value : option.text);
              } else {
                optValue = (option.attributes["value"].specified ? option.value : option.text);
              }
              parts.push(encodeURIComponent(field.name) + "=" + encodeURIComponent(optValue));
            }
          }
        }
        break;
      case undefined: //fieldset
      case "file": //file input
      case "submit": //submit button
      case "reset": //reset button
      case "button": //custom button
        break;
      case "radio": //radio button
      case "checkbox": //checkbox
        if (!field.checked) {
          break;
        }
        /* falls through */
      default:
        //don't include form fields without names
        if (field.name.length) {
          parts.push(encodeURIComponent(field.name) + "=" + encodeURIComponent(field.value));
        }
    }
  }
  return parts.join("&");
}

var btn = document.getElementById("serialize-btn");
EventUtil.addHandler(btn, "click", function(event) {
  var form = document.forms[0];
  alert(serialize(form));
});
```

