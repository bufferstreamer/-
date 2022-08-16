位置：head标签中 title标签下
语法：
        <style>
            要操作的段落名{
            属性名：属性值；
            }
        </style>

颜色属性：color:颜色名；
背景颜色：background-color: 颜色名；
宽度属性：width:数字；
高度属性：height:数字；
引入方式：  内嵌式：在html文件中任意位置均可通常规定在head得title下
           外联式：通过link标签引入外部CSS文件
           行内式：直接在要操作的段落行得标签属性处使用style属性操作
选择器：    标签选择器：标签名{属性名：属性值；}
            类选择器：.类名{属性名：属性值；}   类名不能以数字和-开头 一个标签可以有多个类名 一个类名可以被多个标签使用
            id选择器：#id属性值{属性名：属性值；}   id属性不能重复 一个标签只能由一个id 一个id只能选中一个标签
            通配符选择器：*{属性名：属性值；}   页面中所有的标签都选中
字体和文本样式：
    字体大小：font-size:数字+px；   需要设置单位否则可能无效
    字体粗细：font-weight:数字（100-900）或者normal/bold；
    字体样式：font-style:normal/italic; 正常或倾斜
    字体：font-family:字体1，字体2，字体3...字体系列； 从左到右检索支持的字体，按照最后得方式输出。字体系列：sens-serif无衬线字体
                                                                                                     serif衬线字体
                                                                                                     monospace等宽字体
可以按照font:s w s f;的格式设置文字，前两个可以省略，省略时为默认值
    文本缩进(首行缩进)：text-indent：数字+px/数字+em
    文本水平对齐方式：text-align:left/center/right; 需要对元素的父类使用    margin:0 auto;对需要居中的元素使用即可
    文本修饰：text-decoration:underline/line-through(删除线)/overline(上划线)/none;
    行高：line-height:数字+px/倍数; 设置行高为单行文字父元素的高度，可以使文字垂直居中，设置为文字单倍行高可以消除上下间距。    
样式层叠问题：同一标签设置相同样式，后面的样式会覆盖前面的
复合选择器：
    后代选择器： 语法：选择器1 选择器2 {css}   后代包括儿子，孙子等所有的后代
    子代选择器： 语法：选择器1>选择器2 {css}   子代只包括儿子。    
    并集选择器： 语法：选择器1,选择器2 {css}   
    交集选择器： 语法：选择器1选择器2  {css}   选到两个选择器的交集  标签选择器必须在最前面
    emmet语法：     生成标签p class为red id为1      p.red#1
                    生成儿子                       .father>.son
                    生成内部文本                   .father>.son{内容}
                    生成多个                       .father>.son{内容}*10
    hover伪类选择器：语法：选择器:hover{css}   编辑鼠标悬停在元素上的状态
背景图片：background-image:url('图片路径')
背景平铺: background-repeat:repeat(水平垂直都平铺)/no-repeat(不平铺)/repeat-x(沿着X平铺)/repeat-y
背景位置: background-postion:水平方向位置 垂直方向位置      数字+px 图片左上角与坐标点重合 或者方位名字表示
可以通过background:color image repeat postion 连写 可以省略

元素显示模式：
    块级元素：
        属性：display:block
        显示特点:独占一行
                宽度默认为父元素的宽度，高度默认由内容撑开
                可以设置宽高
        代表标签:div p h ul li等
    行内元素：
        属性:display:inline
        显示特点:一行可以显示多个
                宽度和高度默认由内容撑开
                不可以设置宽高
        代表标签:a span
    行内块元素:
        属性:display:inline-block
        显示特点:一行可以显示多个
                可以设置宽高
        代表标签:input textarea
    image有行内块元素的特点但是在调试工具中显示为inline
元素转换:display:元素
嵌套规范:p标签不能嵌套div h 等 a标签不能嵌套a标签
CSS三大特性:
    继承性:
        特性:子元素可以继承父元素特点
        可以继承的属性:color font-类 text-类 line-height
        不可以继承的属性:width height background-color
    层叠性:
        特性:同一个标签设置不同的效果，会共同作用
             同一个标签设置相同的效果，会显示最终的样式
             当样式冲突时，只有选择器优先级相同时才会通过层叠性比较
    优先级:继承<通配符<标签<类选择器<id选择器<行内样式<!important 放在属性值之后 ;之前 但是不能提高继承的优先级，只要是继承优先级就是最低
    权重叠加计算:复合选择器中(行内样式的个数,id选择器个数,类选择器个数,标签选择器个数) 看高位即可
    盒子模型:内容区域(content)：width height  
            内边界(padding):从上开始赋值，顺时针赋值，如果没赋值就看对面的值 
            外边界(maigin)：同padding 
                           合并现象:maigin垂直方向会合并而水平方向不会合并
                           塌陷现象:嵌套的block元素中子元素的margin-top也会使父元素产生移动
            边框(border): border-width边框粗细  border-color边框颜色    border-style边框样式(solid实线、dashed虚线、dotted点线)
    对于行内元素来说水平方向的margin和padding可以生效，而垂直方向不可以
    采用CSS3的box-sizing:border-box可以使盒子的width和height设置为盒子的尺寸而不是content的尺寸从而实现自动內减
结构伪类选择器: E：first-child{}  找到第一个子元素的E类标签
               E：last-child{}  找到最后一个子元素的E类标签
               E：nth-child(n){}  找到第n个子元素的E类标签  ()中的n可以选择多个 如2n，2n+1，-n+4(前四个)，n+5(五个以后)等
               E：nth-last-child(n){}  找到倒数第n个子元素的E类标签
               E: nth-of-type(n){} 在e类型中的元素中寻找
伪元素：必须设置content内容才能生效 默认是行内元素
            ::before    在父元素内容前生成一个伪元素
            ::after     在父元素内容后生成一个伪元素
浮动:   float:left/right;   浮动的元素会脱离标准流 比标准流高半个级别所以可以覆盖标准流  浮动会在上一个浮动元素后面左右浮动
                            浮动元素会受到上面元素的边界影响 浮动元素可以设置宽高可以一行多个
清除浮动方法:       额外标签法:在父元素最后添加一个块级元素 给添加的块元素添加clear:both；
                   伪元素清除法:    单伪元素清除法:           .father::after{content:''; display:block; clear:both;}   
                                   双伪元素清除法:           .father::before,::after{content:'';display:table;} 
                                                            .father::after{clear:both;}同时可以解决margin塌陷现象
                    给父元素设置overflow:hidden;       同时可以解决margin塌陷现象
BFC:会默认包裹住内部子元素 可用来清楚浮动           浮动元素和行内块元素都是BFC盒子 overflow的属性不为visible时也是BFC盒子
    不存在margin的塌陷现象 可用来解决margin塌陷                    
定位:可以解决盒子得层叠问题————定位之后的元素层级最高，可以层叠在其他盒子上面
     可以让盒子始终固定在屏幕中的某个位置
     方法:先设置定位方式 再设置偏移值
     定位方式属性名:position:static(静态)/relative(相对)/absolute(绝对)/fixed(固定);
     静态定位:是默认值，就是之前的标准流，不能通过方位属性+数字来移动
     相对定位:相对于自己之前的位置进行移动 而且不会脱标
     绝对定位:相对于非静态定位的父元素进行移动 会脱标
     子绝父相:子元素绝对定位，父元素相对定位
     固定定位:相对于页面移动 会脱标
层级关系:定位元素>浮动元素>标准流元素   定位得层级关系相同，按照书写顺序来显示 但是可以根据z-index:数字;来改变定位得层级关系，数字越大层级越高。
垂直对齐方式:   vertical-align:baseline(基线对齐，默认)/top(顶部)/middle(中部)/bottmo(底部);   
               只能给行内元素或者行内块元素设置，给块元素设置没有效果
光标类型:cursor:default(默认，通常是箭头)/pointer(小手样式，提示可以点击)/text(工字形，提示可以选择文字)/move(十字光标，提示可以移动);
边框圆角:   border-radius:数字/百分比;  从左上角开始顺时针赋值，没有赋值的看对角。
溢出部分显示效果:   overflow:visible(默认，溢出部分可见)/hidden(溢出部分隐藏)/scroll(无论是否溢出都显示滚动条)/auto(根据内容决定是否显示滚动条);
元素本身隐藏:   visibility:hidden;隐藏元素 同时占位置  display:none;隐藏元素 不占位置   display:block;显示
元素整体透明度: opacity:0~1数字;    0表示完全透明 1表示完全不透明
边框合并:   border-collapse:collapse;   让相邻表格边框合并 达到单线效果 
三角形: 盒子的边框是梯形，只要将盒子的宽高设置为0，边框就成了三角形
链接伪类选择器:     a:link{}选中a未被选择的状态
                   a:visited{}选中被访问后的状态
                   a:hover{}鼠标悬停的状态
                   a:active{}鼠标点击的状态     必须按照这四个顺序书写
焦点伪类选择器:  :focus{}
属性选择器: 标签[属性名+属性值]{}       
精灵图:多张小图片合成的大图片
背景图片大小:background-size:宽度 高度; 
            数字()/百分比(相对于盒子的宽高百分比)/contain(将图片等比例放大直到不超出盒子最大)/cover(图片等比例放大直到盒子没有空白)
文字阴影:   text-shadow:h-shadow(必须，水平方向偏移量) v-shadow(必须，垂直方向偏移量) blur(可选，模糊度) color(可选，颜色)
盒子阴影:   box-shadow:h-shadow v-shadow blur spread(可选，阴影扩大) color inset(可选，内部阴影)
过渡:       让元素慢慢变化，可搭配hover使用 transition:all(所有能过渡的属性都过渡)/属性名1+时间(数字+s)，属性名2+时间(数字+s)...;
            需要给过渡的元素本身加transition而不是加给hover此时鼠标移入移出都有过渡效果 如果只加给hover只有移入有效果移除无效果
骨架标签:   <!DOCTYPE html>/*文档类型声明为html5*/
           <html lang="en">/*声明网页语言lang en=ENGLISH ZH-CN=CHINESE*/
           <meta charset="UTF-8">/*标识网页的字符编码*/
SEO(search engine optimization):搜索引擎优化    让网站在搜索引擎中的排名靠前
                                三大标签:   title
                                        description
                                        keywords
有语义标签:      header 头部
                nav    导航
                aside  侧边栏
                footer 底部
                section区块
                article文字
                以上均是块元素 可认为是有特殊含义的div标签即可
ico图标的设置:  网页标签左侧的图标  link<rel="icon" href="路径">
版心:将页面的主体内容约定在界面中心 通过设定固定宽度和margin:0 auto;实现 抽象出一个公共类来增加代码重用率

