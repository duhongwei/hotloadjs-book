# 开始

热替换，也有的叫热加载现在越来越多的应用到开发中。但是要支持热替换需要一大堆东西支持，相当于要跑飞机，得先搭飞机场。用 [hotload.js](https://github.com/duhongwei/hotloadjs) 就不一样了，uglify后只有3KB多一点，没有任何依赖，在页面中引用后就能支持js模块热替换了。

使用 [hotload.js](https://github.com/duhongwei/hotloadjs) 编写js模块后，模块就有了热替换的能力。服务器端文件变化时通过socket发到页面，页面重新加载变化的js文件（一个模块一个文件），文件加载后，hotloadjs [更新所有相关的模块](hotload/unload.md) ，并做 [清理](hotload/unload.md) 和 [状态保持](hotload/hold.md) 的工作， 就实现了模块的热替换。

先感受一下效果 [演练](example.md)

Hotloadjs把精力放在依赖处理和热替换上，不负责具体加载文件。

Hotloadjs 只用于浏览器环境，参照了ADM规范。

hotloadjs 支持ie6+ 和现代浏览器

Hotloadjs 规定：模块分为两个状态，waiting和ready。

如果有什么问题可以在[holoadjs issue](https://github.com/duhongwei/hotloadjs/issues) 上提出。

接下来，会做更深入地探讨。无规矩不成方圆，首先从[规范](specs.md)说起
