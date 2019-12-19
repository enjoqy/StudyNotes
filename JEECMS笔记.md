## jeecms v9

在浏览器地址栏中输入http://网站域名/jeeadmin/jeecms/index.进入后台管理登录界面

出现系统后台登录界面后，输入登录用户名admin、密码password，单击登录按钮（输入密码错误3次后会要求输入验证码）后台界面

超级管理员正确登陆系统后，系统默认的工作区域主要由下面几部分组成。界面主要分为三个大区域，包括顶部展示区、左侧主菜单导航区、右侧主操作区。

主功能菜单区：包括工作台、内容、栏目、数据中心、用户管理、运营、辅助、界面、配置 

子功能菜单区：即每个主功能菜单的子菜单

## 1.基本概念

- 模型：即构成网站栏目和内容的不同元素，系统可自定义组合形成不同的模型，系统默认包括新闻、单页、下载、图库、视频、招聘、文库
- 栏目：发布的资讯内容所属的频道名称，是系统内容分类的一种形式，可以无限制定义个数和上下级关系。
- 内容分类：发布的资讯内容所属的分类名称，是系统内容分类的一种形式，主要体现在不同位置展示所用。
- 固顶：系统用来衡量资讯内容重要性的指标，以数字大小来界定。
- FTP：发布网页等内容时，上传图片和附件以及发布HTML页面可以选择存放的物理位置，若站点无配置则存放web服务器。
- 工作流：指在发布一篇资讯内容时，经过不同的管理人员审核操作的过程。
- 定时任务：系统设定在某一个特定时间内，由程序自动执行相关操作。
- 扩展：系统除去核心功能之外，可根据需要安装和卸载的其他功能。
- Tags：用于描述和分类内容，加强资讯内容间的相关性，以便于检索和分享。
- 回收站：删除后的资讯内容存放位置，如需还原，可直接进回收站找回。
- 归档：将已发布的资讯内容撤消，不让此信息展示在前台页面的操作。
- 出档：将已撤消的资讯内容重新发布，让此信息重新展示在前台页面的操作。
- 退稿：拒绝网友或其他编辑所投稿件的操作。
- 提交、审核：把已写好的资讯内容提交给上级管理员审批是否通过的操作
- 草稿：已填写好的资讯内容，让其过一段时间后再展示到前台。
- 终审：资讯内容的最终展示的状态。
- 模板：预先制作好的页面布局，提供一个包含变量及数据的基本HTML框架，使同样布局的页面展示不同的数据。
- 模板方案：一套完整页面的文件夹，下面包含常用模板文件，可设置变更模板方案以及导出方案
- 资源：美工制作的静态html引用的css、img、js等存放和管理位置 
- 角色：美工制作的静态html引用的css、img、js等存放和管理位置 
- 页面静态化：生成静态HTML页面功能，包含首页、栏目页、内容页
- 采集：网络爬虫功能，预设定规则将外部网页内容保存到本系统

## 2.标签套用结构说明

嵌套在html中的标签 以[@标签名  各种属性..]开始 ,以[/@标签名] 结尾，标签里面还可以嵌套标签，就像html中的<ul><li></li></ul>。

```html
<ul class="slideshow"id="slidesImgs">
[@cms_content_list recommend='1' count='5' orderBy='2'typeId='2,3' titLen='18' channelOption='1' ]
      [#list tag_list as a]
      <li>
         <a href="${a.url}" target="_blank">
             <img src="${a.typeImg!site.defImg}"alt="${a.title}" width="100%" />
                    </a>
                       <spanclass="title">
                [@text_cut s=a.title len=titLen /]
                       </span>
      </li>
     [/#list]
 [/@cms_content_list]
</ul>
```

- 你会看到如上的[@cms_content_list…….]（文章列表）标签里面还嵌套了一层标签,它是以[#..]开始的，像这种标签不仅仅有[#list]  还有[#if]…[#else]….[/#if ] 等，

- 这些[#]是通用的，[#list tag_list  as  a] 就表示取个别名为 a （多看几处你会发现不循环的东西是不是直接用的tag_bean）,list列表在循环的时候用a来代替，在内容展示中就可以写 ${a.url} ${a.typeImg!site,defImg}  ${a.title}等

- 循环多少次是由[@属性count决定的]
- [@.../]要是在html中，它所处的位置就是<div>

```html
[@cms_content_list recommend='1' count='5' orderBy='2' titLen='18']
	[#list tag_list as a]
	[/#list]
[/@cms_content_list]
count：循环次数
titLen：文本最多显示18字符
as: 取别名
```

- [#..]是依附于[@..]标签的，就像<li>依附于<ul>一样，你可以在html直接写<li>，一样的可以解析出来，但是在cms标签里面不行，它是很严格。说明你的最外层标签它肯定是[@..]
- [#..]是对内容展示的调动，如：循环，判断等.
- :判定一个标签的是干什么的以及它会有什么样的效果，是根据[@..]来判别的，这些[@..]的标签都是以功能模块来命名的。（人家已经把名字取好了）
- html中没有自定义标签，而jeecms中是有自定义标签的。

### 1.[@cms_content_list] 文章列表

（文章列表）标签里面还嵌套了一层标签,它是以[#..	]开始的，像这种标签不仅仅有[#list]  还有[#if]…[#else]….[/#if ] 等

```html
[@cms_content_list count='6' orderBy='4' typeId='2' channelId='${c.id}' channelOption='0']
[#list tag_list as a]
    <div class="y-productlist y-top165">
        <p class="y-center y-pro390">
            <img src="${a.contentImg}">
        </p>
        <p class="y-center y-top62">
            <a class="y-smore">
                <img src="/${res}/ympp/images/y-bmore.png">
            </a>
        </p>
    </div>

标题图：<img src="${c.titleImg!}">
类型图：<img src="${c.typeImg!}">
内容图：<img src="${c.contentImg!}">

[/#list]
[/@cms_content_list]
```

作用：显示文章列表

count = '6'：调取6条数据（如果有6条）

orderBy：排序方式 0：ID降序 1：ID升序 2：发布时间降序 3：发布时间升序 4：固定级别降序，发布时间降序 5： 固定级别降序，发布时间升序 6：日访问降序（推荐）7：周访问降序 8：月访问降序 9：总访问降序 10： 日评论降序（推荐） 11： 周评论降序 12： 月评论降序 13： 总评论降序 14： 日下载降序（推荐）15： 周下载降序 16： 月下载降序 17： 总下载降序 18： 日顶降序（推荐） 19： 周顶降序 20： 月顶降序 21： 总顶降序

typeId：1 普通新闻  2图文  3焦点  4头条，类型ID，可选，允许多个类型ID，用“，”分开。 

channelId：栏目ID，允许多个栏目ID，用“，”分开。和channelpath之间二选一，ID优先级更高。遍历时用channelId选择父id

id：文章ID，允许多个文章的ID，用“，”分开。排斥其他所有删选参数

tagId：TAG ID 允许多个TAG ID，用“，”分开。和tagNames之间二选一，ID优先级更高

tagName：AG NAME 允许多个TAG NAME ，用“，”分开。

topicId：专题ID

channelPath：栏目路径，允许多个栏目路径，用“，”分开。

channelOption：栏目选项，用于单栏目情况下。 0 ：自身栏目 1 ：包含子栏目 2: 包含副栏目

siteId：站点ID，可选，允许多个站点ID，用“，”分开

recommend：是否推荐。 0 ：所有都推荐 1 ：推荐 2 ：不推荐，默认所有

title：标题，可以为null

image：标题图片， 0 ：所有 1 ：有 2 ：没有 。默认所有

excludeId：不包含的文章ID，用于按tag查询相关文章

style_list：文章列表显示样式

文字列表：

- lineHeight：行高；【行高】
- headMarkImg：列表头图片；【图片地址】
- headMark：列表头编号；【1：小黑点；2：小红点；3：单箭头；4：双箭头】
- bottomLine：下划线；【0：无；1：有】不能为空。
- dateFormat：日期格式；【java日期格式，如：yyyy-MM-dd】
- datePosition：日期位置；【1：后面左边；2：后面右边；3：前面】不能为空
- ctgForm：类别；【0：无；1：栏目；2：站点】不能为空
- showTitleStyle：显示标题样式；【0：不显示；1：显示】不能为空
- useShortTitle：是否使用简短标题；【0：不使用；1：使用】不能为空
- titLen：标题长度；【英文字母按半个计算】为空则不截断
- target：是否新窗口打开；【0：原窗口；1：新窗口】不能为空
- styleList：文章列表显示样式

图文列表：

- picWidth：图片宽度；【按百分比计算（如为24.9；即每个图片占总宽度的24.9%，每行可放四张图片）】不能为空。
- picHeight：每行图片显示高度；【按像素px计算】不能为空
- picFloa：图片是否左浮动；【0：否；1：是】不能为空。: 图片右边距； 
- rightPadding：【按像素px计算】不能为空。
- showTitleStyle：显示标题样式；【0：不显示；1：显示】不能为空
- useShortTitle：是否使用简短标题；【0：不使用；1：使用】不能为空
- titLen：标题长度；【英文字母按半个计算】为空则不截断
- target：是否新窗口打开；【0：原窗口；1：新窗口】不能为空

焦点图：

- focusType：焦点图类型；【1；2；3】不能为空
- flashWidth：flash宽度；【按像素px计算】不能为空。
- flashHeight：flash高度；【按像素px计算】不能为空。
- textHeight：文本高度；【按像素px计算】不能为空。
- useShortTitle：是否使用简短标题；【0：不使用；1：使用】不能为空
- titLen：标题长度；【英文字母按半个计算】为空则不截断

### 2.[@cms_Include] 页面模板包含标签

作用：把做好的页面引入到另外一个页面上去，一般是整个网站的一些公共部分，每个网页都需要的，比如页头，页脚。

示例：

 [#include "../include/页头顶栏.html"/]

 [#include "../include/页头导航栏.html"/]

 [#include "../include/页头搜索栏.html"/]

### 3.[@cms_ friendlink_list] 友情链接

ctgId：友情链接类别(1:文字链接   2：图片链接)

### 4.[@cms_channel]  栏目对象标签

作用：显示某个栏目

id：栏目ID

path：栏目路径

siteId：站点ID，存在时获取该站点栏目，不存在时获取当前站点栏目

效果截图：

![img](https://img-blog.csdn.net/20150505143316935?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMjE3Njk4NA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/Center)

完整代码：

```html
<div id="main">
<div class="page box">
<div class="w700 fl box">
<div class="rb_top"></div>
<div class="rb_mid box">
<div class="w300 fl">
<div class="shrd">
<h2>
    [@cms_channel id='1']<a href="${tag_bean.url}" target="_blank">${tag_bean.name}</a>		[/@cms_channel]
</h2>
<ul class="list">
    [@cms_content_list channelId='1' count='6' orderBy='4' titLen='18' descLen='40' append='...' channelOption='1'] 
        [#list tag_list as a] 
            [#if a_index = 0]
                <li class="toptext">
                    <h3><a href="${a.url}" title="${a.title}" target="_blank">${a.stit}</a></h3>
                    <a href="${a.url}" target="_blank"><img src="${a.titleImg!site.defImg}" alt="${a.title}" /></a>
                    <p>[@text_cut s=a.desc len=descLen append=append/]</p>
                </li>
            [#else]
                <li>
                    <span><a href="${a.ctgUrl}" target="_blank">[${a.ctgName}]</a></span>
                    <a href="${a.url}" title="${a.title}" target="_blank">[@text_cut
        s=a.title len=titLen append=append/]</a>
                 </li>
            [/#if] 
        [/#list] 
    [/@cms_content_list]
</ul>
</div>
</div>
<div class="rb_low"></div>
</div>
```

### 5.[@cms_channel_list] 栏目列表标签

作　用： 显示各栏目列表

praentId：存在时，获取该栏目的子栏目，不存在时，获取顶级栏目channel.parent.id

siteId：站点ID。存在时，获取该站点顶级栏目，不存在时获取当前站点顶级栏目。（仅在parentId不存在时起作用）

hasContent：是否只获取可以有内容的栏目。【0：获取所有；1：只获取可以有内容的栏目】（默认0）

linkClass：链接class

style：标签内部样式。如果指定sysContent或userContent，则该项无效。【1：普通链接列表；】（默认1）

sysTpl：使用系统模板。【0：不使用；1：使用】（默认1）

sysContent：系统内容样式。（默认0）

userContent：自定义内容样式。如果指定了系统内容样式，则该项无效。（默认0）

sys Page：系统分页样式。【0：不分页；1：样式一；2：样式二】（默认0）

user Page：自定义分页样式。如果指定了系统分页样式，则该项无效。【0：不分页；1：样式一；2：样式二】（默认0）

custom：字符串数组。用于个性化处理。（默认空数组）

### 6.[@cms_pagination/] 分页标签

（一般是跟其他标签一起使用的，一般和一些列表标签一起使用。）

sysPage：对页面显示出来的记录进行分页是否为内容分页。1：内容分页；0：栏目分页。默认栏目分页。

```html
<div class="pagebar">
[@cms_comment_page contentId=contentId count='6' orderBy='1']
<dl class="rmpl">
[#if tag_pagination.list?size = 0]
<dt><span>暂无相关评论！</span></dt>
[#else]
[#list tag_pagination.list as c]
<dt><span>${(c.commentUser.username)!"匿名网友"}</span> 于 ${c.createTime} 评论道:</dt>
<dd>${c.textHtml!}</dd>
<dd class="line"></dd>
[/#list]
<div class="pagebar">[@cms_pagination sysPage='1'/]</div>
[/#if]
</dl>
[/@cms_comment_page]
</div>
```

### 7. [@cms_Content] 文章对象标签

作用：显示某篇文章

id：文章ID

next：下一篇

channeled：栏目ID

### 8.[@cms_content_page]  文章列表分页标签

作用：对显示的文章列表进行分页

tagId：TAG ID 允许多个TAG ID，用“，”分开。和tagNames之间二选一，ID优先级更高。

tagName：TAG NAME 允许多个TAG NAME ，用“，”分开

topicId：专题ID

channelId：栏目ID，允许多个栏目ID，用“，”分开。和channelpath之间二选一，ID优先级更高

channelPath：栏目路径，允许多个栏目路径，用“，”分开

channelOption：栏目选项，用于单栏目情况下。 0 ：自身栏目 1 ：包含子栏目 2: 包含副栏目

siteId：站点ID，可选，允许多个站点ID，用“，”分开

typeId：类型ID，可选，允许多个类型ID，用“，”分开。

Recommend：是否推荐。 0 ：所有都推荐 1 ：推荐 2 ：不推荐，默认所有

title：标题，可以为null

image：标题图片， 0 ：所有 1 ：有 2 ：没有 。默认所有

orderBy：排序方式 0：ID降序 1：ID升序 2：发布时间降序 3：发布时间升序 4：固定级别降序，发布时间降序 5： 固定级别降序，发布时间升序 6：日访问降序（推荐）7：周访问降序 8：月访问降序 9：总访问降序 10： 日评论降序（推荐） 11： 周评论降序 12： 月评论降序 13： 总评论降序 14： 日下载降序（推荐）15： 周下载降序 16： 月下载降序 17： 总下载降序 18： 日顶降序（推荐） 19： 周顶降序 20： 月顶降序 21： 总顶降序

excludeId：不包含的文章ID，用于按tag查询相关文章

### 9.[@cms_topic_page] 专题分页标签

channelId：栏目ID

recommend：是否推荐。 0 ：所有都推荐 1 ：推荐 2 ：不推荐，默认所有

### 10.[@cms_topic_list] 专题列表标签

作用：显示专题列表

channeled：栏目ID

recommend：是否推荐。 0 ：所有都推荐 1 ：推荐 2 ：不推荐，默认所有

### 11.[@cms_comment_page] 评论分页标签

作用：对评论列表进行分页

siteId：站点id

contentId：内容ID

greaterThen：评论内容最大支持大于多少

checked：是否需要审核

recommend：是否推荐

orderBy：排列顺序：0 ：按评论时间降序 1 ：按评论时间升序。 默认降序

### 12.[@cms_comment_list] 评论列表标签

作用：显示评论列表

siteId：站点id

contentId：内容ID

greaterThen：评论内容最大支持大于多少

checked：是否需要审核

recommend：是否推荐

orderBy：排列顺序：0 ：按评论时间降序 1 ：按评论时间升序。 默认降序

### 13.[@cms_vote] 投票标签

作用：实现投票模块

id：投票ID 可以为空，为空则获取站点的默认投票

siteId：站点ID  默认为当前站点

### 14.[@cms_tag_list] Tag列表标签

作用：显示tag列表

### 15.[@cms_lucene_list] 搜索结果列表标签

作用：显示搜索出来的结果列表

q：搜索关键字

siteId：站点ID

channeled：栏目ID

startDate：开始时间

endDate：结束时间

### 16.[@cms_lucene_page] 搜索结果分页标签

作用： 对搜索结果分页

q：搜索关键字

siteId：站点ID

channeled：栏目ID

startDate：开始时间

endDate：结束时间

注意：[@cms_lucene_list]与[@cms_lucene_page]的效果显示是不一样的，[@cms_lucene_list]其显示的结果 由其内的熟悉count和搜索结果的总数决定的，如果搜索结果的总数大于count则显示的结果就为count设定的值，如果搜索结果的总数小于 count设定的值则显示搜索结果的总数。而[@cms_lucene_page]不一样，它始终是会显示出所有的搜索结果来的，只是每一页显示的值是由 count决定的，所以，我建议，一般情况下，还是用[@cms_lucene_page]比较好，因为用它既能显示出搜索结果的分页又能正确的显示出搜 索出来的结果总数。

### 17.[@cms_guestbook_list] 留言列表标签

作用：显示用户的留言列表

siteId：站点ID

ctgId：类别ID，用于调用不同类别

checked：是否审核后显示。0，不审核 1，审核 默认是不审核

### 18.[@process_time/] 页面处理时间标签

作用：显示处理某个页面所需要的时间











