title: How It Works?
date: 2011-06-30 10:17:16
tags: 
- hexo
- git
- qiniu
categories: soft_use
description: Introduce how to use hexo with theme-freemind.
feature: /images/mainmenu/08.jpg
toc: true
---
### install&config ###
#### hexo ####
{% raw %}
```
gitcafe,node.js.		//参考http://hexo.io/docs/index.html
npm install -g cnpm --registry=http://r.cnpmjs.org
cnpm install 
npm install -g hexo(npm install hexo@2.5.3)//注意版本与插件兼容问题
```
```
npm install -g hexo
npm update hexo -g
hexo init blog && cd blog
hexo new "New Post" / hexo n
hexo generate / hexo g
hexo server / hexo s
hexo deploy / hexo d
hexo help
#修改_config.yml
	new_post_name: :year-:month-:day-:title.md # hexo n 生成类似octopress命名文件
```
{% endraw %}
<!-- more -->
#### theme-freemind ####
{% raw %}
```
npm install hexo-tag-bootstrap --save		//参考https://github.com/wzpan/hexo-theme-freemind/
设置description 小i图标与文字内容换行
theme-freemind/layout/_partial/article.ejs --> markdown(item.description)
加上则有md渲染但是会换行,自行取舍.
PS:说明来自主题作者的邮件.
```
{% endraw %}
### md-write ###
http://hexo.io/docs/writing.html
```
categories:
	knowledge
	skill
	soft_use
	soft_dev
	unuseful
```
### source-backup ###
```
cd source
git init
git add .
git commit -m "source backup"
git remote add origin https://gitcafe.com/channe/channe.git
git push origin master:source		//gitcafe-pages分支默认是静态页面分支,我们把source保存在另一个分支下git
(git push -u origin master)
```
### 外链空间 ###
* install qiniu-qrsync.exe
* bucket-channe 可用于备份静态页面分支,http://channe.qiniudn.com,可访问->qiniu.conf
```
{
    "access_key": "********",
    "secret_key": "********",
    "bucket": "channe",
    "sync_dir": "public",
    "async_ops": "",
    "debug_level": 1
}
```
* 图片,视频,一切想嵌入在页面中的元素可以用另一个bucket存放->res.conf
```
{
    "access_key": "********",
    "secret_key": "********",
    "bucket": "krisirk",
    "sync_dir": "res",
    "async_ops": "",
    "debug_level": 1
}
```

### HEXO DEPLOY ###
有时候网络不好,hexo d 发布命令老出错中断.我们来一发手动档.
```
git config --global user.name 'channe'
git config --global user.email 'bearxuqian@gmail.com'
删除.deploy*文件夹里的.git目录.
git init
git add .
git commit -m 'first commit'
git remote add origin 'git@gitcafe.com:channe/channe.git'
git checkout -b gitcafe-pages
git push origin gitcafe-pages
#git push -u origin master  HEXO DEPLOY命令是把local.master -> origin.gitcafe-pages
```
### 参考 ###
{% textcolor info %}
http://cnpmjs.org/
http://jiabin.tk/2013/06/22/add-rss-and%20sitemap-into-hexo/
{% endtextcolor %}