> 笔者最初在 cnblog 上面发了很多随笔（水文），后面感觉广告有点多，并且难于管理文章，于是破罐破摔（不要学我）搭建了自己的博客。后来，我折腾过 wordpress、jeklly、github Pages（hexo） 和 gitee Pages 等等，既放不下 cnblog 上的流量与互动（毕竟上面还有很多青春），也放不下 github Pages 的清爽。于是时隔多日后，我打算构建这样一条工作流，每次写文章时可以方便的更新与维护。

## 软件安装

首先 vscode 与 typora 的安装不多赘述，picgo 是一个图床工具，可以方便的将图片上传到对象存储，你可以选择不使用它，但我还是强烈建议尝试这个高效的工具，对象存储很便宜，作为图床再合适不过了。

picgo 可以通过拖拽、Url、剪贴板等方式将图片快速上传到对象存储，并快速复制上传后图片的链接。

picgo：[Molunerfinn/PicGo: :rocket:A simple & beautiful tool for pictures uploading built by vue-cli-electron-builder](https://github.com/Molunerfinn/PicGo)

各种云的配置方法：[配置手册 | PicGo](https://picgo.github.io/PicGo-Doc/zh/guide/config.html#图床区)。另外，picgo 还支持设置自定义域名、图片时间戳重命名等功能，读者可以多多探索一下。

下图给出一个例子，在购买好腾讯云 cos 服务之后，会得到 Bucket ID 等等信息，其中 AppID 是用户 ID，目前（2024/11/13） Bucket ID 后面的数字就是 appid，填好之后就可以上传了。

<img src="https://blog-1256878123.cos.ap-nanjing.myqcloud.com/img/202411132224410.png" alt="image-20241113222406587" style="zoom:50%;" />

picgo 提供了 vs code 的插件：[PicGo/vs-picgo: A VSCode plugin of PicGo](https://github.com/PicGo/vs-picgo)

所以如果你选择使用 vs code 来编辑博客的话，也可以选用 picgo 来做图床工具。

本文选择使用 typora 来编辑博客，找到 typora 偏好设置中的图片部分，在上传服务设定那一栏找到 PicGo（app），并选择安装目录，然后点击「验证图片上传选项」进行测试。

<img src="https://blog-1256878123.cos.ap-nanjing.myqcloud.com/img/202411132235233.png" alt="image-20241113223525947" style="zoom:50%;" />

然后，你就可以在 typora 粘贴图片后发现有一个 tip，提醒你选择图片的路径。

## 工作流搭建

本文主要基于两种博客（cnblog，github pages）来搭建工作流：

- cnblog 提供了 vs code 插件，可以方便的新建、下载、发布随笔（博客）。
- github pages 我使用 mkdocs 搭建，这是一个静态网站生成器，不同于普通博客组织文章的方式，而是以一种树状结构来组织文章（类似很多软件的官方文档组织形式）。选用这个的理由是可以更好的形成知识结构，也方便之后查找。

安装 cnblog 插件：[博客园 cnblogs 客户端 - Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=cnblogs.vscode-cnb)，登录 cnblog 账号，然后设置好工作空间（设置到 mkdocs 的文章目录，当然如果是别的博客，例如 hexo 也是同理）。

首先新建好 md 文件，编写好博客内容，使用 `ctrl+p` 调出 vscode 命令面板，输出 `> cnblogs post`，选择上传博文，即可把当前博客上传到博客园。

<img src="https://blog-1256878123.cos.ap-nanjing.myqcloud.com/img/202411132304680.png" alt="image-20241113230401157" style="zoom:50%;" />

如果是要修改之前的老文章，可以在博客园插件的「随笔列表」中下载文件，插件会将文件自动放在工作空间，你可以随意挪动、修改文件名，本地文件与博客园文章的关联关系并不会断。

> 需要注意的一点是，在随笔列表中点击查看文件，都会自动把博客下载到本地。

然后，在 mkdocs 一边，由于我搭建了 github workflow，因此只需要将代码 push 到 master 即可，它会自动部署 pages。

代码可以参考：[kpole.github.io/.github/workflows/main.yml at master · kpole/kpole.github.io](https://github.com/kpole/kpole.github.io/blob/master/.github/workflows/main.yml)

自此，除去写文章本身之外，只需花不到一分钟的时间即可同时发布两个平台。