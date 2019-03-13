# multiDex 坑
超过65k后使用了google的解决方法：
第一步：
在 build.gradle中
```
defaultConfig {

/**添加多 dex分包支持*/
multiDexEnabled true
}

dependencies {

compile 'com.android.support:multidex:1.0.1'
}
```
第二步：
在AndroidManifest.xml 中 application 的 name 需要指定为`android.support.multidex.MultiDexApplication`
如果你的工程中已经含有Application类,那么让它继承 `android.support.multidex.MultiDexApplication` 类
如果你的Application已经继承了其他类并且不想做改动，那么还有另外一种使用方式,覆写attachBaseContext()方法:
```
@Override
protected void attachBaseContext(android.content.Context base) {
super.attachBaseContext(base);
android.support.multidex.MultiDex.install(this);
}
```
此时重新编译打包后发现果然打包出多个dex文件，在安卓6.0上测试完美运行，但是坑来了 ：在4.4系统上一运行就奔溃！
log显示无法找到application类：
``
java.lang.RuntimeException: Unable to instantiate application com.xx.xx java.lang.ClassNotFoundException: Didn't find class "com.xx.xx" on path: DexPathList[ xxxx]
```
后来知道在5.0系统上使用art 支持多dex，而低版本dalvik默认先加载主dex，如果启动时需要的类不在主dex内就会报错ClassNotFoundException。
查阅后得知：对于dex 的--multi-dex 选项设置与预编译的library工程有冲突,如果你的应用中包含引用的lirary工程,需要将预编译设置为false:
在 build.gradle中添加
dexOptions{
        javaMaxHeapSize "4g" //specify the heap size for the dex process
        preDexLibraries = false //delete the already predexed libraries
}

再次编译打包后，测试在4.4系统上完美运行！

