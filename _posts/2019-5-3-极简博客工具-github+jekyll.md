---
layout: post
title: jekyll+github构造极简个人博客
--- 

现在的博客平台很多，直接注册个账号就可以使用。如果你不想依赖他人平台，也可以自己购买服务器搭建。我的博客平台就是自己搭建的，但这两者都不符合“极简”原则。很多时候，我们只需要在网络上，有那么一块属于自己的空间，不用费很多心思去维护，也没有垃圾消息、广告的骚扰。除了自己的文字，什么都不需要有。这是我喜欢的风格。Keep It Stupid Simple。如果你也在寻找这样的一个平台，我推荐 Github.com。

我对Github.com最早的印象，它是一个开源代码托管平台，开发人员会比较热衷于使用。最近在别人的推荐下，我也玩起了 github.com。用了一段时间，完全颠覆了我之前的看法。上面除了代码，还有其他地方难以找到的工具，比如我用的翻墙软件。有很多的原创文章，有些还是涉及敏感话题的。前两天我搜一本电子书，用google搜了半天，基本都是要积分才能下载。转念一想，何不用github.com试试？一搜就搜到了，完全的免费。随着深入的使用，我相信还会有更多的惊喜等着我。
在github.com上注册了账号后，就自动获得了空间和域名，默认免费的空间好像是1G，对于博客来说也足够了。域名的格式很很好记：https://your_account.github.io。再配合jekyll，就可以搭建自己的博客了。五一假期在家捣腾了一下，接下来就分享下我的操作过程。
（注：因为github.com采用git作为版本管理工具，最好能了解下git的相关命令，但这不是必须的。）

1. 注册github.com账号，这一步就不具体介绍了。
2. 登录成功后，在左上角搜索框搜索“jekyll-now” 项目，进入该项目空间，如下图。点击左上角的Fork按钮，复制到自己的仓库（Repository），并把仓库名改成：xxx.github.io。这个名称就是博客的域名，浏览器输入https://xxx.github.io 就可直接访问。
![image](http://yanzhiww.top/wordpress/wp-content/uploads/2019/05/jekyll-now-1024x335.jpg)
3. 进入刚Fork过来的“xxx.github.io”的仓库，点击“Setting”，进入设置页面。
![image](http://yanzhiww.top/wordpress/wp-content/uploads/2019/05/git-setting-1024x291.jpg)
4. 启用“Github pages”功能
![image](http://yanzhiww.top/wordpress/wp-content/uploads/2019/05/github-pages-1024x678.jpg)
5. 到这里，你的个人博客已经搭建好了，直接访问https://xxx.github.io 就能看到效果。是不是很简洁？接下来再做一些个性化的配置。
6. 在仓库中找到_config.yml文件，点击打开，如下图。点击右边红框中的编辑图标，name是你的博客名称，description是博客的副标题，改成你自己的就行。然后点击“commit”按钮保存。
![image](http://yanzhiww.top/wordpress/wp-content/uploads/2019/05/github-config-1024x538.jpg)
7. 你的文章需要放到_post文件夹下才会被自动识别并展示，。文件统一采用markdown格式，文件名的格式应遵循格式：“yyyy-m-d-file name.md”。_post文件夹下默认的文件，直接删除就可。
![image](http://yanzhiww.top/wordpress/wp-content/uploads/2019/05/git-posts-1024x480.jpg)
8. 上传博客文章，写好文章后，点击“upload files”，上传文件到_posts目录下。也可以点击“create new file”，在线创建一个md文件，然后把文章内容复制过来。文章头部必须加上以下内容：

```
---
layout: post
title: 你的文章标题
---
```
![image](http://yanzhiww.top/wordpress/wp-content/uploads/2019/05/github-upload-1024x402.jpg)
9. 再次打开https://xxx.github.io，博客标题已经变成了你设置的标题，新上传的文章也显示在首页上了。因为是静态页面，需要清下浏览器缓存，才会看到效果。
![image](http://yanzhiww.top/wordpress/wp-content/uploads/2019/05/github_home-1024x678.jpg)
10. 修改about页面，找到about.md这个文件，直接修改即可。

11. 默认的首页是没有分页功能的，影响使用。下面给首页加上分页功能。

a. 打开_config.yml文件，在末尾加上以下两行配置：
```
paginate: 5   # 每页显示5篇文章
paginate_path: “/page:num”
```
b. 下载“index.zip”，解压后，将里面的index.html文件直接替换掉仓库里的index.html文件。 [下载](http://yanzhiww.top/wordpress/wp-content/uploads/2019/05/index.zip)

至此，您的博客系统旧搭建好了，以后定期往_posts文件里上传文章就可。

 