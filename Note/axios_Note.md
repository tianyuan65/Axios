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
    * 1.4 axios的基本使用
        * axios中接收一个对象，里面包括几个常用的属性，请求类型(method:'/GET/POST/PUT/DELETE')、url(发送请求后)、请求体内容(data)，除了这些还有其他的属性
        * 首先，引入axios(就是jsDelivr CDN)
        * 其次，绑定事件，并发送AJAX请求。如何发送？
            * 调用axios函数，其内部接收一个参数，参数为一个对象，里面包括一些属性，就是请求类型、URL(启动好的json-server，在这里启动好的就是db.json的json.server)和请求体内容
            * ```
                const btns=document.querySelectorAll('button')
                btns[i].onclick=function(){
                    // 发送AJAX请求
                    axios({
                        // 请求类型
                        method:'GET/POST/PUT/DELETE',
                        // URL
                        url:'http://localhost:3000/posts/2'
                        data:{}
                    }).then(response=>{
                        console.log(response);
                    })
                }
              ```
        * 最后，由于axios最终返回的结果是promise实例对象，所以调用then方法来指定成功的回调，并在其中获取成功的结果。
            * ```then(response=>{console.log(response);})```
        * 效果：
            * GET请求
                * ![Request header](images/GET-%E8%AF%B7%E6%B1%82header.PNG)
                * ![展示指定请求体内容](images/GET-%E8%AF%B7%E6%B1%82%E4%BD%93.PNG.PNG)
            * POST请求
                * ![Request header](images/POST-%E8%AF%B7%E6%B1%82%E5%A4%B4.PNG)
                * ![展示添加的请求体内容，具体的到db.json中看](images/POST-%E8%AF%B7%E6%B1%82%E4%BD%93.PNG)
            * PUT请求
                * ![Request header](images/PUT-%E8%AF%B7%E6%B1%82%E5%A4%B4.PNG)
                * ![展示修改的请求体内容，db.json看](images/PUT-%E8%AF%B7%E6%B1%82%E4%BD%93.PNG)
            * DELETE请求
                * ![Request header](images/DELETE-%E8%AF%B7%E6%B1%82header.PNG)
                * ![请求体内容-成功删除内容](images/DELETE-%E8%AF%B7%E6%B1%82%E4%BD%93.PNG)


* **第二章：axios源码分析**