## 代码组织

代码按如下形式按顺序组织：

1. `@import`
2. 变量声明
3. 样式声明

```css
// good
@import "est/all.less";

@default-text-color: #333;

.page {
    width: 960px;
    margin: 0 auto;
}
```

***

## `@import` 语句

`@import` 语句引用的文件必须写在一对引号内，`.less` 后缀不得省略。引号使用 `'` 和 `"` 均可，但在同一项目内必须统一。

```css
// bad
@import 'est/all';
@import "my/mixins.less";
```
```css
// good
@import "est/all.less";
@import "my/mixins.less";
```

***

## 空格

### 混入（Mixin）

Mixin 和后面的空格之间不得包含空格。在给 mixin 传递参数时，在参数分隔符（`,` / `;`）后必须保留一个空格：

```css
// bad
.box {
    .size(30px,20px);
    .clearfix ();
}
```
```css
// good
.box {
    .size(30px, 20px);
    .clearfix();
}
```

***

## 嵌套和缩进


嵌套的声明块前必须增加一次缩进，有多个声明块共享命名空间时尽量嵌套书写，避免选择器的重复。

但是需注意的是，尽量仅在必须区分上下文时才引入嵌套关系。

```css
// bad
.main .title {
  font-weight: 700;
}

.main .content {
  line-height: 1.5;
}

.main {
.warning {
  font-weight: 700;
}
    
  .comment-form {
    #comment:invalid {
      color: red;
    }
  }
}
```
```css
// good
.main {
    .title {
        font-weight: 700;
    }

    .content {
        line-height: 1.5;
    }
    
    .warning {
        font-weight: 700;
    }
}

#comment:invalid {
    color: red;
}
```

***

## 变量

Less 的变量值总是以同一作用域下最后一个同名变量为准，务必注意后面的设定会覆盖所有之前的设定。

变量命名必须采用 `@foo-bar` 形式，不得使用 `@fooBar` 形式。

```css
// bad
@sidebarWidth: 200px;
@width:800px;
```
```css
// good
@sidebar-width: 200px;
@width: 800px;
```

***

## 继承

使用继承时，如果在声明块内书写 `:extend` 语句，必须写在开头：

```css
// bad
.sub {
    color: red;
    &:extend(.mod all);
}
```
```css
// good
.sub {
    &:extend(.mod all);
    color: red;
}
```

***

## 混入（Mixin）

在定义 mixin 时，如果 mixin 名称不是一个需要使用的 className，必须加上括号，否则即使不被调用也会输出到 CSS 中。

```css
// bad
.big-text {
    font-size: 2em;
}

h3 {
    .big-text;
}
```
```css
// good
.big-text() {
    font-size: 2em;
}

h3 {
    .big-text();
}
```

如果混入的是本身不输出内容的 mixin，必须在 mixin 后添加括号（即使不传参数），以区分这是否是一个 className（修改以后是否会影响 HTML）。

```css
// bad
.box {
    .clearfix;
    .size (20px);
}
```
```css
// good
.box {
    .clearfix();
    .size(20px);
}
```

Mixin 的参数分隔符使用 `,` 和 `;` 均可，但在同一项目中必须保持统一。

***

## 命名空间

变量和 mixin 在命名时必须遵循如下原则：

* 一个项目只能引入一个无命名前缀的基础样式库（如 est）
* 业务代码和其他被引入的样式代码中，变量和 mixin 必须有项目或库的前缀

***

## 字符串

在进行字符串转义时，使用 `~""` 表达式与 `e()` 函数均可，但在同一项目中必须保持一致。

字符串两侧的引号必须使用 `"`。

## JS 表达式

可以使用 JS 表达式（<code>~\`\`</code>）生成属性值或变量，其中包含的字符串两侧的引号尽量使用单引号（`'`）。

***

## 注释

单行注释尽量使用 `//` 方式。

```css
// Hide everything
* {
    display: none;
}
```