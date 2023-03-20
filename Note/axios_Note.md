## axios
* **第一章：axios的理解和使用**
    * 1.1 axios是什么？
        * 1. 前端最流行的ajax请求库
        * 2. 是基于promise的HTTp客户端，可以在浏览器和node.js两个环境中运行。
            * 在浏览器端运行时，借助axios，向浏览器端发送AJAX请求来获取数据
            * 在node.js中运行时，向远端服务发送HTTP请求
    * 1.2 axios的特点
        * 1. 向浏览器端发送AJAX请求(XMLHttpRequests)
        * 2. 在node.js中发送HTTP请求
        * 3. 支持Promise的相关操作
        * 4. 请求和响应拦截器(在请求之前做一些准备工作；在响应回来后，对结果进行处理)
        * 5. 对请求和相应数据做转换
        * 6. 取消请求
        * 7. 自动地把结果转换为JSON数据
        * 8. 保护客户端阻挡跨站攻击(XSRF)
    * 1.3 axios的介绍与页面配置
        * 安装：详见此链接INSTALLINNG(https://github.com/axios/axios#cdn)
            * npm  npm install axios(包管理工具，通常会在做项目时，下载安装后使用)
            * bower  bower install axios(包管理工具，会在页面中通过script标签对其做引入、使用)
            * yarn add axios(同npm)
            * jsDelivr CDN(详见https://github.com/axios/axios#cdn 中对应方法的链接)
            * unpkg CDN(同jsDelivr)

* **第二章：axios源码分析**