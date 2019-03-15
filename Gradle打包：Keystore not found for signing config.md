# Gradle打包：Keystore not found for signing config
Mac ，AS 开发，在 Project 根目录下创建keystore.properties(便于隐藏Keystore密码等打包敏感信息) 和 Keystore/keystore.jks 。
刚开始 keystore.properties 配置如下：
```
keyAlias =***
keyPassword =***
storeFile =/Keystore/***.jks
storePassword=***
```
然后 `gradle.build `,接着就报错：
```
> Task :app:validateSigningDevRelease FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':app:validateSigningDevRelease'.
> Keystore file '/Keystore/***.jks' not found for signing config 'config'.
```
确定应该是路径问题找不到 jks 文件。
更改路径如下：
```
keyAlias =***
keyPassword =***
storeFile = Keystore/***.jks
storePassword=***
 ```
 $ gradle build
 ......
 > Task :app:validateSigningDevRelease FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':app:validateSigningDevRelease'.
> Keystore file '/Users/dabo/WS/yourProjectName/app/Keystore/***.jks' not found for signing config 'config'.
```
到此才知道这里配置的路径是相对路径且和调用者 `app/build.gradle ` 同级 ，都在 app 下。
最终配置：
```
keyAlias =***
keyPassword =***
storeFile = ../Keystore/***.jks
storePassword=***
```




