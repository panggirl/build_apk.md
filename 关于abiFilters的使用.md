#关于abiFilters的使用
```
android {
    compileSdkVersion 28 
    defaultConfig { 
      ndk {
            abiFilters "armeabi-v7a", "x86" //
        }
    } 
}
```
涉及到so库文件 abi 兼容的选择。因为如果全部都适配的话，包很大，这样兼容那些用户数极少的cpu就很不划算，所以我只适配了armeabi-v7a这一个。
`abiFilters "armeabi-v7a"`:指定ndk需要兼容的架构，把除了v7a以外的兼容包都过滤掉，只剩下一个v7a的文件夹,这样其他依赖包里mips,x86,
armeabi,arm-v8之类的so会被过滤掉.创建了各个兼容平台的目录。具体可以把 pak 包解压查看 lib 目录下存在的目录 
因为只要出现了这个目录如 armeabi-v7a ，系统就只会在这个目录里找.so文件而不会遍历其他的目录，所以就出现了之前找不到.so文件的情况
（因为其他目录没有我的.so文件）。
