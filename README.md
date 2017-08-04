### 为什么使用hexo
1.更新方便,使用md编写博客内容之后，用github提交就可以了.

不用再像ghost进入后台，才能去更新博客内容了

2.不需要自己搭建VPS.

3.可以不使用图床,在本地好管理图片
### 1.install git
    
    sudo apt-get intall git

### 2.install node
    
    sudo git clone https://github.com/nodejs/node.git
    cd master_node
    apt-get install gcc g++ make # 少什么安什么
    ./configure
    make
    sudo make install
    # sudo apt-get install nodejs
    # sudo apt-get install npm
### 3.install hexo

    # npm install --update-binary --no-shrinkwrap
    # npm cache clean --force
    # npm install 
    # 之前是使用淘宝镜像的
    # npm install -g cnpm --registry=https://registry.npm.taobao.org
    npm config set unsafe-perm true
    sudo cnpm install hexo-cli -g
    hexo -v
    npm update hexo-cli -g # 升级hexo

### 使用hexo
#### 4.[hexo主题](https://hexo.io/themes/)
4.1.[安装icarus主题](https://github.com/ppoffice/hexo-theme-icarus/wiki)

    mkdir blog
    cd blog
    hexo init
        source：用于存放我们用markdown编写的博文源文件和静态资源
        # Markdown 和 HTML 文件会被解析并放到 public 文件夹
        scaffolds: Hexo的模板是指在新建的markdown文件中默认填充的内容
        themes：用于存放主题文件，每个主题也有自己的主题配置文件_config.yml文件
        _config.yml：站点配置文件，用于配置博客信息，如作者，博客名称等
    npm install  # 安装node的包 node_modules文件夹
    npm install prompt
    npm install jquery
    hexo clean
    git clone https://github.com/ppoffice/hexo-theme-icarus.git themes/icarus
    npm install -S hexo-generator-json-content # 搜索引擎
    
4.2.启用主题
    
    vim _config.yml
        theme: icarus
4.3.更新主题
    
    cd themes/icarus
    git pull

### 5.hexo 部署到github
[githut pages](https://pages.github.com/):
github每个帐号只能有一个仓库来存放个人主页，而且仓库的名字必须是username/username.github.io，这是特殊的命名约定。你可以通过http://username.github.io 来访问你的个人主页
    
    cd blog
    npm install hexo-deployer-git --save
        #如果有此错误Permission denied (publickey).请用对应的用户运行如下命令
        ssh-keygen -t rsa -C '你的邮箱' # 将对应用户的~./ssh/id_rsa.pub的内容 放到github上
    vim _config.yml
        deploy:
          type: git
          repo: https://github.com/leipengkai/leipengkai.github.io.git 
          branch: master
    #hexo d # 相当于git push 到上面的仓库上.
    #https://leipengkai.github.io/
### 6.使用git命令行部署
    
    cd blog
    git clone https://github.com/leipengkai/leipengkai.github.io.git deploy/leipengkai.github.io
    # 发表一个新文章
    hexo new "new-post" #新建文章
    # 绑定独立域名
    cd ./source
    vim CNAME
        www.femnxyz.xyz

    vim update_blog.sh 
        #!/bin/bash
        IFS="|"
        hexo g
        cp -R public/* README.md  deploy/leipengkai.github.io
        cd deploy/leipengkai.github.io
        git add .
        git commit -m $1
        git push -u origin master 
    chmod u+x update_blog.sh
    ./update_blog.sh "first commit"
    https://leipengkai.github.io/ 
    

    # 也可以用
    vim d.sh 
        #!/bin/bash
        hexo g 
        cp README.md public 
        hexo d -m $1
    ./d.sh "new-post,try to add domain"
        

### 绑定独立域名
1.[获取](https://help.github.com/articles/setting-up-an-apex-domain/)github的IP地址

2.在你的域名注册提供商那里配置DNS解析,推荐使用CNAME类型的记录
   CNAME  www.femnxyz.xyz leipengkai.github.io

3.添加CNAME文件

    cd ./source
    vim CNAME
        www.femnxyz.xyz
    cd ../  
 使用git命令行部署的 效果是:当输入https://leipengkai.github.io/ 会转向到www.femnxyz.xyz这个url上，内容是github上的内容.

如果在github-->setting-->sustom domain-->www.femnyy.com时，当输入https://leipengkai.github.io/ 会转向到www.femnyy.com这个网站上,内容是femnyy.com网站的内容.

### [添加disqus评论系统](https://disqus.com)翻墙之后才能看到
    
    # Qisqus – settings – Add Disqus to your site 
    # Website Name:www.femnxyz.xyz
    # create after -->setting -->shortname
    vim _config.yml
        disqus_shortname: www-femnxyz-xyz (you-shortname)
    vim thems/mabao/_config.yml
        comment_provider: disqus
    # 如需取消某个页面的评论，在md文件的front-matter中增加
        comments: false

### 分类

    hexo new page "python3"
    ~/blog/source/categories/index.md
    vim source/categories/index.md
    # front-matter格式如下
        ---
        title: python3
        date: 2017-07-15 14:47:28
        type: categories
        ---
    # 分类显示页面建立完毕

### 测试分类 

    hexo n "time"
    vim  ~/blog/source/_posts/测试分类.md
        ---
        title: 测试分类
        date: 2017-07-15 15:00:46
        tags: python
        categories: python3
        ---
 
    tags: 编程语言

### 熟悉hexo命令
    
    mkdir blog
    hexo init
        source：用于存放我们用markdown编写的博文源文件和静态资源
        # Markdown 和 HTML 文件会被解析并放到 public 文件夹
        scaffolds: Hexo的模板是指在新建的markdown文件中默认填充的内容
        themes：用于存放主题文件，每个主题也有自己的主题配置文件_config.yml文件
        _config.yml：站点配置文件，用于配置博客信息，如作者，博客名称等
        node_modules文件夹
    hexo g(generate) # 生成静态文件，
        会在当前目录下生成一个新的叫做public的文件夹 
    hexo s(server) # 启动本地web服务，用于博客的预览 
    http://localhost:4000/

    # 其它常用命令
    hexo clean #清除缓存 网页正常情况下可以忽略此条命令
    hexo d(deploy) 将public的内容部署播客到远端（比如github, heroku等平台）
    hexo d -g #生成部署 在执行hexo deploy时将其public复制到.deploy_git文件夹中
    hexo s -g #生成预览
    hexo new "new-post" #新建文章
        source/_posts目录下会生成一个”new-post.md”的markdown文件
    hexo new page "pageName" #新建页面
    
    hexo server #Hexo 会监视文件变动并自动更新，您无须重启服务器。
    hexo server -s #静态模式
    hexo server -p 5000 #更改端口
    hexo server -i 192.168.1.1 #自定义 IP
