# 生成签名及手动打包 apk

打包 就是生成 apk 文件，有了apk文件 Android 系统手机就可以安装使用。打包分 debug 和 release 版本，应用市场的是 release 版本 

### 生成 key 文件即 *.jks 文件
Build -> Generate Signed Bundle/APK --> Create New .. --> key store path、password私钥文件密码，Key：Alias(别名)、password别名密码、
Validity(years)有效期、First and Last name 开发者姓名、Organizational Unit开发者的组织单位、Organization 开发者的组织、
City orLocation开发者所在的城市、State or Province 开发者所在的州或省、Country Code(XX)开发者所在国家 
```
■ key store path: 存放密钥文件的路径
■ Password：密钥文件密码 ■ Confirm：重复密码，确认密码。
■ Key 
          ■ Alias：别名。就是你密钥的另一个名称。
          ■ Password：密码。可以和上面填的相同，一定要选能记住的，忘了的话，以后程序升不了级。
          ■ Confirm：重复密码。
          ■ Validity(years)：有效期。这是密钥的有效期，一般要求25年以上，这里就填个50年吧。
          ■ -------------以上四项都是必填的，下面则是选填-------------------
          ■ First and Last Name：姓名。
          ■ Organizational Unit：组织部门。
          ■ Organization：组织。
          ■ City or Locality：城市或地区。
          ■ State or Province：州或者省。
          ■ Country Code(XX)：国家代码。中国就是CN，基本上就是国家域名后缀。
```
### 根据 *.jks 文件 build apk
Build -> Generate Signed Bundle/APK --> Choose existing... 、key store password 、key alias、key password -->
Destination Folder、Build variants(debug or release)、Signature Versions(V1 V2)

### 打包签名的时候可以选择两种签名方式 V1 、V2

Android 7.0中引入了APK Signature Scheme v2，v1是jar Signature来自JDK
V1：应该是通过ZIP条目进行验证，这样APK 签署后可进行许多修改 - 可以移动甚至重新压缩文件。
V2：验证压缩文件的所有字节，而不是单个 ZIP 条目，因此，在签名后无法再更改(包括 zipalign)。正因如此，现在在编译过程中，我们将压缩、调整和签署
合并成一步完成。好处显而易见，更安全而且新的签名可缩短在设备上进行验证的时间（不需要费时地解压缩然后验证），从而加快应用安装速度。

`v1和v2的签名使用`
```
1）只勾选v1签名并不会影响什么，但是在7.0上不会使用更安全的验证方式
2）只勾选V2签名7.0以下会直接安装完显示未安装，7.0以上则使用了V2的方式验证
3）同时勾选V1和V2则所有机型都没问题
```
还可以在app的build.gradle的android标签下加入如下:
```
signingConfigs {  
    debug {  
        v1SigningEnabled true  
        v2SigningEnabled true  
    }  
    release {  
        v1SigningEnabled true  
        v2SigningEnabled true  
    }  
}  
```

如何验证apk应用的什么签名方式呢？
使用adb shell dumpsys package xxx（这里的xxx是指你的apk包名）的命令查看apkSigningVersion的值为多少。




