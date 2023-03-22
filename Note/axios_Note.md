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

* **第二章：axios源码分析**


## 总结
* 