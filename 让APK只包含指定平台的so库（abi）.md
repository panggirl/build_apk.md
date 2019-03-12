# 让APK只包含指定平台的so库（abi）
在使用第三方SDK时会有各种平台的 so 库文件，他们支持的平台目标可能不一样，这样就设计到 abi 兼容问题。
方案
第一种：针对对每个要生成的渠道包分别进行配置（在app下的build.gradle文件中配置）
```
productFlavors {
        xiaomi{
            ndk {
                abiFilters "armeabi"
            }
        }
        huawei{
            ndk {
                abiFilters "armeabi-v7a"
                abiFilters "x86"
                abiFilters "armeabi"
                abiFilters "arm64-v8a"
                abiFilters "x86_64"
            }
        }
        meizu{
            ndk {
                abiFilters "armeabi-v7a"
                abiFilters "armeabi"
                abiFilters "arm64-v8a"
            }
        }
}
```
像这种就是单独对渠道包配置，对应打出来的包就只包含你所需要的so库。

第二种：全局配置（这种需求比较多，同样是在app下的build.gradle下配置）
```
defaultConfig {
        ndk {
            abiFilters "armeabi", "armeabi-v7a", "arm64-v8a"
        }
    }
 ```
这样生成的所有Apk就最多有armeabi, armeabi-v7a, arm64-v8a这三种平台的so库，当然要它本身就有这三个平台的so库


