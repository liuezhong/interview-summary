1、ES6
   http://es6.ruanyifeng.com/
2、内存泄漏
   一、什么是内存泄漏？
       不再用到的内存，没有及时释放，就叫做内存泄漏（memory leak）。
   二、垃圾回收机制
       a、标记清除（mark and sweep）
       这是JavaScript最常见的垃圾回收方式，当变量进入执行环境的时候，比如函数中声明一个变量，垃圾回收器将其标记为“进入环境”，当变量离开环境的时候（函数执行结束）将其标记为“离开环境”。

       垃圾回收器会在运行的时候给存储在内存中的所有变量加上标记，然后去掉环境中的变量以及被环境中变量所引用的变量（闭包），在这些完成之后仍存在标记的就是要删除的变量了

       b、引用计数(reference counting)
       在低版本IE中经常会出现内存泄露，很多时候就是因为其采用引用计数方式进行垃圾回收。引用计数的策略是跟踪记录每个值被使用的次数，当声明了一个 变量并将一个引用类型赋值给该变量的时候这个值的引用次数就加1，如果该变量的值变成了另外一个，则这个值得引用次数减1，当这个值的引用次数变为0的时 候，说明没有变量在使用，这个值没法被访问了，因此可以将其占用的空间回收，这样垃圾回收器会在运行的时候清理掉引用次数为0的值占用的空间。

       在IE中虽然JavaScript对象通过标记清除的方式进行垃圾回收，但BOM与DOM对象却是通过引用计数回收垃圾的， 也就是说只要涉及BOM及DOM就会出现循环引用问题。
   三、内存泄漏的识别方法
       a、浏览器
          Chrome 浏览器查看内存占用，按照以下步骤操作。
          1、打开开发者工具，选择 Timeline 面板
          2、在顶部的Capture字段里面勾选 Memory
          3、点击左上角的录制按钮。
          4、在页面上进行各种操作，模拟用户的使用情况。
          5、一段时间后，点击对话框的 stop 按钮，面板上就会显示这段时间的内存占用情况。
          如果内存占用基本平稳，接近水平，就说明不存在内存泄漏。
       b、命令行
          命令行可以使用 Node 提供的process.memoryUsage方法。

          console.log(process.memoryUsage());
          // { rss: 27709440,
          //  heapTotal: 5685248,
          //  heapUsed: 3449392,
          //  external: 8772 }
          process.memoryUsage返回一个对象，包含了 Node 进程的内存占用信息。该对象包含四个字段，单位是字节，含义如下。
          rss（resident set size）：所有内存占用，包括指令区和堆栈。
          heapTotal："堆"占用的内存，包括用到的和没用到的。
          heapUsed：用到的堆的部分。
          external： V8 引擎内部的 C++ 对象占用的内存。
          判断内存泄漏，以heapUsed字段为准。
3、html5有哪些新特性、移除了那些元素？如何处理HTML5新标签的浏览器兼容问题？如何区分 HTML 和 HTML5？

    HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加。

    拖拽释放(Drag and drop) API

    语义化更好的内容标签（header,nav,footer,aside,article,section）

    音频、视频API(audio,video)

    画布(Canvas) API

    地理(Geolocation) API

    本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失；

    sessionStorage 的数据在浏览器关闭后自动删除


    表单控件，calendar、date、time、email、url、search

    新的技术webworker, websocket, Geolocation
  移除的元素
  纯表现的元素：basefont，big，center，font, s，strike，tt，u；

  对可用性产生负面影响的元素：frame，frameset，noframes；
  支持HTML5新标签：

      IE8/IE7/IE6支持通过document.createElement方法产生的标签，

      可以利用这一特性让这些浏览器支持HTML5新标签，

      当然最好的方式是直接使用成熟的框架、使用最多的是html5shim框架

         <!--[if lt IE 9]>

         <script> src="http://html5shim.googlecode.com/svn/trunk/html5.js"</script>

         <![endif]-->

      如何区分： DOCTYPE声明\新增的结构元素\功能元素

4、性能优化
    代码层面：避免使用css表达式，避免使用高级选择器，通配选择器。

    缓存利用：缓存Ajax，使用CDN，使用外部js和css文件以便缓存，添加Expires头，服务端配置Etag，减少DNS查找等

    请求数量：合并样式和脚本，使用css图片精灵，初始首屏之外的图片资源按需加载，静态资源延迟加载。

    请求带宽：压缩文件，开启GZIP，

    代码层面的优化
        用hash-table来优化查找

        少用全局变量

        用innerHTML代替DOM操作，减少DOM操作次数，优化javascript性能

        用setTimeout来避免页面失去响应

        缓存DOM节点查找的结果

        避免使用CSS Expression

        避免全局查询

        避免使用with(with会创建自己的作用域，会增加作用域链长度)

        多个变量声明合并

        避免图片和iFrame等的空Src。空Src会重新加载当前页面，影响速度和效率

        尽量避免写在HTML标签中写Style属性

        移动端性能优化

        尽量使用css3动画，开启硬件加速。
        适当使用touch事件代替click事件。
        避免使用css3渐变阴影效果。
        可以用transform: translateZ(0)来开启硬件加速。
        不滥用Float。Float在渲染时计算量比较大，尽量减少使用
        不滥用Web字体。Web字体需要下载，解析，重绘当前页面，尽量减少使用。
        合理使用requestAnimationFrame动画代替setTimeout
        CSS中的属性（CSS3 transitions、CSS3 3D transforms、Opacity、Canvas、WebGL、Video）会触发GPU渲染，请合理使用。过渡使用会引发手机过耗电增加
        PC端的在移动端同样适用
5、sessionStorage,localStorage,cookie区别
    都会在浏览器端保存，有大小限制，同源限制
    cookie会在请求时发送到服务器，作为会话标识，服务器可修改cookie；web storage不会发送到服务器
    cookie有path概念，子路径可以访问父路径cookie，父路径不能访问子路径cookie
    有效期：cookie在设置的有效期内有效，默认为浏览器关闭；sessionStorage在窗口关闭前有效，localStorage长期有效，直到用户删除
    共享：sessionStorage不能共享，localStorage在同源文档之间共享，cookie在同源且符合path规则的文档之间共享
    localStorage的修改会促发其他文档窗口的update事件
    cookie有secure属性要求HTTPS传输
    浏览器不能保存超过300个cookie，单个服务器不能超过20个，每个cookie不能超过4k。web storage大小支持能达到5M
6、如何判断一个对象是否为数组
    如果浏览器支持Array.isArray()可以直接判断否则需进行必要判断

    /**
     * 判断一个对象是否是数组，参数不是对象或者不是数组，返回false
     *
     * @param {Object} arg 需要测试是否为数组的对象
     * @return {Boolean} 传入参数是数组返回true，否则返回false
     */
    function isArray(arg) {
        if (typeof arg === 'object') {
            return Object.prototype.toString.call(arg) === '[object Array]';
        }
        return false;
    }
