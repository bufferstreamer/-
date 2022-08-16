标签分为单标签和双标签
<标签名 属性>内容</标签名> 可以有多个属性 属性和属性之间必须以空格分开 标签和属性也要空格分开 属性没有顺序之分
属性： 属性名="属性值"
标题标签：  <h1>1级标题</h1>~<h6>6级标题</h6> 只有六级标题
段落标签：  <p>段落内容</p>
换行标签：  <br>文字内容
水平线标签：<hr>文字内容  
文本格式化标签：    <b>加粗</b>             <strong>加粗</strong>
                   <u>下划线</u>           <ins>下划线</ins>
                   <i>倾斜</i>             <em>倾斜</em>
                   <s>删除线</s>           <del>删除线</del>
图片标签：  <img src="" alt="" title="" width="" height="">
                                        src属性值为图片路径 alt属性值为替换文本，当图片加载失败时显示alt的文本
                                        title属性值为提示文本，当鼠标悬停在图片时，显示提示文本，不知可以用于图片标签，标题标签也可
上级路径：  ../
音频标签：  <audio src="" controls autoplay loop></audio>  controls显示播放控件 autoplay打开网页自动播放 loop循环播放 只支持mp3 wav ogg
视频标签：  <video src="" controls autoplay loop></video>   只支持mp4 webm ogg
链接标签：  <a href="" target="">提示信息</a>   href属性值为跳转的网页 target属性值为_self时跳转网页覆盖当前网页 是target的默认值
                                               属性值为_blank时产生一个新窗口跳转
空链接：    <a href="#"></a>    回到页面顶部 


列表标签：  <ul>无序整体</ul>   <li>整体中的每一行</li> 默认每一行有小圆点 其中ul中只能由li标签
            ul是块元素 宽度默认为父元素的宽度，高度默认由内容撑开 
            a是行内元素 宽高由内容撑开不能设置
           <ol>有序整体</ol>   <li>整体中的每一行</li>
           <dl>自定义整体</dl>  <dt>主题</dt>   <dd>每一项内容</dd>

表格标签：  <table>表格整体</table>     <tr>表格每一行</tr>       <td>每个单元格</td>      
           <border>表格边框<border>    <width>表格宽度</width>   <height>表格高度</height>
           <caption>表格大标题</caption>    <th>表头单元格</th>
           <thead>表格头部</thead>      <tbody>表格主体</tbody>   <tfoot>表格底部</tfoot>

合并单元格：    将内容相同的多个单元格合并，左右相同保留左侧，上下相同保留上侧
           <td rowspan="">跨行合并      <td colspan="">跨列合并  注意 不能跨结构标签合并 即不能跨tbody和tfoot合并

表单标签：  <input type=""> type的值为text为默认文本输入框  placeholder占位符 展示提示信息 name  value
                                        为password为密码输入框 同上
                                        为radio单选     name属性可以分组 同一组只能有一个被选中 checked属性，默认被选中
                                        为checkbox为多选    checked属性，默认被选中
                                        为file为上传文件    multiple多文件上传
                                        为submit为提交
                                        为reset为重置       
                                        为button为普通按钮  value属性表示按钮文字
                                        所有的按钮想要实现功能都需要用<form></form>标签把整个表单标签包裹起来
            <select>下拉菜单</select>   <option>菜单选项</option>
            文本域标签：<textarea cols="" rows=""> 输入多个文本</textarea>  cols属性值为宽度 rows为可输入行数
            label标签：把文字内容与表单标签绑定起来     
无语义标签：    div标签一行只显示一个   
               span标签一行显示多个
有语义标签：    <header>网页头部</header>
               <nav>网页导航</nav>
               <footer>网页底部</footer>
               <aside>网页侧边栏</aside>
               <section>网页区块</section>
               <article>网页文章</article>
空格合并现象：网页会将html中得多个空格，换行，tab等转换为一个空格



