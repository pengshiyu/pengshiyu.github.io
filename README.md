Hexo 是一个快速、简洁且高效的博客框架。
Hexo 使用 Markdown（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。

环境要求：
```
Node.js >= 8.10  nvm安装
Git ： brew install git
```

安装hexo
```
$ npm install -g hexo-cli
$ hexo -v
hexo: 4.2.0
```

```bash
# 新建网站
$ hexo init [folder]

# 启动服务 
$ hexo s 
```
参考
https://www.cnblogs.com/liuxianan/p/build-blog-website-by-hexo-github.html

部署到Github
```
cnpm install hexo-deployer-git -D
```

常用指令
```
hexo new "postName" #新建文章
hexo new page "pageName" #新建页面
hexo generate #生成静态页面至public目录
hexo server #开启预览访问端口（默认端口4000，'ctrl + c'关闭server）
hexo deploy #部署到GitHub
hexo help  # 查看帮助
hexo version  #查看Hexo的版本
hexo clean  # 清除缓存文件

hexo n == hexo new
hexo g == hexo generate
hexo s == hexo server
hexo d == hexo deploy

hexo s -g #生成并本地预览
hexo d -g #生成并上传
hexo g -d // 相当于 hexo g + hexo d
```

主题

https://github.com/iissnan/hexo-theme-next


http://theme-next.iissnan.com/getting-started.html


https://www.jianshu.com/p/5d5931289c09


搜索

https://www.jianshu.com/p/d388119a90ec