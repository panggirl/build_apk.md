# 解决方案」Gradle’s dependency cache may be corrupt (this sometimes occurs after a network connection timeout.)

是由于网络不稳定导致了下载中断，下载的Gradle版本包不完整而导致了无法解压文件。这时如果点击“Re-download dependencies and
sync project (requires network)”进行重新尝试的话也并不能解决问题，
原因应该是在路径下面已经存在了下载的Gradle包而运行的时候却无法解压导致的，需要手动的将不完整的Gradle包删除后再重新的尝试。
示例：/Users/dabo/.gradle/wrapper/dists/gradle-5.1.1-all/97z1ksx6lirer3kbvdnh7jtjg 下删除里面的问题并点击
Android Studio错误日志栏里面的“Re-download dependencies and sync project (requires network)”后，
Android Studio将会重新下载这个版本的Gradle包。但是，由于网络原因可能还是会失败。

这个时候有两种方式解决这个问题
* 1.将项目中gradle目录下的子目录wrapper中的gradle-wrapper.properties文件中`distributionUrl=https\`的HTTPS 更改为HTTP，然后同步一下gradle就可以了。

* 2.和方法一类似，将失败的链接从“https://services.gradle.org/distributions/gradle-5.1.1-all.zip”改为
“http://services.gradle.org/distributions/gradle-5.1.1-all.zip”后到浏览器地址栏里粘贴上去将这个文件下载下来。
下载完成之后将“gradle-5.1.1-all.zip”文件复制到“/Users/dabo/.gradle/wrapper/dists/gradle-5.1.1-all/97z1ksx6lirer3kbvdnh7jtjg”
（.gradle文件夹的路径根据个人电脑而定），并对zip压缩包进行解压，随后复制“gradle-5.1.1-all.zip.lck”文件（创建一个空白的新文件也可以）
并将文件命名为“gradle-5.1.1-all.zip.ok”。这时候就大功告成了，重新点击“Re-download dependencies and sync project (requires network)”就能够正常运行了。
