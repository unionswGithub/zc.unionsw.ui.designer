MagicalDrag布局器 支持任何js框架的UI嵌入，由于精力有限，我们仅提供了基于Jquery的layui和基于Vue的ElementUi
欢迎您有新的UI嵌入需要，可以跟我们合作，会得到我们最大的技术支持和最优惠的价格
以下为大家讲解如何新接入其他UI 阅读本文档前请先阅读 《使用文档.md》 体验下当前已经接入的ui文件分布方式


第一步：导入新UI的依赖资源，请放在
    assets\drag\ui\{ui名称}\{版本号}\...
    可以参考目前layui element的放置方式

第二步：为您的新ui创建一个新领域 布局器为每一种技术的ui独立关联，互不干涉，因此采用iframe技术隔离保护，这样做的好处可以让您iframe的内容不会与布局器本身冲突
    新建 iframe-{ui名称}-{版本号}.html 与index.html同级，
    如果您是使用vue框架 内容与iframe-element-2.10.1.html基本一致
    如果您是使用jquery框架 内容与iframe-layui-2.5.4.html基本一致

    注意 改动地方是 要引入您的ui的js和css

    <!DOCTYPE html>
    <html>
    <head>
        
        <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
        <meta name="renderer" content="webkit">
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
        <meta name="viewport" content="width=device-width,initial-scale=1">
        <link rel="shortcut icon" href="favicon.ico"/>
        <title>MagicalDrag,layui表单设计器-基于layui的可视化拖拽布局器</title>
        <meta name="description" content="表单布局器,基于layui的可视化拖拽布局器">
        <meta name="keywords" content="layui表单设计,前端拖拽布局器,可视化拖拽布局器">
        <!--引入您的uicss-->
        <link href="assets/drag/ui/{ui名称}/{版本号}/{您的UI}.css" rel="stylesheet">
        <link href="assets/drag/css/magicalcoder-iframe.css" rel="stylesheet">

        <script type="text/javascript" src="assets/drag/js/lib/json3.js"></script>
        <!--echarts 图表工具-->
        <script type="text/javascript" src="assets/drag/js/lib/echarts/4.2.1/echarts.min.js"></script>

        <!--引入您的UIjs-->
        <script type="text/javascript" src="assets/drag/ui/{ui名称}/{版本号}/{您的UI}.all.js"></script>

        <!--此iframe相关脚本-->
        <script type="text/javascript" src="assets/drag/js/user/ui/iframe-ui-before.js"></script>
        <script type="text/javascript" src="assets/drag/js/user/ui/{ui名称}/{版本号}/iframe-ui.js"></script>
        <script type="text/javascript" src="assets/drag/js/user/ui/iframe-ui-end.js"></script>
    </head>
    <body id="iframeBody" class="inner_iframe">
        <div class="drag-mc-pane layui-form" id="magicalDragScene" magical_-coder_-id="Root">
    </div>
    </body>
    </html>

第三步：在user文件夹下增加个性化ui的处理js和配置js
    具体路径：
        assets\drag\js\user\ui\{ui名称}\{版本}\constant.js   这里处理此ui下控件的属性等配置操作 具体配置参见《使用文档.md》
        assets\drag\js\user\ui\{ui名称}\{版本}\iframe-ui.js  这里处理此ui下控件的初始化等交互操作

        同理，此文件 根据您的ui采用jquery框架编写的话就拷贝layui下的，采用vue框架编写的话就拷贝element下的

第四步：这一步就涉及到如何把此UI的组件放入布局器了 打开  assets\drag\js\user\ui\{ui名称}\{版本}\constant.js
    建议：打开此ui的官方网站，仔细查看文档，对方提供了哪些控件，然后都配置到我们的constat.js里
     这里是比较耗费工作量的地方，慢则1周半 快则1周即可完成 关于constatn.js，可以先参考当前layui或者elementui的配置 一个个按照现有示例尝试
     您加入的新控件越多，您自己的软件就越强大

第五步：预览
    采用http方式访问index.html 应该会报错，因为当前是开发模式，您要指定加载您的ui资源
                【第一处改动】
                     <select  lay-filter="select-filter-top-change-frame-type">
                            <!--如果您的value以http开头，则浏览器会直接重定向到此地址-->
                            <option value="/element">Element-UI</option>
                            <option value="/layui">Layui</option>
                            <!--                        <option value="bootstrap3">bootstrap3 (Jquery)</option>
                                                    <option value="bootstrap4">bootstrap4 (Jquery)</option>
                                                    <option value="amazeui2">amazeui2 (Jquery)</option>-->
                        </select>
                 【第二处改动】 这里如果您嫌麻烦 也可以直接写死加载当前ui的
                      <script type="text/javascript">
                            /*放此处 有利于尽快让iframe初始化完成 */
                            var ui = window.location.href;
                            if(ui.indexOf("layui")!=-1){
                                $("[lay-filter='select-filter-top-change-frame-type']").val("/layui")
                                $("#dropInnerIframe").attr("src","iframe-layui-2.5.4.html")
                            }else if(ui.indexOf("element")!=-1) {
                                $("[lay-filter='select-filter-top-change-frame-type']").val("/element")
                                $("#dropInnerIframe").attr("src","iframe-element-2.10.1.html")
                            }else {
                               /* $("[lay-filter='select-filter-top-change-frame-type']").val("/element")
                                $("#dropInnerIframe").attr("src","iframe-element-2.10.1.html")*/

                                $("[lay-filter='select-filter-top-change-frame-type']").val("/layui")
                                $("#dropInnerIframe").attr("src","iframe-layui-2.5.4.html")
                            }
                        </script>

                【第三处改动】 这里如果您嫌麻烦 也可以直接写死加载当前ui的 同时只能有一个被打开 也可以自己放入框架 自行选择加载哪个
                        <!--如果使用element ui则打开此处注释-->
                        <!--<script type="text/javascript" src="assets/drag/js/user/ui/element/2.10.1/constant.js"></script>-->
                        <!--如果使用layui则打开此处注释-->
                        <script type="text/javascript" src="assets/drag/js/user/ui/layui/2.5.4/constant.js"></script>
                        !--如果使用其他则打开此处注释-->
                        <script type="text/javascript" src="assets/drag/js/user/ui/{ui名称}/{版本}/constant.js"></script>

第六步：恭喜您完成了集成 如果遇到问题 欢迎咨询我们 大胆 细心 一定能完成新UI的集成
