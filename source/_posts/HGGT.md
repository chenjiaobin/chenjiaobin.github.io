---
title: Hexo+Git+GitHub+Travis部署个人博客
date: 2017-08-29 23:10:00
tags: 
- Hexo
- Git
- Travis
- GitHub
categories:
- 构建工具
- Hexo+Tracvis
---
作为一个高端大气上档次的技术开发者来说，拥有属于自己的博客是非常有必要滴，有了技术博客我们可以干嘛？...... 听我细细道来,技术博客不仅可有装B，又能将自己学到的知识概括性的一字面形式写出来，即强化了自己的知识，当然也方便了他人，一箭双雕，两全其美，何乐而不为。<!--more-->
废话不多说，直接进主题吧！！！！
你现在说看到的这个博客就是我用一下技术来实现的*Hexo+Git+GitHub+Travis*
开始你的博客搭建吧~~~

### 准备工作
1.现在很多东西都是需要用到git和node.js的，所以先在你的电脑上安装git和node.js，这种简单的操作就自己百度脑补了哈！本哈就不介绍了！
2.注册一个GitHub账号
### SSH Key
为什么要配置这个呢？因为你提交代码肯定要拥有你的github权限才可以，但是直接使用用户名和密码太不安全了，所以我们使用ssh key来解决本地和服务器的连接问题
执行
```
cd ~/.ssh    #检查本机已存在的ssh密钥
```
如果提示：No such file or directory 说明你是第一次使用git，执行
```
ssh-keygen -t rsa  #生成公钥和秘钥
```
会要求你填写钥匙的名字，最好还是用它原本的那个`id_rsa`，要不后面可能会出现#### 大问题，本哈就在这里栽了跟头，然后就是输入密码，这个密码是你每次提交代码时需要输入的。生成这两个钥匙后将私钥放到.ssh文件夹里面去#### （切记切记切记），如果没有.ssh这个文件夹先不要急，先继续往下做，后面再把他们放到.ssh里面去，将公钥用文本编辑器打开复制全部内容，进入github，在setting--ssh and gpg key--new ssh key新建一个sshkey,将刚才复制的东西复制进去，名字自己随便取。
### 测试是否连接成功
如果看见Hi xxx,you,ve successfully，那么你成功了（一般执行完这一步前面没有.ssh文件夹的问题算是解决了，现在已经有这个文件夹了，赶紧把钥匙文件复制进去）
```
ssh -T git@github.com
```
如果失败了就执行以下命令进行查看
```
ssh -v git@github.com
```
### 配置github全局账户
```
git config  --global user.name "your account"
git config --global user.email "your email"
```
到这一步配置算是告一段落了，休息一下吧。。。。。

接着就是创建你的博客了，来吧，继续
### Hexo
Hexo是一套基于node.js的静态博客框架。新建一个文件夹Blog,进入文件夹，右键有个`git bash here`（安装完git才有）,点击进入，执行以下命令
```
npm install -g hexo  #全局安装hexo

```
接着初始化这个文件夹Blog，执行
```
hexo init  #初始化

```
好啦，至此，全部安装工作已经完成！blog就是你的博客根目录，所有的操作都在里面进行。。默认有个模板是可以用的，如果不喜欢的话自己下载一个替换就行了。提供个[下载地址](https://hexo.io/themes/)给你吧!克隆出模板后放在theme文件夹里面，然后到根目录的_config.yml里面的theme换成你的主题名称就ok了，具体主题的一些配置要看主题的作者的介绍。
接着就是要生成静态文件了，也就是生成浏览器认识的代码格式html,执行
```
hexo g #生成静态文件
hexo s #启动服务，可以在本地通过localhost:4000来访问
```
如果在浏览器预览不了就看一下端口时候被占用了
### 将博客放到github
修改根目录的_config.yml配置文件
```
deploy:
  type: git
  repository: git@github.com:yourname/yourname.github.io.git
  branch: master
     
```
### 安装提交插件
```
npm install hexo-deployer-git --save
```
执行
```
hexo d
```
好了，在你的github上面已经能看到你的东西了，也直接可以访问了

继续，还有活干呢！！已经快到达终点了哈
### 配置Tracis
其实这个也不用怎么配置那，不要被吓到，用GitHub账号登录，然后点击`sync acconunt`将你GitHub上面的仓库同步过来，然后点击你要部署的仓库，将前面的开关开起来，接着点击项目链接进入页面，在这个页面你会看到`more option`,选中里面的`setting`,将general里面的`Build only ...`开起来，其他默认。接着还有，将下面的`Environment Variables`设置变量，在这里先停一下，先到你根目录创建一个`.travis.yml`文件，这个文件非常重要，文件内容可以参考该[链接](https://troyyang.com/2017/06/24/Travis_Auto_Build_Deploy_Github_Projects/)修改你的账户和邮箱和创库
接着就右键项目，创建新分支，执行
```
git branch source  #名字自己随意,但是这个名字要跟.travis.yml里面的branch的名字一样
```
然后在通过执行以下命令将分支提交到你的仓库
```
git checkout source #切换分支
git add  .
git commit -m '提交说明'
git pull
git push origin source #提交分支
```
接着继续回到travis，刚才那个`name`的值就是.travis.yml里面那个`git push --force --quiet "https://${GitHub_TOKEN}@${GH_REF}" master:master`里面的`GitHub_TOKEN`，后面的值就是你在前面复制到github上面的公钥的内容，复制一遍到这里来，点击`add`，就ok了

不出意外，你的博客已经建好了，是不是很嗨森！！！
如果命不好出现问题那只能自己慢慢调试了，这也能锻炼你调试代码和配置的能力！！
总之，加油吧！！
骚年


