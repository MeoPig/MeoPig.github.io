### 这里是博客的Hexo源文件，克隆此分支可以在任何设备下继续博客编写上传。

---

 首先安装Hexo：`sudo npm install hexo-cli -g`

接着克隆该分支并安装相关依赖：

```
git clone git@………………
cd xxx.github.io
npm install
npm install hexo-deployer-git --save
```

现在就可以通过hexo相关命令进行相关操作：

[hexo文档](https://hexo.io/zh-cn/docs/asset-folders)

```
hexo g   #生成静态资源文件
hexo s   #运行本地服务器预览博客
hexo d   #将当前博客部署，此举会向meopig.github.io的master分支提交代码，同时githubPage会得到更新。
hexo new '文章标题'  #新建文章
```

文章新建与发布流程:

1. 首先通过 ` hexo new '文章标题'` 新建文章，会得到.md文件，在此文件撰写文章。
2. 文章撰写完毕后，通过 `hexo g` 生成静态资源文件。此时可以通过 `hexo s` 预览博客现状，可以看到新文章已在博客中。
3. 运行 hexo d 部署博客，线上博客已更新完毕。

---

#### 每次写完最好都把源文件上传一下，更新源文件，以防止设备损坏情况下，这里有最新的备份。

```
git add .
git commit –m "xxxx"
git push 

```
