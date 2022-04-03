A project from [JavaScirpt 30 -- Day 10](https://github.com/wesbos/JavaScript30/tree/master/10%20-%20Hold%20Shift%20and%20Check%20Checkboxes)

Hold Shift to Check Multiple Checkboxes

实现按住 shift 勾选或取消勾选多个复选框的功能

index-orginal 是下载的项目原始实现方式，存在两个问题：

- 当取消勾选一个复选框，按住 shift, 再勾选其他复选框时，依然会把两者之间所有复选框勾选
- 无法通过按住 shift 同时取消勾选多个复选框

我在原方案基础上做了改进，将

```js
// 检查shift键是否按下，并且用户是在勾选当前复选框
if (e.shiftKey && this.checked) {
  checkboxes.forEach((checkbox) => {
    console.log(checkbox);
    if (checkbox === this || checkbox === lastChecked) {
      inBetween = !inBetween;
      console.log("Starting to check them in between!");
    }
    if (inBetween) {
      checkbox.checked = true;
    }
  });
}
```

改为

```js
// 检查是否按下shift键, 按下时 e.shiftKey=true
if (e.shiftKey) {
  // 从第一个循环到最后一个复选框
  // 遇到lastChecked后开始勾选 -- inBetween 从 false变为 true
  // 遇到当前复选框后取消勾选 -- inBetween 从 true 变为 false
  checkboxes.forEach((checkbox) => {
    if (checkbox === lastChecked || checkbox === this) {
      inBetween = !inBetween;
    }
    if (inBetween) {
      // 如果这次是勾选复选框，上次也是勾选，则勾选中间所有复选框
      if (this.checked && lastChecked.checked) checkbox.checked = true;
      // 如果这次是取消勾选复选框，这次也是取消勾选，则取消勾选中间所有复
      if (!this.checked && !lastChecked.checked) checkbox.checked = false;
      // 如果上次与这次行为不一致，则什么也不做
    }
  });
}
```

解决了上述问题，能够同时勾选或同时取消勾选多个复选框，并且只有在连续两次点击行为一致时才对中间所有复选框进行相同的操作。
