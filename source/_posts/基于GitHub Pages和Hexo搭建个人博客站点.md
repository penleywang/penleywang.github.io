在GitHub上创建仓库
===

# 注册GitHub帐号

GitHub官网地址：[https://github.com/](https://github.com/)

注意：需要邮箱认证。比如：我申请的帐号名为：penleywang

# 创建个人博客源码仓库

创建一个仓库，仓库名称为：penleywang.github.io

注意：这里按GitHub Pages要求，仓库名称格式必须为：<你的帐号名>.github.io

描述为：个人博客

访问级别设置为：Public，本来就是给别人看的嘛！

勾选下方同时生成 README文件的复选框。按照Git Hub Pages要求，**必须**要有一个README文件，否则访问会发生404错误，并且提示 **There isn't a GitHub Pages site here.** 

创建完成后，在master分支下，只有一个REAME.md文件。

# 安装Hexo

以Windows 10系统为例，简单介绍Hexo环境安装，详细安装请参见官方文档：[https://hexo.io/docs/](https://hexo.io/docs/)

前提：

Node.js（至少8.6，建议10及以上），官网：https://nodejs.org，下载地址：https://nodejs.org/en/download/

Git，官网：https://git-scm.com/，下载地址：https://git-scm.com/download/win

Git分服务端和客户端，我们这里装的是服务端和命令行客户端（当然也带了个客户端UI，但感觉很弱）。

所以为了方便，我另外安装了一个好用的客户端：https://tortoisegit.org/

Hexo：

```bash
$ npm install -g hexo-cli
```

# 准备项目

## 克隆博客仓库到本地目录

我将博客仓库克隆到 `D:\MyDocs\Hexo`下：

进入`D:\MyDocs\Hexo`目录，右键菜单打开 Git bash窗口，执行如下命令（ssh方式，也可以https方式）

```bash
$ git clone git@github.com:PenleyWang/penleywang.github.io.git
```

## 初始化hexo项目

克隆完成后，本来想执行`hexo init penleywang.github.io`，将项目文件直接生成到`penleywang.github.io`，但提示此目录不为空（因为我们里面有个README.md呢）。

```bash
$ hexo init penleywang.github.io
FATAL D:\MyDocs\Hexo\penleywang.github.io not empty, please run `hexo init` on an empty folder and then copy your files into it
FATAL Something's wrong. Maybe you can find the solution here: http://hexo.io/docs/troubleshooting.html
Error: target not empty
```

只好生成到一个新的目录`blog`了，注意blog不用自己创建，命令会自动创建的。

```bash
$ hexo init blog
INFO  Cloning hexo-starter https://github.com/hexojs/hexo-starter.git
Cloning into 'D:\MyDocs\Hexo\blog'...
remote: Enumerating objects: 22, done.
... ...
added 362 packages from 470 contributors and audited 2622 packages in 33.509s
found 0 vulnerabilities
INFO  Start blogging with Hexo!
```

将`blog`中的全部内容复制到`penleywang.github.io`目录下，作为博客的基础项目。

## 本地书写和测试



# 发布到GitHub Pages

https://hexo.io/docs/github-pages

## 设置访问Token和环境变量

创建access token，用于 **[Travis CI](https://github.com/settings/tokens/347200117)** ：https://github.com/settings/tokens

环境变量设置：https://travis-ci.com/PenleyWang/penleywang.github.io/settings

变量名：**GH_TOKEN**

值：<access token>

## 增加Travis CI配置文件

将本地测试通过的项目，推送到GitHub仓库中，创建CI文件 `.travis.yml` ，用于自动发布。

```yaml
sudo: false
language: node_js
node_js:
  - 10 # use nodejs v10 LTS
cache: npm
branches:
  only:
    - master # build master branch only
script:
  - hexo generate # generate static files
deploy:
  provider: pages
  skip-cleanup: true
  github-token: $GH_TOKEN
  keep-history: true
  on:
    branch: master
  local-dir: public
```



## 禁用GitHub Pages的默认构建功能

https://help.github.com/en/github/working-with-github-pages/about-github-pages

GitHub Pages默认使用 Jekyll，当每次有提交时，会自动构建。但我们使用的Hexo，并未使用 Jekyll ，所以会导致构建出错。我们自己使用Travis来构建，所以，需要禁用GitHub Pages的自动构建功能。

只需要根目录下创建一个 `.nojekyll` 文件即可。