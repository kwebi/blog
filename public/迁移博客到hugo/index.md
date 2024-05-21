# 迁移博客到hugo+LoveIt


### 前言

之前一直使用 hexo+next 主题，但是遇到个问题就是每次换环境或者换电脑，需要重新安装 node，node_moudles，从新装主题，由于 npm 更新或者包源更换等原因，好久没用，现在失败，所以不想折腾 hexo 了，试一下简单的 hugo+LoveIt 主题。

### 过程

主要参考[从零开始的 Hugo 博客搭建](https://zhuanlan.zhihu.com/p/644838582)这个系列文章。

#### 下载

- 从 Github 下载 [Hugo](https://link.zhihu.com/?target=https%3A//github.com/gohugoio/hugo/releases) 对应版本的（由于文件过多，所以可能要点 show all 才能看见）压缩包，注意要下载带扩展 extend 版本的，不然后续可能会报错。然后解压至 D:\Programs\bin。
- 配置系统路径：
- 打开开始菜单栏，搜索高级系统设置。
- 打开高级系统设置，点击环境变量，在下半部分的系统变量中找到 Path ，点击，点击新建。把路径 D:\Programs\bin 输进去，再确定即可。打开命令行，查看 Hugo 的版本，以判断上面步骤是否成功，代码如下：

```bash
#查看hugo是否安装成功
hugo version
```

#### 使用

```bash
# 终端中前往 D:\Hblog 创建一个新的网站,新建blog文件夹，可换成自己想要的名字
hugo new site blog
# 安装自己喜欢的主题（这里以 LoveIt 为例）
## Git 的下载这里不涉及，若有需求可以自行查找
## 如果要用 vercel 的话，不要用 git ，直接 下载源码再放到 themes 目录下面就行，具体可见文章第二部分
cd blog
git init
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt

#若主题要更新
cd themes/LoveIt
git pull

# 回退到 D:\Hblog\blog设置 conf.toml 的 theme
cd ..
cd ..
# 添加主题配置
echo theme = "LoveIt" >> config.toml
```

- 下载好主题后，`themes/LoveIt` 的 `exampleSite` 目录一般有一个 `config.toml` 配置文件，根据注释做一些基本的修改，再复制到你的根目录下的 config.tmol 文件。这里直接给出修改后的 `config.toml` 文件：

```toml
baseURL = "https://kwebi.github.io"

# 更改使用 Hugo 构建网站时使用的默认主题
theme = "LoveIt"

# 网站标题
title = "Kwebi's blog"
defaultContentLanguage = "zh-cn"
# 网站语言, 仅在这里 CN 大写 ["en", "zh-CN", "fr", "pl", ...]
languageCode = "zh-CN"
# 语言名称 ["English", "简体中文", "Français", "Polski", ...]
languageName = "简体中文"

# 是否包括中日韩文字
hasCJKLanguage = true

# 默认每页列表显示的文章数目
paginate = 12
# 谷歌分析代号 [UA-XXXXXXXX-X]
googleAnalytics = ""
# 版权描述，仅仅用于 SEO
copyright = ""

# 是否使用 robots.txt
enableRobotsTXT = true
# 是否使用 git 信息
#enableGitInfo = true
# 是否使用 emoji 代码
enableEmoji = true

# 忽略一些构建错误
ignoreErrors = ["error-remote-getjson", "error-missing-instagram-accesstoken"]

# 作者配置
[author]
  name = "kwebi"
  email = ""

# 菜单配置
[menu]
  [[menu.main]]
    weight = 1
    identifier = "posts"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    name = "文章"
    url = "/posts/"
    # 当你将鼠标悬停在此菜单链接上时, 将显示的标题
    title = ""
  [[menu.main]]
    weight = 2
    identifier = "tags"
    pre = ""
    post = ""
    name = "标签"
    url = "/tags/"
    title = ""
  [[menu.main]]
    weight = 3
    identifier = "categories"
    pre = ""
    post = ""
    name = "分类"
    url = "/categories/"
    title = ""

# Hugo 解析文档的配置
[markup]
  # 语法高亮设置 (https://gohugo.io/content-management/syntax-highlighting)
  [markup.highlight]
    # false 是必要的设置 (https://github.com/dillonzq/LoveIt/issues/158)
    codeFences = true
    guessSyntax = true
    lineNos = true
    lineNumbersInTable = true
    # false 是必要的设置
    # (https://github.com/dillonzq/LoveIt/issues/158)
    # Goldmark 是 Hugo 0.60 以来的默认 Markdown 解析库
    noClasses = false
  # Goldmark 是 Hugo 0.60 以来的默认 Markdown 解析库
  [markup.goldmark]
    [markup.goldmark.extensions]
      definitionList = true
      footnote = true
      linkify = true
      strikethrough = true
      table = true
      taskList = true
      typographer = true
    [markup.goldmark.renderer]
      # 是否在文档中直接使用 HTML 标签
      unsafe = true
  # 目录设置
  [markup.tableOfContents]
    startLevel = 2
    endLevel = 6
[params]
  # 网站默认主题样式 ["auto", "light", "dark"]
  defaultTheme = "light"
  # 公共 git 仓库路径，仅在 enableGitInfo 设为 true 时有效
  #gitRepo = ""
  # LoveIt 新增 | 0.1.1 哪种哈希函数用来 SRI, 为空时表示不使用 SRI
  # ["sha256", "sha384", "sha512", "md5"]
  fingerprint = "sha256"
  # LoveIt 新增 | 0.2.0 日期格式
  dateFormat = "2006-01-02"
  # 网站标题, 用于 Open Graph 和 Twitter Cards
  title = "Kwebi的博客"
  # 网站描述, 用于 RSS, SEO, Open Graph 和 Twitter Cards
  description = "Kwebi的博客"
  # 网站图片, 用于 Open Graph 和 Twitter Cards
  images = ["首页中间的图片"]

# 页面头部导航栏配置
[params.header]
  # 桌面端导航栏模式 ["fixed", "normal", "auto"]
  desktopMode = "normal"
  # 移动端导航栏模式 ["fixed", "normal", "auto"]
  mobileMode = "normal"
  # LoveIt 新增 | 0.2.0 页面头部导航栏标题配置
  [params.header.title]
    # LOGO 的 URL
    logo = ""
    # 标题名称
    name = "Kwebi的博客"
    # 你可以在名称 (允许 HTML 格式) 之前添加其他信息, 例如图标
    pre = ""
    # 你可以在名称 (允许 HTML 格式) 之后添加其他信息, 例如图标
    post = ""
    # LoveIt 新增 | 0.2.5 是否为标题显示打字机动画
    typeit = false

# 页面底部信息配置
[params.footer]
  enable = true
  # LoveIt 新增 | 0.2.0 自定义内容 (支持 HTML 格式)
  custom = ''
  # LoveIt 新增 | 0.2.0 是否显示 Hugo 和主题信息
  hugo = false
  # LoveIt 新增 | 0.2.0 是否显示版权信息
  copyright = true
  # LoveIt 新增 | 0.2.0 是否显示作者
  author = true
  # 网站创立年份
  since = 2018
  # ICP 备案信息，仅在中国使用 (支持 HTML 格式)
  icp = ''
  # 许可协议信息 (支持 HTML 格式)
  license = '<a rel="license external nofollow noopener noreffer" href="https://creativecommons.org/licenses/by-nc/4.0/" target="_blank">CC BY-NC 4.0</a>'

# LoveIt 新增 | 0.2.0 Section (所有文章) 页面配置
[params.section]
  # section 页面每页显示文章数量
  paginate = 20
  # 日期格式 (月和日)
  dateFormat = "01-02"
  # RSS 文章数目
  rss = 10

# LoveIt 新增 | 0.2.0 List (目录或标签) 页面配置
[params.list]
  # list 页面每页显示文章数量
  paginate = 20
  # 日期格式 (月和日)
  dateFormat = "01-02"
  # RSS 文章数目
  rss = 10

# LoveIt 新增 | 0.2.0 应用图标配置
[params.app]
  # 当添加到 iOS 主屏幕或者 Android 启动器时的标题, 覆盖默认标题
  title = ""
  # 是否隐藏网站图标资源链接
  noFavicon = false
  # 更现代的 SVG 网站图标, 可替代旧的 .png 和 .ico 文件
  svgFavicon = ""
  # Android 浏览器主题色
  themeColor = "#ffffff"
  # Safari 图标颜色
  iconColor = "#5bbad5"
  # Windows v8-10磁贴颜色
  tileColor = "#da532c"

# LoveIt 新增 | 0.2.0 搜索配置
[params.search]
  enable = true
  # 搜索引擎的类型 ["lunr", "algolia"]
  type = "algolia"
  # 文章内容最长索引长度
  contentLength = 4000
  # 搜索框的占位提示语
  placeholder = ""
  # LoveIt 新增 | 0.2.1 最大结果数目
  maxResultLength = 10
  # LoveIt 新增 | 0.2.3 结果内容片段长度
  snippetLength = 50
  # LoveIt 新增 | 0.2.1 搜索结果中高亮部分的 HTML 标签
  highlightTag = "em"
  # LoveIt 新增 | 0.2.4 是否在搜索索引中使用基于 baseURL 的绝对路径
  absoluteURL = false
  ##注册使用，前往：https://www.algolia.com/
  [params.search.algolia]
    index = ""
    appID = ""
    searchKey = ""

# 主页配置
[params.home]
  # LoveIt 新增 | 0.2.0 RSS 文章数目
  rss = 10
  # 主页个人信息
  [params.home.profile]
    enable = true
    # Gravatar 邮箱，用于优先在主页显示的头像
    gravatarEmail = ""
    # 主页显示头像的 URL
    avatarURL = ""
    # LoveIt 更改 | 0.2.7 主页显示的网站标题 (支持 HTML 格式)
    title = ""
    # 主页显示的网站副标题 (允许 HTML 格式)
    subtitle = "欢迎来到我的博客"
    # 是否为副标题显示打字机动画
    typeit = true
    # 是否显示社交账号
    social = true
    # LoveIt 新增 | 0.2.0 免责声明 (支持 HTML 格式)
    disclaimer = ""
  # 主页文章列表
  [params.home.posts]
    enable = true
    # 主页每页显示文章数量
    paginate = 6
    # LoveIt 删除 | 0.2.0 被 params.page 中的 hiddenFromHomePage 替代
    # 当你没有在文章前置参数中设置 "hiddenFromHomePage" 时的默认行为
    defaultHiddenFromHomePage = false

# 作者的社交信息设置
[params.social]
  GitHub = ""
  Linkedin = ""
  Twitter = ""
  Instagram = ""
  Facebook = ""
  Telegram = ""
  Medium = ""
  Gitlab = ""
  Youtubelegacy = ""
  Youtubecustom = ""
  Youtubechannel = ""
  Tumblr = ""
  Quora = ""
  Keybase = ""
  Pinterest = ""
  Reddit = ""
  Codepen = ""
  FreeCodeCamp = ""
  Bitbucket = ""
  Stackoverflow = ""
  Weibo = ""
  Odnoklassniki = ""
  VK = ""
  Flickr = ""
  Xing = ""
  Snapchat = ""
  Soundcloud = ""
  Spotify = ""
  Bandcamp = ""
  Paypal = ""
  Fivehundredpx = ""
  Mix = ""
  Goodreads = ""
  Lastfm = ""
  Foursquare = ""
  Hackernews = ""
  Kickstarter = ""
  Patreon = ""
  Steam = ""
  Twitch = ""
  Strava = ""
  Skype = ""
  Whatsapp = ""
  Zhihu = ""
  Douban = ""
  Angellist = ""
  Slidershare = ""
  Jsfiddle = ""
  Deviantart = ""
  Behance = ""
  Dribbble = ""
  Wordpress = ""
  Vine = ""
  Googlescholar = ""
  Researchgate = ""
  Mastodon = ""
  Thingiverse = ""
  Devto = ""
  Gitea = ""
  XMPP = ""
  Matrix = ""
  Bilibili = ""
  Discord = ""
  DiscordInvite = ""
  Lichess = ""
  ORCID = ""
  Pleroma = ""
  Kaggle = ""
  MediaWiki= ""
  Plume = ""
  HackTheBox = ""
  RootMe= ""
  Phone = ""
  Email = ""
  RSS = true # LoveIt 新增 | 0.2.0

# LoveIt 更改 | 0.2.0 文章页面全局配置
[params.page]
  # LoveIt 新增 | 0.2.0 是否在主页隐藏一篇文章
  hiddenFromHomePage = false
  # LoveIt 新增 | 0.2.0 是否在搜索结果中隐藏一篇文章
  hiddenFromSearch = false
  # LoveIt 新增 | 0.2.0 是否使用 twemoji
  twemoji = false
  # 是否使用 lightgallery
  lightgallery = false
  # LoveIt 新增 | 0.2.0 是否使用 ruby 扩展语法
  ruby = true
  # LoveIt 新增 | 0.2.0 是否使用 fraction 扩展语法
  fraction = true
  # LoveIt 新增 | 0.2.0 是否使用 fontawesome 扩展语法
  fontawesome = true
  # 是否在文章页面显示原始 Markdown 文档链接
  linkToMarkdown = false
  # LoveIt 新增 | 0.2.4 是否在 RSS 中显示全文内容
  rssFullText = false
  # LoveIt 新增 | 0.2.0 目录配置
  [params.page.toc]
    # 是否使用目录
    enable = true
    # LoveIt 新增 | 0.2.9 是否保持使用文章前面的静态目录
    keepStatic = false
    # 是否使侧边目录自动折叠展开
    auto = true
  # LoveIt 新增 | 0.2.0 代码配置
  [params.page.code]
    # 是否显示代码块的复制按钮
    copy = true
    # 默认展开显示的代码行数
    maxShownLines = 50
# LoveIt 更改 | 0.2.0 KaTeX 数学公式
[params.page.math]
  enable = true
  # LoveIt 更改 | 0.2.11 默认行内定界符是 $ ... $ 和 \( ... \)
  inlineLeftDelimiter = ""
  inlineRightDelimiter = ""
  # LoveIt 更改 | 0.2.11 默认块定界符是 $$ ... $$, \[ ... \], \begin{equation} ... \end{equation} 和一些其它的函数
  blockLeftDelimiter = ""
  blockRightDelimiter = ""
  # KaTeX 插件 copy_tex
  copyTex = true
  # KaTeX 插件 mhchem
  mhchem = true
# LoveIt 新增 | 0.2.0 Mapbox GL JS 配置
[params.page.mapbox]
  # Mapbox GL JS 的 access token
  accessToken = ""
  # 浅色主题的地图样式
  lightStyle = "mapbox://styles/mapbox/light-v10?optimize=true"
  # 深色主题的地图样式
  darkStyle = "mapbox://styles/mapbox/dark-v10?optimize=true"
  # 是否添加 NavigationControl
  navigation = true
  # 是否添加 GeolocateControl
  geolocate = true
  # 是否添加 ScaleControl
  scale = true
  # 是否添加 FullscreenControl
  fullscreen = true
# LoveIt 更改 | 0.2.0 文章页面的分享信息设置
[params.page.share]
  enable = true
  Twitter = false
  Facebook = false
  Linkedin = false
  Whatsapp = false
  Pinterest = false
  Tumblr = false
  HackerNews = false
  Reddit = false
  VK = false
  Buffer = false
  Xing = false
  Line = false
  Instapaper = false
  Pocket = false
  Flipboard = false
  Weibo = false
  Blogger = false
  Baidu = false
  Odnoklassniki = false
  Evernote = false
  Skype = false
  Trello = false
  Mix = false
# LoveIt 更改 | 0.2.0 评论系统设置
[params.page.comment]
  enable = false
  # Disqus 评论系统设置
  [params.page.comment.disqus]
    # LoveIt 新增 | 0.1.1
    enable = false
    # Disqus 的 shortname，用来在文章中启用 Disqus 评论系统
    shortname = ""
  # Gitalk 评论系统设置
  [params.page.comment.gitalk]
    # LoveIt 新增 | 0.1.1
    enable = false
    owner = ""
    repo = ""
    clientId = ""
    clientSecret = ""
  # Valine 评论系统设置
  [params.page.comment.valine]
    enable = false
    appId = ""
    appKey = ""
    placeholder = ""
    avatar = "mp"
    meta= ""
    pageSize = 10
    # 为空时自动适配当前主题 i18n 配置
    lang = ""
    visitor = true
    recordIP = true
    highlight = true
    enableQQ = false
    serverURLs = ""
    # LoveIt 新增 | 0.2.6 emoji 数据文件名称, 默认是 "google.yml"
    # ["apple.yml", "google.yml", "facebook.yml", "twitter.yml"]
    # 位于 "themes/LoveIt/assets/lib/valine/emoji/" 目录
    # 可以在你的项目下相同路径存放你自己的数据文件:
    # "assets/lib/valine/emoji/"
    emoji = ""
  # Facebook 评论系统设置
  [params.page.comment.facebook]
    enable = false
    width = "100%"
    numPosts = 10
    appId = ""
    # 为空时自动适配当前主题 i18n 配置
    languageCode = "zh_CN"
# LoveIt 新增 | 0.2.0 Telegram Comments 评论系统设置
[params.page.comment.telegram]
  enable = false
  siteID = ""
  limit = 5
  height = ""
  color = ""
  colorful = true
  dislikes = false
  outlined = false
# LoveIt 新增 | 0.2.0 Commento 评论系统设置
[params.page.comment.commento]
  enable = false
# LoveIt 新增 | 0.2.5 utterances 评论系统设置
[params.page.comment.utterances]
  enable = false
  # owner/repo
  repo = ""
  issueTerm = "pathname"
  label = ""
  lightTheme = "github-light"
  darkTheme = "github-dark"
# giscus comment 评论系统设置 (https://giscus.app/zh-CN)
[params.page.comment.giscus]
  # 你可以参考官方文档来使用下列配置
  enable = false
  repo = ""
  repoId = ""
  category = "Announcements"
  categoryId = ""
  # 为空时自动适配当前主题 i18n 配置
  lang = ""
  mapping = "pathname"
  reactionsEnabled = "1"
  emitMetadata = "0"
  inputPosition = "bottom"
  lazyLoading = false
  lightTheme = "light"
  darkTheme = "dark"
# LoveIt 新增 | 0.2.7 第三方库配置
[params.page.library]
  [params.page.library.css]
    # someCSS = "some.css"
    # 位于 "assets/"
    # 或者
    # someCSS = "https://cdn.example.com/some.css"
  [params.page.library.js]
    # someJavascript = "some.js"
    # 位于 "assets/"
    # 或者
    # someJavascript = "https://cdn.example.com/some.js"
# LoveIt 更改 | 0.2.10 页面 SEO 配置
[params.page.seo]
  # 图片 URL
  images = []
  # 出版者信息
  [params.page.seo.publisher]
    name = ""
    logoUrl = ""

# LoveIt 新增 | 0.2.5 TypeIt 配置
[params.typeit]
  # 每一步的打字速度 (单位是毫秒)
  speed = 100
  # 光标的闪烁速度 (单位是毫秒)
  cursorSpeed = 1000
  # 光标的字符 (支持 HTML 格式)
  cursorChar = "|"
  # 打字结束之后光标的持续时间 (单位是毫秒, "-1" 代表无限大)
  duration = -1

# 网站验证代码，用于 Google/Bing/Yandex/Pinterest/Baidu
[params.verification]
  google = ""
  bing = ""
  yandex = ""
  pinterest = ""
  baidu = ""

# LoveIt 新增 | 0.2.10 网站 SEO 配置
[params.seo]
  # 图片 URL
  image = ""
  # 缩略图 URL
  thumbnailUrl = ""

# LoveIt 新增 | 0.2.0 网站分析配置
[params.analytics]
  enable = true
  [params.analytics.google]
    id = ""
    # whether to anonymize IP
    # 是否匿名化用户 IP
    anonymizeIP = true
   # Fathom Analytics
   # 百度统计,自己配置的
  [params.analytics.baidu]
    id = ""
#  Cookie 许可配置
#[params.cookieconsent]
  #enable = true
  # 用于 Cookie 许可横幅的文本字符串
  #[params.cookieconsent.content]
    #message = ""
    #dismiss = ""
    #link = ""

#  第三方库文件的 CDN 设置
[params.cdn]
  # CDN 数据文件名称, 默认不启用
  # ["jsdelivr.yml"]
  # 位于 "themes/LoveIt/assets/data/cdn/" 目录
  # 可以在你的项目下相同路径存放你自己的数据文件:
  # "assets/data/cdn/"
  data = ""

#  兼容性设置
[params.compatibility]
  # 是否使用 Polyfill.io 来兼容旧式浏览器
  polyfill = false
  # 是否使用 object-fit-images 来兼容旧式浏览器
  objectFit = false
# 网站地图配置
[sitemap]
  changefreq = "weekly"
  filename = "sitemap.xml"
  priority = 0.5

# Permalinks 配置
[Permalinks]
  # posts = ":year/:month/:filename"
  posts = ":filename"

# 隐私信息配置
[privacy]
  #  Google Analytics 相关隐私 (被 params.analytics.google 替代)
  [privacy.googleAnalytics]
    # ...
  [privacy.twitter]
    enableDNT = true
  [privacy.youtube]
    privacyEnhanced = true

# 用于输出 Markdown 格式文档的设置
[mediaTypes]
  [mediaTypes."text/plain"]
    suffixes = ["md"]

# 用于输出 Markdown 格式文档的设置
[outputFormats.MarkDown]
  mediaType = "text/plain"
  isPlainText = true
  isHTML = false

# 用于 Hugo 输出文档的设置
[outputs]
  home = ["HTML", "RSS", "JSON"]
  page = ["HTML", "MarkDown"]
  section = ["HTML", "RSS"]
  taxonomy = ["HTML", "RSS"]
  taxonomyTerm = ["HTML"]
```

- 配置好后，修改 D:\Hblog\blog\archetypes\default.md 的模板样式:

```yaml
---
title: "{{ replace .TranslationBaseName "-" " " | title }}"
subtitle: ""
date: {{ .Date }}
lastmod: {{ .Date }}
draft: true
author: ""
authorLink: ""
license: ""
tags: [""]
categories: [""]
featuredImage: ""
featuredImagePreview: ""
summary: ""
hiddenFromHomePage: false
hiddenFromSearch: false
toc:
  enable: true
  auto: true
mapbox:
share:
  enable: true
comment:
  enable: true
---
```

- 尝试发一篇文章：

```bash
hugo new posts/从零开始的hugo教程.md
```

- 编辑该 markdown 文件即可。

- 在终端中运行一下以下命令

```bash
hugo server
#注意若 content_name.md 的参数 draft 为true，则执行上命令后文章并不显示
# 若要显示 draft 为 true 的草稿，则用下命令
hugo server -D
# 若要在之后网页中显示文章，则要把 draft 改为 false
```

- 根据终端提示：打开`http://localhost:1313/`即可在本地预览访问。

#### gitPages

1. 将源文件推送到 github

```bash
#终端中前往 D:\Hblog\blog 进行 git 初始化 ，已经初始化后不比再初始化
git init

#检查是否有改变
git status

#将当前文件夹，提交到暂存区
git add .

#交到版本库
git commit -m "First"

#创建分支
git branch -M main

#确定要推送的远程仓库
git remote add origin https://github.com/<用户名>/<仓库名>.git

#推送到远程仓库，旧仓库可能是master
git push -u origin main
```

- 后续修改了想继续推送到仓库

```bash
git status
git add .
git commit -m "相关修改信息"
git push
```

> 提示：Github 是不会把空文件夹同步的，这个问题不大，不影响后面的操作。

2. 利用 github Action自动化部署

Hugo 默认会将生成的静态网页文件存放在 public/ 目录下，我们可以通过将 public/ 目录初始化为 git 仓库并关联我们的 kwebi/kwebi.github.io 远程仓库来推送我们的网页静态文件。每次修改后推送到该仓库就能修改了。通过上述命令我们可以手动发布我们的静态文件，但还是有以下弊端：
- 发布步骤还是比较繁琐，本地调试后还需要切换到 public/ 目录进行上传
- 无法对博客 .md 源文件进行备份与版本管理

[Hugo 白话文 | GitHub Action 自动部署](https://zhuanlan.zhihu.com/p/470491820?utm_id=0)

#### 创建ssh key
```bash
ssh-keygen -t rsa -b 4096 -C "1565718281@qq.com"
# Generating public/private rsa key pair. Enter file in which to save the key (/Users/tc/.ssh/id_rsa):

# 输入你需要指定的文件，比如 /.ssh/id_rsa_hugo_deploy
```
#### 配置博客静态资源仓库的deploy keys

打开github网页，把刚刚生成的公钥（.pub结尾）放到xxx.github.io仓库的deploy keys里面

#### 配置博客源内容仓库的 Secrets

打开github网页，把刚刚生成的私钥放到blog源仓库的repository secrets里面，记住自定义的秘钥名字，后面要用。

### github action

- 添加 gh-pages.yml 文件
  在 D:\Hblog\blog 下新建一个文件，名称为.github，然后在.github 夹下新建一个文件夹 workflows，再在 workflows 文件夹下新建一个文件叫 gh-pages.yml 在 gh-pages.yml 输入以下内容后保存。

把这次修改同步到 github
```yaml
name: Deploy Hugo Site to Github Pages on Master Branch

on:
  push:
    branches:
      - main

jobs:
  build-deploy:
    runs-on: ubuntu-latest #不要用旧的，不然会失败
    steps:
      - uses: actions/checkout@v2
        with:
          submodules: true  # Fetch Hugo themes (true OR recursive)
          fetch-depth: 0    # Fetch all history for .GitInfo and .Lastmod

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '0.76.0'
          # extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          deploy_key: ${{ secrets.BLOG_SECRET }}
          external_repository: kwebi/kwebi.github.io # remote branch
          publish_dir: "./docs"
        #   cname: blog.funnycode.org.cn          
          keep_files: false # remove existing files
          publish_branch: main  # deploying branch
          commit_message: ${{ github.event.head_commit.message }}

```

- publish_dir 指定发布的目录，./docs 指 blog 项目下的 docs 目录下的内容会被发布
- publish_branch 发布到 kwebi.github.io 项目的 docs 分支
- secrets.BLOG_SECRET 的 BLOG_SECRET 则是上面设置 Private Key 的变量名
<!-- - cname 必须要配置好，和下文提到的 Setting 里面配置图对应 -->

```bash
git status
git add .
git commit -m "new"
git push
```
成功后，找到刚刚的 Github 仓库，点击 Actions ，就可以看到我们的网站部署成功。

#### kwebi.github.io 配置

- 配置 Setting
`publish_dir` 如果是`"./docs"` 相应的gitpages Source的目录也要更改

- 如果github上面仓库改名后，本地git remote也需对应更新
```bash
git remote -v #查看remote仓库有哪些
git remote set-url origin https://github.com/username/newname.git #origin可自定义
git push origin <branch name> #可以是main或者master等
```


