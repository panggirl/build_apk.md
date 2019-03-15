# 配置 buildConfigField 

`buildConfigField "boolean", "LOG_DEBUG", "true"`
这个自定义的一个布尔值变量，作用？
比如是程序中我可以通过`BuildConfig.LOG_DEBUG`拿到这个变量，然后通过这个来书写线上环境的地址和测试环境的地址加以区分
可以通过变量值来分析是否要输出DEbug日志，日志true的时候就打印日志.

```
buildTypes {
        debug {
            // 显示Log
            buildConfigField "boolean", "LOG_DEBUG", "true"
            // 测试 base_url
            buildConfigField "String", "BASE_URL", "\"http://debug.com:8080\""
    
        }
        release {
            // 不显示Log
            buildConfigField "boolean", "LOG_DEBUG", "false"
            // 线上 base_url
            buildConfigField "String", "BASE_URL", "\"http://relase.com:8080\""
            
        }
    }
```

