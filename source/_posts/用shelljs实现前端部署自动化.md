---
title: 用shelljs实现前端部署自动化
date: 2018-08-17 14:28:03
tags: [前端自动化, Javascript]
---

# 一、场景

在前端开发过程中，你肯定经常使用到以下等命令：

```
npm run build
git add .
git commit -m 'commit'
git push
```

<!-- more -->

本人在用`vue-cli`写我的个人博客时，将其部署到`coding pages`上。不用`github pages`的原因纯粹是因为慢。。。每一次部署，都要将上面的命令敲一遍，实在令我很痛苦。如果能用一条命令执行以上所有任务，那就爽多了。

话不多说，说干就干。

# 二、[Shelljs](https://github.com/shelljs/shelljs)

这个库能够让我们在`js`文件中执行 shell 命令，具体可以看[文档](https://github.com/shelljs/shelljs)。

### 安装

```
npm install [-g] shelljs
```

有两种使用方式，一种是全局模式(对应全局安装),一种是局部模式。看下面的使用案例就知道两者区别。

# 三、使用

在根目录下新建文件`shell.js`，内容如下：

```
//局部模式
var shell = require('shelljs');
//全局模式下，就不需要用shell开头了。
//require('shelljs/global');

if (shell.exec('npm run build').code !== 0) {//执行npm run build 命令
  shell.echo('Error: Git commit failed');
  shell.exit(1);
}

//由于我的用另外一个仓库存放dist目录，所以这里要将文件增量复制到目标目录。并切换到对应目录。
shell.cp ('-r', './dist/*', '../../Rychou');
shell.cd('../../Rychou');

shell.exec('git add .');
shell.exec("git commit -m 'autocommit'")
shell.exec('git push')

```

这时在根目录下执行`node shell.js`就可以了

这里只是最简单的使用案例。

# 四、再让它更方便些

在`package.json`中加入：

```
"script":{
+	"push":"node ./shell.js"
}
```

在根目录下执行`npm run push`就搞定了。

> 参考链接：[Shelljs](https://github.com/shelljs/shelljs)
