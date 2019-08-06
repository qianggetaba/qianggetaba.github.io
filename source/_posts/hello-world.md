---
title: Hello World
date: 2019-08-04 09:59:02
---

Hello World is simple, but Make world is better.

建站命令
```
sudo pacman -S nodejs npm
node -v
npm -v
npx -v
```
```
npm install hexo-cli -g
cd proj
hexo init myblog
cd myblog
npm install
hexo server

hexo generate  ;hexo g for short,static pages, upload public to gitpages

hexo n "文章标题"  ;new article, \Hexo\source\_posts , .md
```
```
git init

```

```
hexo clean
hexo g & hexo s


# 使用dev分支保存git pages 源码，master为生成的git pages的静态html文件
hexo clean && hexo g && git checkout master && rm -rf 2019 archives categories css img index.html js page tags && cp -r public/* . && git add . && git commit -m "update" && git push && git checkout dev

```

upload public to gitpage


