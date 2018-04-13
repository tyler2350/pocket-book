# Pocket Book

FishPlusOrange's pocket book. Just for recording.

Good good study, day day up.

## Table of Contents

### About Vue

关于 Vue。

<details>

<summary>View contents</summary>

- [Vuex 使用 commit 提交 mutation 修改 state 的原因](#vuex-使用-commit-提交-mutation-修改-state-的原因)

</details>

### About Browser Compatibility

关于浏览器兼容性。

<details>

<summary>View contents</summary>

- [低版本 IE 浏览器下绝对定位的元素未设置背景时无法响应鼠标点击或悬浮事件](#低版本-ie-浏览器下绝对定位的元素未设置背景时无法响应鼠标点击或悬浮事件)
- [IE 浏览器下 iframe 弹窗中输入框光标丢失（无法输入）问题](#ie-浏览器下-iframe-弹窗中输入框光标丢失无法输入问题)

</details>

## Contents

### About Vue

#### Vuex 使用 commit 提交 mutation 修改 state 的原因

首先，[Vuex 官方文档](https://vuex.vuejs.org/zh-cn/mutations.html)表示：

> 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。

但是在开发过程中发现直接修改 state 也是可行的，所以二者有什么区别呢。

Vuex 提供**严格模式**，查看源码发现，`commit` 函数内部通过 `_withCommit` 函数和 `_withCommit` 函数内部的 `_committing` 状态值来修改 state。而严格模式下由 `enableStrictMode` 函数通过观察 `_committing` 状态值，如果非 true 则调用 `assert` 函数抛出错误 **“Do not mutate vuex store state outside mutation handlers”**。

所以如果不使用 commit 提交 mutation 修改 state，那么在严格模式下就会报错。更重要是，[Vuex 官方文档](https://vuex.vuejs.org/zh-cn/strict.html)还表示：

> 在严格模式下，无论何时发生了状态变更且不是由 mutation 函数引起的，将会抛出错误。这能保证所有的状态变更都能被调试工具跟踪到。

**故直接修改 state 所发生的状态变更无法被 Vue 调试工具 vue-devtools 跟踪到**。

[Back to TOC](#table-of-contents)

### About Browser Compatibility

#### 低版本 IE 浏览器下绝对定位的元素未设置背景时无法响应鼠标点击或悬浮事件

推荐的解决方案是通过设置 background 属性 hack：

```css
div {
    background: url(about:blank);
}
```

其他解决方案：

- 不使用绝对定位
 
- 添加背景色
 
- 添加透明背景图片

[Back to TOC](#table-of-contents)

#### IE 浏览器下 iframe 弹窗中输入框光标丢失（无法输入）问题

在开发过程中遇到这样一个问题，IE 浏览器下弹出一个 iframe 弹窗，第一次可以正常获得输入框焦点，关闭弹窗后再打开（未刷新页面），无法获得输入框焦点，即无法输入。

网上查到一种方法，通过 IE 特有的方法 `CollectGarbage()` 来回收内存，经尝试问题依然没有得到解决。

个人理解造成该问题的主要原因是，IE 浏览器下在关闭（移除）弹窗（iframe 外层容器）时，iframe 并没有从 DOM 中移除，导致再打开弹窗时 DOM 冲突。经研究，主要有两种解决方案：

1. 在移除弹窗时，先移除 iframe：

```javascript
// window.parent.document 表示跨 iframe 操作父页面元素
$('iframe', window.parent.document).remove();
```

2. 每次打开弹窗时，手动获得输入框焦点：

```javascript
$('input').focus();
```

本人解决该问题时采用方案二，因为在实际解决过程中发现方案一在 IE10 版本下（非仿真模式）不可行，所以推荐**方案二**。

[Back to TOC](#table-of-contents)
