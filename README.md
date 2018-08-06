使用官方dockerfile改造，添加aliyun mirrors

由于官方版本移除了VOLUME /root/.m2
配置文件放置于/usr/share/maven/ref/
所以以前通过 -v $HOME/.m2:/root/.m2 缓存的方式将不可用

故建议使用 docker volume create maven-repo 的形式创建一个repo本地缓存用以加速构建
加载形式为 -v maven-repo:/usr/share/maven/ref

如果需要在本地共享出maven缓存则建议首先执行 -v $HOME/.m2:/root/.m2 取出容器内置的配置文件,
然后取消运行，修改为 -v $HOME/.m2:/usr/share/maven/ref ,即可执行缓存
或者
在 $HOME/.m2 下自行建立 settings.xml, 写入 <localRepository>${user.home}/.m2/repository</localRepository>
