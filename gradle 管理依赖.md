# gradle 管理依赖

使用Android Studio开发Android应用时，避免不了需要借助Gradle引入各式各样的第三方库文件，帮助我们更好的开发App，
常见的引入方式有：Jar文件，so文件，Library库文件，aar文件，远程jcenter、maven仓库文件

### Jar 
将jar文件复制至app module目录下的libs文件夹下，然后打开app module目录下的build.gradle配置文件，在dependencies项中添加配置命令，
这里有两种配置方式可供选择：

* 一次性引入libs目录下所有jar文件
```
implementation fileTree(include: ['*.jar'], dir: 'libs')
```
* 单个逐一引入jar文件
```
implementation files('libs/universal-image-loader-1.8.6-with-sources.jar')
```
### So 
新版Gradle实现了自动打包编译so文件的功能，并且为so文件指定了默认的目录app/src/main/jniLibs，当然默认是没有这个文件夹的，
我们只需要新建一个jniLibs文件夹，并将so文件复制到该文件夹下，编译运行即可。

通常，为了更好地管理第三方库文件，或者更简单地将Eclipse项目转化为Android Studio项目，建议将jar文件和so文件放在一起，
统一搁置在app/libs目录下，此时，我们只需要在build.gradle的android一栏中添加如命令，指定so文件的目录即可：
```
    sourceSets {
        main {
                jniLibs.srcDirs = ['libs']
        }
    }
```
### Library库文件
将第三方Library库文件复制到项目根目录下，打开项目根目录下的`settings.gradle`文件，添加配置命令，如：
```
include ':app', ':aliyunface_module'
```
然后打开app module目录下的 `build.gradle`，添加配置命令，如：
```
implementation project(':aliyunface_module')
```
小技巧：推荐在项目根目录下新建一个文件夹，如extras文件夹，将所有Library库文件都复制到该文件下，方便统一浏览管理，这样上面两步对应的配置命令将变成：
```
include ':app', ':extras:aliyunface_module'
....
implementation project(':extras:aliyunface_module')
```

### aar文件
aar其实也是一个压缩文件，相比jar文件，它能够含带res资源文件等，aar文件的引入方式有两种：

* Module -> Import .JAR/.AAR Package 形式引入
选择File菜单，或者打开Project Structure界面，添加新的Module（New Module…），选择Import .JAR/.AAR Package，选择目标aar文件导入。
导入之后，在项目根目录下会自动生成一个同aar名称一样的新的文件夹放置aar文件及其配置文件，

然后打开app module目录下的build.gradle配置文件，在dependencies依赖项中添加配置即可：
```
implementation project(':oss-android-sdk-2.9.2')
```
`注意：这种引入方式无法查看aar文件中的代码和资源等文件`

* Module -> libs目录中引入形式引入
将aar文件复制到如 app module或 aliyunface_module 目录下的libs文件夹中，然后打开module目录下的build.gradle配置文件，在android一栏中添加依赖

```
repositories {
    flatDir {
        dirs 'libs'
    }
}
```

然后再在dependencies一栏中添加：
```
// implementation：该依赖方式所依赖的库不会传递，只会在当前module中生效。
// api：该依赖方式会传递所依赖的库，当其他module依赖了该module时，可以使用该module下使用api依赖的库。
api (name:'oss-android-sdk-2.9.2',ext:'aar')

```

--------------------
关于其他 other_module 引用 aliyunface_module 时也需要用到 aar 时 就会发现找不到对应的 oss-android-sdk-2.9.2 库，
因为在 aliyunface_module中指定的 libs 目录是相对的，在 other_module 中的 build.gradle 只会从自己所在目录下面的 libs 中查找，
结果是找不到相关的 aar 库，所以就会编译出错。
解决方法--在 other_module 中的 build.gradle 文件中的添加如下 ：
```
android {
    repositories {
        flatDir {
            dirs 'libs', project(':aliyunface_module').file('libs')
        }
    }
}
 
```

然后别忘了打开 other_module 目录下的 build.gradle 配置文件， 在 dependencies 依赖项中添加配置：
```
implementation project(':aliyunface_module')
```
### jcenter、maven仓库文件
这个主要在项目根目录的build.gradle文件中添加，具体参考项目根目录的build.gradle文件




