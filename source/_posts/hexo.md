---
title: hexo
date: 2020-07-08 18:05:54
tags:
---

### hexo的简单使用

#### 必备环境的安装

> github,nodejs,hexo

1. github，node就直接跳过了
2. 在安装过node之后就可以npm了,安装hexo
```
npm install hexo 
```
3. 新建目录，创建一个hexo的项目
```
hexo init
```
4. 在当前目录下载hexo所需的包
```
npm install
```

<!-- more -->

#### hexo简单使用

1. 上来先换主题，一般github上很多，这里使用next,下载到themes目录下
```
git clone https://github.com/theme-next/hexo-theme-next.git themes/next
```
2. 修改主题，顺道看看主要的配置项
```
# Hexo Configuration
## Docs: https://hexo.io/docs/configuration.html
## Source: https://github.com/hexojs/hexo/

# Site 设置默认的标题，描述，作者，语言等
title: ZCF的博客
subtitle:
description:
keywords:
author: ZCF
language: zh-Hans
timezone:

# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://yoursite.com
root: /
permalink: :year/:month/:day/:title/
permalink_defaults:

# Directory 主要的目录配置，一半不用改
#源文件存放地
source_dir: source
#编译过后的前端内容
public_dir: public
#标签存放
tag_dir: tags
archive_dir: archives
category_dir: categories
code_dir: downloads/code
i18n_dir: :lang
skip_render:

# Writing
new_post_name: :title.md # File name of new posts
default_layout: post
titlecase: false # Transform title into titlecase
external_link: true # Open external links in new tab
filename_case: 0
render_drafts: false
post_asset_folder: false
relative_link: false
future: true
highlight:
  enable: true
  line_number: true
  auto_detect: false
  tab_replace:
  
# Home page setting
# path: Root path for your blogs index page. (default = '')
# per_page: Posts displayed per page. (0 = disable pagination)
# order_by: Posts order. (Order by date descending by default)
index_generator:
  path: ''
  per_page: 10
  order_by: -date
  
# Category & Tag
default_category: uncategorized
category_map:
tag_map:

# Date / Time format
## Hexo uses Moment.js to parse and display date
## You can customize the date format as defined in
## http://momentjs.com/docs/#/displaying/format/
date_format: YYYY-MM-DD
time_format: HH:mm:ss

# Pagination
## Set per_page to 0 to disable pagination
per_page: 10
pagination_dir: page

# Extensions
## Plugins: https://hexo.io/plugins/
## Themes: https://hexo.io/themes/
# 设置主题
theme: next

# Deployment
## Docs: https://hexo.io/docs/deployment.html
# 可以设置git发布，需要安装插件，暂不使用
deploy:
  type:

```


3. 新建一个文章试一试
```
#new 一个文章，默认是post模式，存放在source/_post目录下
#--path about/me,修改文件存储路径
#当文章名成有空格，必须用双引号
hexo new "test hexo“
```

4. 在source/_post发现自动创建的test-hexo.md文件，编辑内容
```
---
title: test/hehe
date: 2019-09-27 14:37:55
tags:
---

### TEST

```
5. 生成静态文件（指令缩写附下行）

``` 
hexo generate
hexo g
```


6. 启动服务（指令缩写附下行），默认启动启动端口4000，可浏览查看

```
hexo server
hexo s
```
