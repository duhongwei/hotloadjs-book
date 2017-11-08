# 演练

演练放在了单独的 hotloadjs-cli 项目里。

## 环境
因为用了Koa，所以nodejs的版本要求高一些，其它方面还好

- nodejs >= 8.4 

## 步骤

### 安装 hotloadjs-cli
``` shell
npm install -g hotloadjs-cli
```
-g是全局安装，这样的好处是，可以在任何地方使用hotloadjs的命令
### 创建项目

比如要创建一个叫apple的项目

``` shell
hotloadjs init apple
```
屏幕 输出

``` shell
=== create project abc sucess ===
cd apple
npm install
npm start
```
说明项目已经创建好了

hotloadjs会在当前目录创建apple文件夹，里面是项目文件。

cd apple 进入项目路径
npm install 安装需要的依赖

install 后可能会看到一些 WARN 这个没关系

npm start 启动项目

执行启动项目后，屏幕 输出

``` shell
socket server running at ws://localhost:8341
web server run at http://localhost:3000
```

说明项目已经启动成功

### 访问

在浏览器中输入 http://localhost:3000

> 注意：如果你是通过远程访问主机，需要把localhost换成 ip 地址


每个演练都有说明 ，这里就不再赘述了。
