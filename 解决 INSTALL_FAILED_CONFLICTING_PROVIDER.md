# 解决 INSTALL_FAILED_CONFLICTING_PROVIDER 的问题方法
问题如下：
```
FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':app:installXmf119Debug'.
> com.android.builder.testing.api.DeviceException: com.android.ddmlib.InstallException: INSTALL_FAILED_CONFLICTING_PROVIDER: 
Package couldn't be installed in /data/app/com.reliance.reliancesmartfire.debug-wCypbgauyIpU-Mf8WsYTkQ==: Can't install 
because provider name com.reliance.reliancesmartfire.fileprovider (in package com.reliance.reliancesmartfire.debug) is 
lready used by com.reliance.reliancesmartfire

```

问题原因

在Android中authority要求必须是唯一的，比如你在定义一个provider时需要为它指定一个唯一的authority。如果你在安装一个带有provider的应用时，系统会检查当前已安装应用的authority是否和你要安装应用的authority相同，如果相同则会弹出上述警告，并且安装失败。

解决方案

在定义provider是，使用软编码的形式，如下：
```
<provider
  android:name="android.support.v4.content.FileProvider"
  android:authorities="${applicationId}.fileprovider"
  android:grantUriPermissions="true"
  android:exported="false">
  <meta-data
    android:name="android.support.FILE_PROVIDER_PATHS"
    android:resource="@xml/file_paths" />
</provider>
```
上述代码中通过${applicationId}.fileprovider的形式来指定provider的authorities，所以该provider的authorities会根据applicationId的不同而不同，从而避免了authorities的冲突问题。

那么如何使用刚才定义的authorities呢？ 
我们在定义authorities是采用了applicationId+fileprovider的形式，在获取authorities的时候，我们就可以通过包名+fileprovider来获取，代码如下：
```
public final static String getFileProviderName(Context context){
  return context.getPackageName()+".fileprovider";
}
```

 
