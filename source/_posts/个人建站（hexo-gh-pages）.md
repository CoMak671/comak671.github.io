---
title: 个人建站（hexo+gh-pages）
date: 2021-10-14 17:17:29
tags:
---

之前有很多想法都散落在各种便签中，时间久了便不知何处，趁这两天忙里偷闲，搭建私人网站以便记录日常点滴。

## 选型
因 [WordPress](https://wordpress.org/) 这类动态框架比较重，个人博客这块目前只考虑静态网站生成器hexo

|  名称  | 特点  |
|  ----  |  ----  |
| [hexo](https://hexo.io/zh-cn/)  |  node、简单、轻量  |
| [jekyll](https://jekyllrb.com/)  |  github亲儿子  |
| [hugo](https://www.gohugo.org/)  |  go、快速  |

## 环境配置
hexo安装
```sh
npm install -g hexo-cli
```
初始化项目
```sh
hexo init your-project-name
```

<!-- more -->

## 本地开发
启动
```sh
hexo server
```

## 远程部署
hexo是markdown语法，最终要生成为html/js/css托管在服务器上，所以有手动和自动两种转换方式
### 手动部署
生成hmtl/js/css
```sh
hexo generate
```
发布到服务器
1. 安装git模块
```sh
npm install hexo-deployer-git --save
```
2. 完善_config.yml中deploy相关配置
```yml
deploy:
    type: git
    repo: git@github.com:CoMak671/comak671.github.io.git
    branch: gh-pages #构建产物存放分支，需要与源码分支(main)独立
```
3. 发布
```sh
hexo deploy
```

### 自动部署
因手动发布比较繁琐，可以将源码托管在github上使用travis自动执行该过程
1. 将github与travis账号打通
    * 创建GH_TOKEN：前往 GitHub 新建 [Personal Access Token](https://github.com/settings/tokens)，只勾选 repo 的权限并生成一个新的 Token
    * 回到 Travis CI，前往你的 repository 的设置页面，在 Environment Variables 下新建一个环境变量，Name 为 GH_TOKEN，Value 为刚才你在 GitHub 生成的 Token。确保 DISPLAY VALUE IN BUILD LOG 保持 不被勾选 避免你的 Token 泄漏。
2. 创建travis job，添加.travis.yml配置
```yml
language: node_js
node_js:
  - 14  # 使用 nodejs LTS v14
branches:
  only:
    - main # 只监控 main 的 branch
cache:
  npm
install:
  - yarn
script:
  - hexo clean
  - hexo generate # generate static files
deploy: # 根据个人情况，这里会有所不同
  provider: pages
  skip_cleanup: true # 构建完成后不清除
  token: $GH_TOKEN # travis设置的 token
  keep_history: true # 保存历史
  on:
    branch: main # hexo 站点源文件所在的 branch
  local_dir: public
  target_branch: gh-pages # 存放生成站点文件的 branch
```

## 主题 
[NexT](https://github.com/next-theme/hexo-theme-next)

## 写作
新建文章
```sh
hexo new article-name
```

## 参考
[将 Hexo 部署到 GitHub Pages](https://hexo.io/zh-cn/docs/github-pages)
[GitHub Pages Deployment](https://docs.travis-ci.com/user/deployment/pages/)
[How to use Hexo and deploy to GitHub Pages](https://gist.github.com/btfak/18938572f5df000ebe06fbd1872e4e39)