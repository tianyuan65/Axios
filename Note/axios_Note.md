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
        * axios中接收一个对象，里面包括几个常用的属性，请求类型(method:'/GET/POST/PUT/DELETE')、请求的url(就是启动好的json-server)、请求体内容(data)，除了这些还有其他的属性
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
                * ![展示指定请求体内容](images/GET-%E8%AF%B7%E6%B1%82%E4%BD%93.PNG)
            * POST请求
                * ![Request header](images/POST-%E8%AF%B7%E6%B1%82%E5%A4%B4.PNG)
                * ![展示添加的请求体内容，具体的到db.json中看](images/POST-%E8%AF%B7%E6%B1%82%E4%BD%93.PNG)
            * PUT请求
                * ![Request header](images/PUT-%E8%AF%B7%E6%B1%82%E5%A4%B4.PNG)
                * ![展示修改的请求体内容，db.json看](images/PUT-%E8%AF%B7%E6%B1%82%E4%BD%93.PNG)
            * DELETE请求
                * ![Request header](images/DELETE-%E8%AF%B7%E6%B1%82header.PNG)
                * ![成功删除请求体内容](images/DELETE-%E8%AF%B7%E6%B1%82%E4%BD%93.PNG)
    * 1.5 axios其他方式方式请求
        * axios接收的对象，除了常用的那三个属性，还有其他的属性，在此简单举例，request方法和post方法
            * request()  --  axios.request(config)
                * 查看请求内容的方法，书写格式和调用执行axios函数的方法相同，请求类型、启动好的想要发送请求的URL，最后调用then方法中的回调，可以展示请求体内容，后续点击按钮可查看最新的添加、删除甚至是修改的请求体内容
            * post()  --  axios.post(url[, data[, config]])
                * 添加新的内容的方法，与request()的使用方法不同，请求类型可以不写，有**启动好的想要发送请求的URL**和**请求体内容**即可，且请求体内容和请求配置也是可以选择的。点击按钮后，可以查看到被添加了新的评论(请求内容)，在Network、控制台以及db.json文件中都可查看。
                    * ```
                         // 发送POST请求，添加新的评论
                        btns[1].onclick=function(){
                            // 使用post方法来发送POST请求，其使用方式为 axios.post(url[, data[, config]])
                            axios.post(
                                'http://localhost:3000/comments',
                                {
                                    "body": "想吃辣的",
                                    "postId": 2
                                }).then(response=>{
                                    console.log(response);
                                })
                        }
                      ```
                    * ![添加了想吃辣的评论](images/POST-%E6%B7%BB%E5%8A%A0%E6%96%B0%E7%9A%84%E5%86%85%E5%AE%B9%EF%BC%88%E8%AF%84%E8%AE%BA%EF%BC%89.PNG)
                    * ![控制台查看新添加的评论](images/%E6%8E%A7%E5%88%B6%E5%8F%B0%E6%9F%A5%E7%9C%8B%E6%88%90%E5%8A%9F%E6%B7%BB%E5%8A%A0%E8%AF%84%E8%AE%BA.PNG)
    * 1.6 axios请求响应结果的结构
        * 

* **第二章：axios源码分析**


## 总结
* 