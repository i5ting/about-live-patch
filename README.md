# about-live-patch


## live-patch

https://github.com/substack/live-patch


![](demo.gif)

## example

First whip up a patch.js to monitor a program.js file for changes:

实现的功能是代码变了，输入结果就变了，让已执行的文件的上下文也跟着变动，自动写入到某个文件中

```
var fs = require('fs');
var gaze = require('gaze');
var file = __dirname + '/program.js';

var patch = require('live-patch')();

gaze(file, function (err, w) {
    w.on('changed', function (p) { read() });
});
read();

function read () {
    fs.readFile(file, 'utf8', function (err, src) {
        if (err) return console.error(err);
        patch.update(src);
    });
}
```

## 我们能学到什么

思路挺有意思，监测文件变动，然后patch

看package.json

```
"dependencies": {
  "falafel": "^1.0.0",
  "gaze": "^0.5.1",
  "has": "^1.0.0",
  "minimist": "^1.1.0",
  "patcher": "^0.0.6",
  "scoper": "^1.0.0"
}
```


- gaze是监测变动的比较好的库，跨平台
- falafel：Transform the ast on a recursive walk.
- scoper：modify nested scope at runtime
- patcher：Object patching and replication for JavaScript
- has和minimist是工具模块很简单

## gaze


A globbing fs.watch wrapper built from the best parts of other fine watch libs.
Compatible with Node.js 0.10/0.8, Windows, OSX and Linux.

https://github.com/shama/gaze


## node-falafel

https://github.com/substack/node-falafel

核心ast parser是https://www.npmjs.com/package/acorn

## acorn

A tiny, fast JavaScript parser, written completely in JavaScript.


## has

Object.prototype.hasOwnProperty.call shortcut

https://www.npmjs.com/package/has

## scoper

modify nested scope at runtime

https://www.npmjs.com/package/scoper

## patcher

Object patching and replication for JavaScript

https://www.npmjs.com/package/patcher


## 总结

- gaze是监测变动的比较好的库，跨平台
- 需要转换ast，使用falafel
- 运行时修改嵌套的scope使用scoper
- 如何给js对象打补丁、替换呢？答案使用patcher

思路

- 文件变了，通过gaze能知道谁变了，变了哪些内容
- 从执行上下文和该文件知道ast，知道变动了那些
- 剩下的就是打补丁了，应用变化
