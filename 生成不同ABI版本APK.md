# 生成不同ABI版本APK在build.gradle中的配置

android {
    ... 
    splits {
        abi {
            enable true
            reset()
            include 'x86', 'x86_64', 'armeabi-v7a', 'arm64-v8a' //select ABIs to build APKs for
            universalApk true //generate an additional APK that contains all the ABIs
        }
    }
    // map for the version code
    project.ext.versionCodes = ['armeabi': 1, 'armeabi-v7a': 2, 'arm64-v8a': 3, 'mips': 5, 'mips64': 6, 'x86': 8, 'x86_64': 9]
 
    android.applicationVariants.all { variant ->
        // assign different version code for each output
        variant.outputs.each { output ->
            output.versionCodeOverride =
                    project.ext.versionCodes.get(output.getFilter(com.android.build.OutputFile.ABI), 0) * 1000000 + android.defaultConfig.versionCode
        }
    }
 }

enable： 启用ABI拆分机制
exclude： 默认情况下所有ABI都包括在内，你可以移除一些ABI。
include：指明要包含哪些ABI
reset()：重置ABI列表为只包含一个空字符串（这可以实现，在与include一起使用来可以表示要使用哪一个ABI，而不是要忽略哪一些ABI）
universalApk：指示是否打包一个通用版本（包含所有的ABI）。默认值为 false。
