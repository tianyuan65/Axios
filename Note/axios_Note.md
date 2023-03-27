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
    * 1.5 axios其他方式方式请求  Request method aliases
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
        * 以上一个为例，点击GET请求的按钮，可以在控制台查看到response输出的结果。点开后会有config、data、header、request等属性。
            * config：配置对象，里面会有请求类型、请求URL、请求体等内容。其中adapter中可以查看到先前添加、删除、修改的内容的个数
            * data：响应体的结果，也就是服务器返回的请求体内容。它是个对象，因为axios自动把服务器返回的结果进行了json解析，把它转成了对象，以便于对结果进行处理。可以查看先前添加、删除、修改的请求体内容
            * header：相应的头信息
            * request：原生的AJAX请求对象，目前已知，axios用来发送AJAX请求，而发送AJAX请求就要使用底层的XMLHttpRequest实例对象，而request属性保存的就是当前axios发送请求时创建的AJAX请求对象，也就是XMLHttpRequest实例对象。
            * status：响应状态码
            * statusText：响应的状态字符串
    * 1.7 axios配置对象  Request Config
        * 指的是axios的在调用时，所接收的参数对象(就是大括号里的所有内容)。要点就是这个配置对象里到底有哪些属性。注：axios的诸多方法中但凡有config这个参数，就代表这个方法里接收的就是配置对象。
        * 配置对象里可以设置的内容
            * URL：指明到底是给谁发送这个请求，设置URL参数
            * method：设置请求的类型，GET/POST/PUT/DELETE
            * baseURL：设定URL的基础结构，例如```http://localhost:3000```。可以把baseURL这是成这个值，然后在设置URL的时候，只需设置后面的路径，不需要再写域名和协议，axios内部会自动将URL和baseURL进行结合，形成最终的URL结果。
            * transformRequest：可以对请求的数据进行处理，处理完后，将处理的结果再发送给服务器
            * transformResponse：对响应的结果做一些改变，改变之后，我们再对改变的结果进行自定义的处理。transformRequest和transformResponse这两个参数都是对请求参数和响应结果做预处理，处理完之后在进行发送请求和响应结果的处理
            * headers：头信息，对请求头信息做配置。在某些项目中，进行身份校验时，要求在头信息中假如特殊的标识，以便检验请求是否满足条件，此时就可以借助headers对请求头信息进行控制。
            * params：设定URL的参数，是一个对象，可以在对象中设置内容。
            * paramSerializer：参数序列化的配置项，对请求的参数做序列化操作，转换成字符串。(看了段皇帝的代码)
            * timeout：超时时间，发送请求时，如果超过了这个事件，此请求就会被取消，单位：ms
            * withCredentials：跨域请求时，对cookie的携带做设置，false就是不携带，如果是true就可以在跨域请求时携带cookie
            * adapter：对请求的适配器做设置，有两种方式，一、发送AJAX请求；二、在node.js中发送HTTP请求
            * auth：对请求的基础验证设置用户名和密码的
                * ```
                    auth: {
                        username: 'janedoe',
                        password: 's00pers3cret'
                    }
                  ``` 
            * responseType：对响应体结果的格式做设置，默认值为json格式的数据内容，接收响应结果后会对其进行转换
            * responseEncoding：响应结果的编码，其值为utf8
            * xsrfCookieName：跨站请求的标识，对cookie的名字做设置
            * xsrfHeaderName：设置跨域请求的头信息。是一个安全设置，保证请求来自于自己的客户端，而不是未知的第三方的客户端
            * onUploadProgress/onDownloadProgress:function(){}：上传和下载时的回调
            * maxContentLength：设置HTTP响应体的最大尺寸，单位：字节byte
            * maxBodyLength：设置请求体内容的最大尺寸
            * validateStatus：设置响应结果的成功，当响应的状态码(xhr.status)的默认值大于等于200，小于300，即为成功，可以直接使用文档里默认写入的，除非我自己设置了成功与失败的规则，否则直接用默认的规则
                * ```
                    validateStatus: function (status) {
                        return status >= 200 && status < 300; 
                    },
                  ```
            * maxRedirects：最大跳转次数，向一个服务器端发送请求时做了跳转，判断是否还需要继续往前进行请求。最多调5次，只能在node.js中使用，前端AJAX中用不到
            * socketPath：设置socket文件的位置，作用是向docker的守护进程发送请求的。若同时申请了socketPath和proxy(文件代理)，socketPath优先执行
            * httpAgent/httpsAgent:new http.Agent/ps.Agent({ keepAlive: true })：对客户端的信息做设置
            * proxy：适用在node.js的服务端中
                * ```
                     proxy: {
                        protocol: 'https',
                        host: '127.0.0.1',
                        // hostname: '127.0.0.1' // Takes precedence over 'host' if both are defined
                        port: 9000,
                        auth: {
                        username: 'mikeymike',
                        password: 'rapunz3l'
                        }
                    }
                  ```
            * cancelToken：取消AJAX请求的设置
            * decompress：判断是否对响应结果进行解压操作，默认是做解压的，只在使用node.js的环境中设置
    * 1.8 axios默认配置 对axios中重复、冗余的代码进行整理，以便在后续简洁书写
        * 书写格式为```axios.defaults.设置内容=''/{}```。以04-axios默认配置.html举例，设置单击响应函数前设置默认配置。
            * ```
                axios.defaults.method='GET'  //设置默认的请求类型为GET
                axios.defaults.baseURL='http://localhost:3000'  //设置基础URL为http://localhost:3000
                axios.defaults.params={id:100}  //设置默认参数为id=100
                axios.defaults.headers={}
                axios.defaults.timeout=3000  //设置超时时间，若在规定的超时时间内没有返回结果，就取消请求
                btns[0].onclick=function(){
                    axios({
                        url:'/posts',
                    }).then(response=>{
                        console.log(response);
                    })
                }
              ```
            * 如上述代码，设置默认请求类型后，在axios中可以不写请求类型；设置baseURL为http://localhost:3000，在axios中只写posts这个资源名称即可(后面还有的话继续写就行)；设置默认的URL参数，在此设置为id：100，点击发送请求按钮后，可以在控制台查看被添加的内容；设置超时时间。
    * 1.9 axios创建实例对象发送AJAX请求
        * 创建名为geng的实例对象，通过create()来创建方法，方法里接收的参数为配置对象，在里面设置默认配置，发送请求方式有两种，1、需要geng()中传递配置对象的具体内容，并调用then方法指定成功的回调；2、借助于封装好的方法发送请求，调用get()，其中接收的参数为配置对象的具体内容，就是```'/react/'```，最终调用then方法并指定成功的回调
            * ```
                const geng=axios.create({
                    baseURl:'https://www.bootcdn.cn/',
                    timeout:2000
                })
                //1.
                geng({
                    url:'/react/'
                }).then(response=>{
                    console.log(response);
                })
                //2.
                geng.get('/react/').then(response=>{
                    console.log(response);
                })
              ```
        * 创建实例对象发送请求的优势：
            * 项目中，接口数据服务来自多方服务器，这多方服务器都提供了数据服务，在发送请求时，若使用默认配置的方式只能向其中一方服务器发送请求。想要向其他的服务器发送请求就需要创建多个默认配置，代码重复且太多。此时使用创建实例对象的方法，来对重复的代码进行整合，在需要向多方服务器发送不同的请求时，就可以直接调用与之相对应的方法，向方法中传入具体配置对象作为参数，并在最后调用指定的成功的回调。存在多方服务器时代码如下：
            * ```
                // 创建实例对象geng  https://www.bootcdn.cn/
                const geng=axios.create({
                    baseURl:'https://www.bootcdn.cn/',
                    timeout:2000
                })

                // 创建实例对象boot
                const boot=axios.create({
                    baseURl:'https://www.youtube.com/',
                    timeout:2000
                })

                // 在此 geng 与axios 对象的功能几乎是一样的
                // geng({
                //     url:'/react/'
                // }).then(response=>{
                //     console.log(response);
                // })
                geng.get('/react/').then(response=>{
                    console.log(response);
                })

                boot({
                    url:''
                }).then(response=>{
                    console.log(response)
                })
                boot.get('').then(response=>{console.log(response)})
              ```
    * 1.10 Interceptor(拦截器)
        * 拦截器是一些函数，分为两大类，一种叫请求拦截器，另一种叫响应拦截器。
        * 作用：
            * 请求拦截器：在发送请求前，借助回调函数，来对请求的参数和内容进行处理和检测，没有问题就发送请求，若有问题就停止并取消请求。就像机场过安检一样，查完我的大行李，进去查我的随身行李，最后再查一下我自己。
            * 响应拦截器：在处理结果之前，先对结果进行预处理，比如：对于失败的结果，对其整体做一个提醒和记录；对数据结果做格式化处理，没有问题的话，再交由我自己自定义的回调来做个性化处理，若有问题，直接在响应拦截器中处理掉即可。也像在机场过海关，看护照，看完护照看签证，看完签证看跟本人像不像，最后用迟疑的眼神给我盖章并放我走。
        * 例子：
            * 1. 因为没有错误(就是没有失败的情况)，先执行请求拦截器成功的回调，再交给响应拦截器并执行响应拦截器成功的回调，最后在我指定的回调中进行个性化处理。
                * ![两个拦截器都成功执行](images/%E4%B8%A4%E7%A7%8D%E6%8B%A6%E6%88%AA%E5%99%A8%E9%83%BD%E6%88%90%E5%8A%9F%EF%BC%8C%E6%9C%80%E5%90%8E%E8%87%AA%E5%AE%9A%E4%B9%89%E5%9B%9E%E8%B0%83%E5%A4%84%E7%90%86.PNG)
            * 若在请求拦截器中抛出一个错误或者在请求拦截器的指定的回调中返回一个新的Promise，并在里面执行失败的方法时，请求拦截器的函数会执行成功的回调，但响应拦截器会执行失败的回调，最后在我指定的回调中进行个性化处理时，也是会执行失败的回调或catch方法
                * ![请求拦截器中的回调中抛出错误](images/%E8%AF%B7%E6%B1%82%E6%8B%A6%E6%88%AA%E5%99%A8%E6%8A%9B%E5%87%BA%E9%94%99%E8%AF%AF%E7%9A%84%E4%B8%80%E7%B3%BB%E5%88%97%E7%BB%93%E6%9E%9C.PNG)
                * ```
                    // 设置请求拦截器  config 配置对象
                    axios.interceptors.request.use(function (config) {
                        console.log('请求拦截器 成功');
                        return config;
                        //throw '参数有问题'
                    }, function (error) {
                        console.log('请求拦截器 失败');
                        return Promise.reject(error);
                    });
                    // 设置响应拦截器
                    axios.interceptors.response.use(function (response) {
                        console.log('响应拦截器 成功');
                        return response;
                    }, function (error) {
                        console.log('响应拦截器 失败');
                        return Promise.reject(error);
                    });
                    // 发送请求
                    axios({
                        method:'GET',
                        url:' http://localhost:3000/posts'
                    }).then(response=>{
                        console.log('自定义回调处理成功的结果');
                        // console.log(response);
                    }).catch(reason=>{
                        console.warn('自定义失败的回调');
                    })
                ```
            * 2. 请求拦截器和响应拦截器分别都有两个的情况下，各自指定的回调执行顺序与只有单个拦截器情况有所不同。如图所示，会先执行2号请求，再执行1号请求，往后的响应是按照书写顺序输出的。
                * ![两种拦截器有多个时的执行顺序](images/%E6%89%A7%E8%A1%8C%E9%A1%BA%E5%BA%8F.PNG)
                * 且在拦截器的指定回调内部可以进行增删改配置对象(config&response)中的参数，在控制台刷新后可以看到修改后的变化。**但我这边不知道是啥原因总执行不该执行的**
                    * ![修改1号请求中config的params](images/%E8%AE%BE%E7%BD%AE%E9%BB%98%E8%AE%A4params.PNG)
                    * **![后续记得加](images)**
                * ```
                    // 设置1号请求拦截器  config 配置对象
                    axios.interceptors.request.use(function (config) {
                        console.log('请求拦截器 成功 - 1号');
                        // 修改config中的参数
                        config.params={a:100}
                        return config;
                    }, function (error) {
                        console.log('请求拦截器 失败 - 1号');
                        return Promise.reject(error);
                    });

                    // 设置2号请求拦截器
                    axios.interceptors.request.use(function (config) {
                        console.log('请求拦截器 成功 - 2号');
                        // 修改config中的参数
                        config.timeout=2000
                        return config;
                    }, function (error) {
                        console.log('请求拦截器 失败 - 2号');
                        return Promise.reject(error);
                    });

                    // 设置1号响应拦截器
                    axios.interceptors.response.use(function (response) {
                        console.log('响应拦截器 成功 1号');
                        return response.data
                        // return response;
                    }, function (error) {
                        console.log('响应拦截器 失败 1号');
                        return Promise.reject(error);
                    });

                    // 设置2号响应拦截器
                    axios.interceptors.response.use(function (response) {
                        console.log('响应拦截器 成功 2号');
                        return response;
                    }, function (error) {
                        console.log('响应拦截器 失败 2号');
                        return Promise.reject(error);
                    });

                    // 发送请求
                    axios({
                        method:'GET',
                        url:' http://localhost:3000/posts'
                    }).then(response=>{
                        console.log('自定义回调处理成功的结果');
                        // console.log(response);
                    }).catch(reason=>{
                        console.warn('自定义失败的回调');
                    })
                  ```
    * 1.11 如何取消请求
        * 1. 添加配置对象的属性
            * axios配置对象中添加cancelToken属性，cancelToken被赋值新的axios.CancelToken，其中传入的参数为一个函数，函数内又传入一个形参c
        * 2. 声明全局变量
            * ```let cancel=null```
        * 3. 将c的值在函数内赋值给cancel
            * ```cancel=c```
        * 4. 给取消请求的按钮绑定事件，并在其中执行cancel函数
            * ```
                btns[1].onclick=function(){
                    cancel()
                }
              ```
        * 5. 但是按照上述步骤在控制台执行时，会看不到取消请求了的效果。所以需要在服务端(integrated terminal，也就是终端)给json-server添加延时响应的操作，```json-server --watch db.json -d 2000```，这样就可以在控制台中发送请求后，在规定的延时时间内取消请求的效果。
            * ![启动端口添加延时响应后取消请求效果](images/%E5%8F%96%E6%B6%88%E8%AF%B7%E6%B1%82.PNG)
        * 6. 有一种特殊情况，因网速问题，用户可能多次点击发送请求的按钮，此时就需要给发送请求设置一个限制，查看在规定的延时时间内连续发送请求时，前一个请求是否在继续发送，若上一个请求还在继续就会被取消。如何实现？**先检测上一个请求是否已经完成**，就是判断第二次或以上次数发送请求时，cancel的值是否是null。因为第二次发送请求时，cancel的值被赋予了c的值，所以在then方法指定的回调里将cancel值初始化```cancel=null```。这样的两手准备可以保证，多次发送请求时cancel值依然为null，而不是函数。整体代码如下
        * ```
            // 获取按钮
            const btns=document.querySelectorAll('button')
            // 声明全局变量
            let cancel=null
            // 发送请求
            btns[0].onclick=function(){
                // 检测上一次请求是否已经完成，若cancel值不是null，就说明上一次的请求还在继续发送
                if(cancel !== null){
                    // 取消上一次的请求
                    cancel()
                }
                axios({
                    method:'GET',
                    url:' http://localhost:3000/posts',
                    // 1. 添加配置对象的属性
                    cancelToken:new axios.CancelToken(function(c){
                        // 3. 将c的值赋值给cancel
                        cancel=c
                    })
                }).then(response=>{
                    console.log(response);
                    // 将cancel的值初始化
                    cancel=null
                })
            }

            // 绑定第二个单击响应事件
            btns[1].onclick=function(){
                cancel()
            }
          ``` 
            * ![多次发送请求，取消上一次请求](images/%E5%A4%9A%E6%AC%A1%E5%8F%91%E9%80%81%E8%AF%B7%E6%B1%82%E8%A2%AB%E5%8F%96%E6%B6%88%E4%B8%8A%E4%B8%80%E4%B8%AA%E8%AF%B7%E6%B1%82.PNG)

* **第二章：axios源码分析**
    * 2.1 目录结构
        * ![目录结构](images/%E7%9B%AE%E5%BD%95%E7%BB%93%E6%9E%84.PNG)
    * 2.2 源码分析
        * 2.2.1 axios与Axios的关系
            * 1. 从语法上来说，axios不是Axios的实例
            * 2. 从功能上来说，axios是Axios的实例
            * 3. axios是Axios.prototype.request函数bind()返回的函数
            * 4. axios作为对象有Axios原型对象上的所有方法，有Axios对象上所有属性
        * 2.2.2 instance与axios的区别
            * 同：
                * 1. 都是一个能发任意请求的函数：request(config)
                * 2. 都有发特定请求的各种方法：get()/post()/put()/delete()
                * 3. 都有默认配置和拦截器的属性：defaults/interceptors
            * 异：
                * 1. 默认配置很可能不一样
                * 2. instance没有axios后面添加的一些方法：create()/CancelToken()/all()
        * 2.2.3 axios整体运行流程
            * ![axios运行流程图](images/axios%E8%BF%90%E8%A1%8C%E6%B5%81%E7%A8%8B.PNG)
            * 1. axios对象创建过程模拟实现：
                * ```
                    // 构造函数
                    function Axios(config) {
                        // 初始化，是为了创建default默认属性
                        this.defaults=config
                        this.intercepters={
                            request:{},
                            response:{}
                        }
                    }

                    // 原型上添加相关的方法
                    Axios.prototype.request=function(config){
                        console.log('发送AJAX请求 请求的类型为'+config.method);
                    }
                    Axios.prototype.get=function(config){
                        return this.request({method:'GET'})
                    }
                    Axios.prototype.post=function(){
                        return this.request({method:'POST'})

                    }

                    // 声明函数
                    function createInstance(config) {
                        // 实例化一个对象
                            //context.get() context.post() 但是不能当做函数使用 context()
                        let context=new Axios(config)  
                        // 创建请求函数
                            //instance 是一个函数，并且可以传对象 instance({})，此时instance不能 instance.get()
                        let instance=Axios.prototype.request.bind(context)  
                        // 将Axios.prototype对象中的方法添加到instance函数对象中
                        Object.keys(Axios.prototype).forEach(key=>{
                            instance[key]=Axios.prototype[key].bind(context)
                        })
                        // 为instance函数对象添加属性default与interceptors
                        Object.keys(context).forEach(key=>{
                            instance[key]=context[key]
                        })
                        return instance
                    }

                    let axios=createInstance()

                    // 发送请求方法1：
                    // axios({method:'GET'})
                    // axios({method:'POST'})
                    
                    //发送请求方法2：
                    axios.get({})
                    axios.post({})
                  ```
            * 2. axios发送请求过程详解
                * request(config)=>dispatchRequest(config)=>xhrAdapter(config)
                    * request(config)：将请求拦截器/dispatchRequest()/响应拦截器通过promise链串连起来，返回promise
                    * dispatchRequest(config)：转化和请求数据==>调用xhrAdapter()发请求==>请求返回后转换响应数据，返回promise
                    * xhrAdapter(config)：创建XHR对象，根据config进行相应设置，发送特定请求，并接收响应数据，返回promise
            * 3. axios发送请求过程模拟实现
                * 首先，**声明构造函数Axios**，在其中初始化配置对象。给Axios函数添加request方法，并给其赋值函数。函数中创建成功的promise对象(就是promise直接调用了resolve方法)。声明一个数组chains，dispatchRequest函数和undefined，其中undefined的作用的占位，没有undefined就会在运行链条是出问题，也就是返回失败的结果时需要调用它。promise调用then方法，传入的参数就是chains的两个元素，赋值给result，并返回promise结果，在这里result就是Axios函数的执行结果；其次，**声明dispatchRequest构造函数。**dispatchRequest函数的返回值是一个promise实例对象，而这个函数的返回值的结果取决于适配器函数(就是xhrAdapter)的成功与否。既然这个函数的返回值是一个promise对象，那就可以调用then方法，返回的结果(xhrAdapter)为成功的promise那就给函数(dispatchRequest)也返回一个成功的promise对象结果，失败则抛出错误；再次，**声明xhrAdapter构造函数(也就是adapter适配器)**，在函数返回一个新的Promise，并在新的Promise中发送AJAX请求。绑定事件的函数中，若成功则在其中调用resolve方法，并在resolve方法中传入配置对象、响应体、响应头请求对象等作为配置对象的内容，失败则调用reject方法，并在其中返回新的Error方法，里面传入失败时输出的字符串等数值；最后，**创建axios函数**，```let axios=Axios.prototype.request.bind(null)```，并调用axios函数。
                * 总结就是，从下到上看才容易理解
                * ```
                    // axios发送请求  axios Axios.prototype.request  bind
                    // 1. 声明构造函数
                    function Axios(config) {
                        this.config=config
                    }
                    Axios.prototype.request=function(config){
                        // 发送请求
                        // 创建一个promise对象
                        let promise=Promise.resolve(config)
                        // 声明一个数组  undefined的作用就是占位，没有就会在运行链条时出问题
                        let chains=[dispatchRequest,undefined]
                        // 循环处理，调用then方法指定回调
                        let result=promise.then(chains[0],chains[1])
                        // 返回promise的结果，在这里result就是Axios函数的执行结果
                        return result;
                    }
                    // 2. dispatchRequest函数(它的返回值是一个promise实例对象)
                    function dispatchRequest(config){
                        // 调用适配器发送请求
                        return xhrAdapter(config).then(response=>{
                            // 对响应的结果进行转换处理
                            return response
                        },error=>{
                            throw error
                        })
                    }
                    // 3. adapter适配器
                    function xhrAdapter(config) {
                        return new Promise((resolve,reject)=>{
                            // 发送AJAX请求
                            let xhr=new XMLHttpRequest()
                            // 初始化
                            xhr.open(config.method,config.url)
                            // 发送请求
                            xhr.send()
                            // 绑定事件
                            xhr.onreadystatechange=function(){
                                if(xhr.readyState===4){
                                    if(xhr.status>=200 && xhr.status<300){
                                        // 成功的状态
                                        resolve({
                                            // 配置对象
                                            config:config,
                                            // 响应体
                                            data:xhr.response,
                                            // 响应头 字符串(parseHeaders)
                                            haeders:xhr.getAllResponseHeaders(),
                                            // xhr 请求对象
                                            request:xhr,
                                            // 响应状态码
                                            status:xhr.status,
                                            // 响应状态字符串
                                            statusText:xhr.statusText
                                        })
                                    }else{
                                        // 失败的状态
                                        reject(new Error('请求失败的状态码为'+xhr.status))
                                    }
                                }
                            }
                        })
                    }

                    // 4. 创建axios函数
                    let axios=Axios.prototype.request.bind(null)

                    axios({
                        method:'GET',
                        url:'http://localhost:3000/posts'
                    }).then(response=>{
                        console.log(response);
                    })
                  ```
            * 4. axios拦截器运行过程
                * 请求：在真正发送请求前执行的回调函数，可以对请求进行检查或配置进行特定处理。成功的回调，传递的默认是config(必须是config)；失败的回调传递的默认是error。
                * 响应：在请求得到响应后执行的回调函数，可以对响应数据进行特定处理。成功的回调传递的默认是response；失败的回调传递的默认是error。
                * 拦截器的use方法执行时，是把被当做参数的函数的两个回调保存在request的handlers属性当中，相当于保存回调
                    * 而在request方法中，也就是axios函数在调用的时，是对数组进行完善，把请求拦截器放在数组的前面，而把响应拦截器放在数组的后面，最终再通过循环的方式，以跳板的形式执行
                * ![axios拦截器运行过程](images/axios%E6%8B%A6%E6%88%AA%E5%99%A8%E6%89%A7%E8%A1%8C%E8%BF%87%E7%A8%8B.PNG)
            * 5. axios拦截器模拟运行过程
                * 首先，**创建Axios构造函数并发送请求**。构造函数中初始化配置对象及拦截器，拦截器里传递两个属性request和response作为对象，这两个属性是下面的拦截器管理器构造函数中的handlers数组会被压入两个函数对象作为数组对象(下面详细说)初始化的。在request方法里创建成功的promise对象和一个数组chains，在chains中传入两个对象dispatchRequest和undefined(为占位而存在的，前面说过)。在处理拦截器的步骤，需要将请求拦截器的回调压入到chains的前面，将响应拦截器的回调压入到chains的后面，```chains.unshift/push(item.fulfilled,item.rejected)```,都需要使用forEach方法遍历request.handlers=[]。再往下，需要从数组chains中一个一个取出回调，第一个就是十个数组当中的第二个请求拦截器的两个回调传入到这其中，然后返回一个新的promise。```promise=promise.then(chains.shift(),chains.shift())```
                    * ![从chains数组中依次一个一个取出回调，最后返回一个新的promise](images/%E4%BB%8Echains%E4%B8%AD%E4%BE%9D%E6%AC%A1%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%8F%96%E5%87%BA%E5%9B%9E%E8%B0%83%EF%BC%8C%E6%89%A7%E8%A1%8C%E5%90%8E%E8%BF%94%E5%9B%9E%E4%B8%80%E4%B8%AA%E6%96%B0%E7%9A%84promise.PNG)
                    * ![将请求和响应压入到chains数组后输出chains](images/%E5%B0%86%E8%AF%B7%E6%B1%82%E5%92%8C%E5%93%8D%E5%BA%94%E5%8E%8B%E5%85%A5%E5%88%B0chains%E5%90%8E%E8%BE%93%E5%87%BAchains.PNG)
                * 其次，**创建dispatchRequest构造函数**，其内部返回一个新的promise对象。新的promise对象里调用resolve方法，方法内传入配置对象的内容作为参数。创建实例context和axios函数，使用bind方法将实例context作为参数传入。之后进行遍历，将context属性、config和interceptors添加到axios函数兑现身上。```Object.keys(context).forEach(key=>{axios[key]=context[key]})```
                    * ![创建axios后interceptors输出结果](images/%E6%9E%84%E5%BB%BA%E5%AE%8Caxios%E5%90%8E%E8%BE%93%E5%87%BA.PNG)
                * 再次，**创建拦截器管理器构造函数InterceptorManager**，其中创建handlers数组，以便在InterceptorManager的use方法中，把作为参数的两个函数做成对象fulfilled和rejected，压到上面的handlers数组中。最后，**创建两个请求和两个响应**，并分别设置编号one和two，在最后自定义回调发送请求后，可以查看到请求和响应拦截器执行的顺序结果。
                * ![拦截器执行顺序](images/%E6%8B%A6%E6%88%AA%E5%99%A8%E6%89%A7%E8%A1%8C%E9%A1%BA%E5%BA%8F.PNG)
                * ```
                    // 构造函数
                    function Axios(config) {
                        this.config=config
                        this.interceptors={
                            request:new InterceptorManager(),
                            response:new InterceptorManager()
                        }
                    }

                    // 发送请求 重点+难点
                    Axios.prototype.request=function(config){
                        // 创建一个Promise对象
                        let promise=Promise.resolve(config)
                        // 创建一个数组
                        const chains=[dispatchRequest,undefined]
                        // 处理拦截器
                        // 请求拦截器 将请求拦截器的回调，压入到chains的前面  需要遍历request.handlers=[]
                        this.interceptors.request.handlers.forEach(item=>{
                            chains.unshift(item.fulfilled,item.rejected)
                        })
                        // 响应拦截器 将响应拦截器的回调，压入到chains的后面  需要遍历request.handlers=[]
                        this.interceptors.response.handlers.forEach(item=>{
                            chains.push(item.fulfilled,item.rejected)
                        })
                        // console.log(chains);
                        // 遍历
                        while(chains.length>0){
                            // 从数组chains中一个一个取出回调 
                            // 第一个就是十个数组当中的第二个请求拦截器的两个回调传入到这其中，然后返回一个新的promise
                            promise=promise.then(chains.shift(),chains.shift())
                        }
                        return promise
                    }

                    // 发送请求
                    function dispatchRequest(config) {
                        // 必须返回一个新的promise对象，不返回的话，后续axios()没法调用then()
                        return new Promise((resolve,reject)=>{
                            resolve({
                                status:200,
                                statusText:'OK'
                            });
                        })
                    }
                    
                    // 创建实例
                    let context=new Axios({})
                    // 创建axios函数
                    let axios=Axios.prototype.request.bind(context)
                    // 将context属性 config interceptors 添加到axios函数对象身上
                    Object.keys(context).forEach(key=>{
                        axios[key]=context[key]
                    })

                    // 拦截器管理器构造函数
                    function InterceptorManager() {
                        this.handlers=[]
                    }
                    InterceptorManager.prototype.use=function(fulfilled,rejected){
                        // 只要调用下面的axios.interceptors.request/response.use，就把作为参数的两个函数做成对象fulfilled和rejected，压到上面的handlers数组中
                        this.handlers.push({
                            fulfilled,
                            rejected
                        })
                    }

                    // 由此以上为功能封装代码；以下为功能测试代码
                    // 设置1号请求拦截器  config 配置对象
                    axios.interceptors.request.use(function one(config) {
                        console.log('请求拦截器 成功 - 1号');
                        // 修改config中的参数
                        config.params={a:100}
                        return config;
                    }, function one(error) {
                        console.log('请求拦截器 失败 - 1号');
                        return Promise.reject(error);
                    });

                    // 设置2号请求拦截器
                    axios.interceptors.request.use(function two(config) {
                        console.log('请求拦截器 成功 - 2号');
                        // 修改config中的参数
                        // config.timeout=2000
                        return config;
                    }, function two(error) {
                        console.log('请求拦截器 失败 - 2号');
                        return Promise.reject(error);
                    });

                    // 设置1号响应拦截器
                    axios.interceptors.response.use(function one(response) {
                        console.log('响应拦截器 成功 1号');
                        return response.data
                        // return response;
                    }, function one(error) {
                        console.log('响应拦截器 失败 1号');
                        return Promise.reject(error);
                    });

                    // 设置2号响应拦截器
                    axios.interceptors.response.use(function two(response) {
                        console.log('响应拦截器 成功 2号');
                        return response;
                    }, function two(error) {
                        console.log('响应拦截器 失败 2号');
                        return Promise.reject(error);
                    });
                    
                    // 发送请求(用户的自定义回调)
                    axios({
                        method:'GET',
                        url:' http://localhost:3000/posts'
                    }).then(response=>{
                        console.log(response);
                    })
                  ```
            * 6. axios取消请求
                * 原理：在cancelToken的身上维护了promise属性，并将改变其状态的变量(resolvePromise)暴露到全局中。在声明全局变量```let cancel=null```保存其值，只要cancel方法在取消请求的函数内调用，内部的resolvePromise就会执行，它(resolvePromise)一执行，resolve函数就会被赋值给resolvePromise，在这一步，this.promise的状态就会变为成功。而它(this.promise)的状态一变为成功，发送请求的方法中的唯一的回调就会执行，唯一的回调一执行，```xhr.abort()```就会执行，这孩子要被执行，发送的请求就会被取消
            * 7. 模拟实现axios取消请求
                * 首先，创建Axios构造函数，给Axios的原型添加request方法，其中设置返回值为```dispatchRequest(config)```，在下面创建dispatchRequest构造函数并在dispatchRequest函数中返回```xhrAdapter(config)```。其次，创建xhrAdapter适配器构造函数，在其中返回一个新的promise，并写入关于发送AJAX请求的处理代码。之后在**关于取消请求的处理中**，给配置对象的cancelToken对象身上添加promise，并指定成功的回调，在指定的回调中添加取消请求的方法，```xhr.abort()```。但是这个方法不能在new Promise中直接执行，需要进入配置对象内是否有cancelToken函数的判断中，否则，请求发都发不出去。最后在判断内部，将整体结构设置为失败，```reject(new Error('请求已经被取消'))```。再次，**创建CancelToken构造函数**，传入executor作为参数(这个是重点之一，后续函数传参数的时候很重要)。函数内部声明一个极为普通的变量resolvePromise，并为实例对象添加promise属性，其属性为新的promise对象，在新的promise对象中指定成功的回调，并将resolve赋值给resolvePromise。最后调用executor函数，传入函数作为参数，在函数里面执行resolvePromise函数。最后，声明一个全局变量，```let cancel=null```。在发送请求的单击事件中，检测上一次发送的请求是否已经完成，创建cancelToken的值，并在其内部将参数c的值赋值给cancel，```let cancelToken=new CancelToke(function(c){cancel=c})```。具体函数作为参数的流程如图：
                    * ![executor的指向](images/executor%E7%9A%84%E6%8C%87%E5%90%91.PNG)
                    * ![CancelToken的参数指向和递进](images/CancelToken(executor(c)%7Bcancel%3Dc%7D).PNG)
                * ```
                    // 创建构造函数
                    function Axios(config) {
                        this.config=config

                    }
                    // 原型的request方法
                    Axios.prototype.request=function(config){
                        return dispatchRequest(config)
                    }

                    // dispatchRequest函数
                    function dispatchRequest(config) {
                        return xhrAdapter(config)
                    }
                    // xhrAdapter 适配器
                    function xhrAdapter(config) {
                        // 发送AJAX请求
                        return new Promise((resolve,reject)=>{
                            // 实例化对象
                            const xhr=new XMLHttpRequest()
                            // 初始化
                            xhr.open(config.method,config.url)
                            // 发送
                            xhr.send()
                            // 处理结果
                            xhr.onreadystatechange=function(){
                                if(xhr.readyState===4){
                                    // 判断结果
                                    if(xhr.status>=200 && xhr.status<300){
                                        // 设置为成功的状态
                                        resolve({
                                            status:xhr.status,
                                            statusText:xhr.statusText
                                        })
                                    }else{
                                        reject(new Error('请求失败'))
                                    }
                                }
                            }
                            // 关于取消请求的处理
                            if(config.cancelToken){
                                // 对cancelToken对象身上的promise对象指定成功的回调
                                config.cancelToken.promise.then(value=>{
                                    // 这行代码不能在new Promise中直接执行，需要进入这个判断，直接执行的话，请求发都发不出去
                                    xhr.abort()
                                    // 将整体结构设置为失败
                                    reject(new Error('请求已经被取消'))
                                })
                            }
                        })
                    }

                    // 创建axios函数
                    const context=new Axios({})
                    const axios=Axios.prototype.request.bind(context)

                    // CancelToken构造函数
                    function CancelToken(executor) {
                        // 声明一个变量 它只是个普通变量 
                        var resolvePromise;
                        // 为实例对象添加属性
                        this.promise=new Promise((resolve)=>{
                            // 将resolve赋值给resolvePromise
                            resolvePromise=resolve
                        })
                        // 调用executor函数
                        executor(function(){
                            // 执行resolvePromise函数
                            resolvePromise()
                        })
                    }
                    
                    // 获取按钮  以上为模拟实现的代码
                    const btns=document.querySelectorAll('button')
                    // 2. 声明全局变量
                    let cancel=null
                    // 发送请求
                    btns[0].onclick=function(){
                        // 检测上一次请求是否已经完成
                        if(cancel !== null){
                            // 取消上一次请求
                            cancel()
                        }

                        // 创建cancelToken的值
                        let cancelToken=new CancelToken(function(c){
                            // 3. 将c的值赋值给cancel
                            cancel=c
                        })

                        axios({
                            method:'GET',
                            url:'http://localhost:3000/posts',
                            // 1. 添加配置对象的属性
                            cancelToken:cancelToken
                        }).then(response=>{
                            console.log(response);
                            // 将cancel初始化
                            cancel=null
                        })
                    }

                    // 绑定第二个取消请求的事件
                    btns[1].onclick=function(){
                        cancel()
                    }
                  ```
                    * ![呈现效果](images/%E5%91%88%E7%8E%B0%E6%95%88%E6%9E%9C.PNG)
                    * ![输出效果](images/%E8%BE%93%E5%87%BA%E6%95%88%E6%9E%9C.PNG)


## 总结
* 构造函数的显式原型对象的方法，实例对象是可以直接调用的
* axios类似于jQuery既可以当做函数调用，也可以调方法使用，其原理就是先定义一个函数，在函数中添加对应的方法和属性，形成最终的结果之后就可以调用