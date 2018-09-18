# fanhuirong.github.io


创建流程
1. 创建仓库fanhuirong.github.io；
2. 创建两个分支：master 与 hexo；设置hexo为默认分支（因为我们只需要手动管理这个分支上的Hexo网站文件）；
3. clone 使用git clone git@github.com:fanhuirong/fanhuirong.github.io.git拷贝仓库；
4. 在本地fanhuirong.github.io文件夹下新建空文件夹Blog，Git bash依次执行npm install hexo、hexo init、npm install 和 npm install hexo-deployer-git（此时当前分支应显示为hexo）;
5. 修改_config.yml中的deploy参数，分支应为master；
6. 依次执行git add .、git commit -m “…”、git push origin hexo提交网站相关的文件；
7. 执行hexo g -d生成网站并部署到GitHub上（在Blog文件夹下）。
`
更换电脑
克隆仓库（默认分支为hexo）；
在本地新拷贝的fanhuirong.github.io文件夹下的Blog里通过Git bash依次执行下列指令：npm install hexo、npm install、npm install hexo-deployer-git（记得，不需要hexo init这条指令）。

切换主题
`cd fanhuirong.github.io/blog` 
`git clone https://github.com/iissnan/hexo-theme-next themes/next`
修改配置文件

[nexT主题使用文档 ](http://theme-next.iissnan.com/getting-started.html)
[GitHub Pages + Hexo搭建博客](http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2/#more)

[小白独立搭建博客--Github Pages和Hexo简明教程](https://my.oschina.net/ryaneLee/blog/638440)

[hexo常见问题](http://www.jianshu.com/p/4d2c07a330da)