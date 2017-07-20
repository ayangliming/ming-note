### 用 Markdown 来记笔记

Markdown 跟 HTML 一样，是一种标签语言。但是 Markdown 语法特别简单，适合用来做笔记。

[Markdown 语法参考](https://coding.net/help/doc/project/markdown.html)

Mardown 语法不是浏览器能直接支持的，所以需要先把 Mardown 语法写成的内容，编译成 HTML ，才能美观的显示出来。

在这个文件里面，我们去写 markdown ，就可以翻译成 html 。

###第一步，在全局安装

```js
npm install gitbook-cli -g
```

然后在本地创建一个笔记文件夹

```js
mkdir （文件夹名字）
```
然后在执行：

```js
cd （文件夹名字）
gitbook init
```
这样，可以生成两个文件：

-README.md 的内容会显示在书皮上
-SUMMARY.md 是目录

###启动服务器，查看和编辑书籍

>gitbook serve

这样，可以启动一个服务器，然后到 localhost:4000 端口，就可以看到这本书了。
可以修改 SUMMARY.md 来添加书籍目录

```js
# Summary

* [Introduction](README.md)
* 第一章
  - [第一小节：学习 Git](./git/1-hello.md)
  - [第二小节：Git 本地工作流](./git/2-local-git.md)
  - [第三小节：Github 基本操作](./git/3-github.md)
  ```
### 托管我的 gitbook

首先到 github.com 上创建 my-note 仓库。

为了部署方便，我们把我们的 (文件夹)的内容结构稍微调整一下，把原有的所有内容都放到 content 文件夹中，也就是有这样的目录结构

```js
➜  (文件夹) ls content
README.md  SUMMARY.md git
➜  (文件夹)
```

然后，把当然项目变成一个 nodejs 的项目：

```js
cd (文件夹)
npm init
```

然后，package.json 中添加这些代码：

```js
"scripts": {
 "start": "gitbook serve ./content ./gh-pages",
 "build": "gitbook build ./content ./gh-pages",
 "deploy": "node ./scripts/deploy-gh-pages.js",
 "publish": "npm run build && npm run deploy",
 "port": "lsof -i :35729"
},
```



有了上面的 npm 脚本之后，我们如果我想在本地 4000 端口查看本书，我需要运行

```js
npm start
```

在准备上传之前，先来创建一个 .gitignore 文件，里面填写

```js
gh-pages
_book
node-modules
```

然后，运行

```js
git init
git add -A
git commit -a -m"hello my book"
git remote add origin git@github.com:ayangliming/文件名.git
git push -u origin master
```

上面这些完成后，gitbook 的原始代码就被安全的备份到 master 分支了。访问 https://github.com/ayangliming/ming-note 可以看到这些内容。

###部署书籍到 gh-pages

我们采用一个 npm 包，来帮助我们完成操作

```js
cd 文件名
npm i --save gh-pages
```
然后创建 文件名/scripts/deploy-gh-pages.js

里面的内容是：

```js
'use strict';

var ghpages = require('gh-pages');

main();

function main() {
    ghpages.publish('./gh-pages', console.error.bind(console));
}
```
这样，每次书稿有了修改，线上传到 maste 分支。再切换到 gh-pages 分支运行

```js
npm run publish
```
就可以把书稿部署到 http://ayangliming.github.io/ming-note



### Markdown 语法

例如，我们在 README.md 中填写

```js
[百度](http://baidu.com)
<a href="http://baidu.com">百度</a>
```

提交保存之后，页面显示出完全一样的链接效果。

>注意，添加内容的文件名，无所谓，但是后缀一定要 .md 不然无法编译成功

### Mardown 中添加语法高亮

什么是语法高亮？ 如果一段代码没有语法高亮，那么就是所有的字符都显示成一个颜色。但是通常编辑器中有语法高亮，也就是不同语法作用的字符串会显示成不同的颜色。

markdown 中，如果写成下面这样，最终显示的效果就是有语法高亮的：   

```js
console.log('hello');
console.log('hello');

console.log('hello');

console.log('hello');
```

```css
body {
  background: red;
}
```

上面的内容会最终显示为：
```js
console.log('hello');
console.log('hello');

console.log('hello');

console.log('hello');
body {
  background: red;
}
```
