title: mac安装 hexo
date: 2017-08-19 17:13:59
category: 笔记
tags: hexo
---

在 Mac 系统上, 安装 hexo，本来很简单到命令，却蛋疼一下午，简单记录下。


### 安装homebrew


```
ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### 安装nodejs
```
brew install node

```
### 安装hexo

```
sudo npm install -g hexo-cli
```

### 异常

```
Error: EACCES: permission denied, rename '/usr/local/lib/node_modules/hexo-cli/node_modules/dtrace-provider/compile.py' -> '/usr/local/lib/node_modules/hexo-cli/node_modules/dtrace-provider/binding.gyp'
    at Object.fs.renameSync (fs.js:772:18)
    at Object.<anonymous> (/usr/local/lib/node_modules/hexo-cli/node_modules/dtrace-provider/scripts/install.js:14:4)
    at Module._compile (module.js:573:30)
    at Object.Module._extensions..js (module.js:584:10)
    at Module.load (module.js:507:32)
    at tryModuleLoad (module.js:470:12)
    at Function.Module._load (module.js:462:3)
    at Function.Module.runMain (module.js:609:10)
    at startup (bootstrap_node.js:158:16)
    at bootstrap_node.js:598:3

```

### 成功解决

```
运行npm config set unsafe-perm true以后，在运行npm install -g hexo-c
```