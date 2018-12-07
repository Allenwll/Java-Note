# Vue介绍：

*   是一套构建用户界面的渐进式框架
*   只关注视图层，采用自底向上增量开发设计
*   目标是通过尽可能的API实现响应的数据绑定和组合的视图组件*   类似的其他的有Angular和React

# 1.node.js安装

> vue项目通常使用webpack工具构建,webpack命令执行依赖node.js的环境 [下载地址](https://nodejs.org/en/download)
> 
> 安装完成后可在cmd上输入npm查看是否安装成功，不成公配置环境变量到Path

# 2.cnpm安装

> npm的依赖包部署在国外，所以需要安装cnpm 它是淘宝对npm的镜像服务器
> 
> `安装命令为 npm install -g cnpm --registry=https://registry.npm.taobao.org`

# 3.vue-cli安装

> 是vue官方提供的命令行工具，用于快速搭建大型单页应用
> 提供开箱即用的构建工具配置，只需一分钟即可启动带热重载
> 保存时静态检查以及可用于生产环境的构建配置的项目
> `安装命令为 cnpm install -g vue-cli`
> 输入vue 测试是否成功

# 4.测试新建vue项目

1.新建项目文件夹 e.g. vue-demo

2.cd到该文件夹 输入:vue init webpack vue-demo

3.进行初始化操作， 可跳过模板提供的测试框架 Karma+Mocha和Nightwatch

4.执行cnpm install 安装依赖包，安装完毕之后项目会多一个node_modules 文件夹  

5.输入npm run dev 启动项目 

6.若启动失败 则将node_modules文件夹删除 再重新执行4步骤