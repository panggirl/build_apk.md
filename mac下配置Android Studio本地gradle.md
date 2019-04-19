# mac os x下配置Android Studio本地gradle

Android studio的项目，打开项目的gradle.wrapper包里面的gradle-wrapper.properties文件，会看到如下内容 
```
distributionBase=GRADLE_USER_HOME 
distributionPath=wrapper/dists 
zipStoreBase=GRADLE_USER_HOME 
zipStorePath=wrapper/dists 
distributionUrl=https://services.gradle.org/distributions/gradle-3.3-all.zip 
```
其中distributionUrl这里的路径是默认的gradle数据源。默认是一个网络链接，即到官网下载的，第一次下载完后，gradle会被缓存到一个本地目录中。之后不用再去下载。 
但是如果提示无法连接这个数据源那就不行了。

解决方法：把gradle下载到本地，然后将distributionUrl配置为本地路径。例如我下载的gradle压缩包放置位置如下 
distributionUrl=file:/Users/guangrong/AndroidStudioProjects/gradle/gradle-3.3-all.zip，这样就可以马上去读到gradle包数据然后构建项目。

没必要动其他的设置，其他设置都可以使用默认的。
