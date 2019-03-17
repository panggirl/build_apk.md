# productFlavors(定制产品)
`MAC AS 3.3`
如果你正在查阅build.gradle文件的所有可选项，请点击这里进行查阅：[Android Plugin DSL参考](http://google.github.io/android-gradle-dsl/current/)
[gradle documentation](https://docs.gradle.org/current/userguide/java_plugin.html#N12394)

Build Type + Product Flavor = Build Variant
每一个Build Type都会生成一个新的APK。 
Product Flavor同样也会做这些事情：项目的输出将会拼接所有可能的Build Type和Product Flavor（如果有Flavor定义存在的话）的组合。 
例如：

android {
    ....
    productFlavors {
        flavor1 {
            ...
        }
        flavor2 {
            ...
        }
    }
}
配置多渠道 flavor ，需要上面两个flavor与默认的debug和release两个Build Type将会生成4个Build Variant： 
在AS IDE 的Gradle 面板的task -> other里可以看到
- assembleFlavor1Debug 
- assembleFlavor1Release 
- assembleFlavor2Debug 
- assembleFlavor2Release 
属性
1、注意ProductFlavor类型的android.productFlavors.*对象与android.defaultConfig对象的类型是相同的。这意味着它们共享相同的属性。 
defaultConfig为所有的flavor提供基本的配置，每一个flavor都可以重设这些配置的值。

2、通常情况下，`buildTypes 的配置会添加到其它配置的后面`。例如， buildTypes 中的applicationIdSuffix后添加到 Flavor中的applicationIdSuffix 的后面。也可以这样：buildTypes 的 packageNameSuffix 会被追加到 Flavor 的packageName 后面。
3、有一些设置可以同时在buildTypes和 Flavor中设置。在这种情况下，按照个别为主的原则决定。 
例如，signingConfig就这种属性的一个例子。 signingConfig允许通过设置android.buildTypes.release.signingConfig来为所有的release包共享
相同的SigningConfig。也可以通过设置android.productFlavors.*.signingConfig来为每一个release包指定它们自己的SigningConfig。

#### Sourcesets and Dependencies
1、Product Flavor 和 buildTypes 也会通过它们自己的 sourceSet 提供代码和资源, 通过设置不同风味实现不同风味包的资源替换，例如：生产和debug模式）的 logo 不同、某些代码不同，那我们就可以设置源集目录，专门收录不同不同配置下的代码和资源文件。
  首先切换到Project视图，我们的工程默认是只有 main 源集，即 ../src/main，下面添加一个debug类型的源集：
  `右击 New -> Folder ->Java Folder->Target Source Set中选 debug -> finish`，系统自动给我们的工程生成了对应目录。
  同样的可以添加XML文件目录：`右击 New -> XML-> Layout XML File / Value XML File
 debug sourceSet： android.sourceSets.flavor1 位于src/debug/ 
  注：这里的Target Source Set会包含buildTypes、productFlavors 和二者的组合变体
  
上面的例子将会的sourceSet： 
```
android.sourceSets.flavor1 位于src/flavor1/ 
android.sourceSets.flavor2 位于src/flavor2/ 
android.sourceSets.androidTestFlavor1 位于src/androidTestFlavor1/ 
android.sourceSets.androidTestFlavor2 位于src/androidTestFlavor2/
```
2、类似 BuildTypes，Product Flavor也可以有它们自己的依赖关系。例如，如果使用flavor来生成一个基于广告的应用版本和一个付费的应用版本，
其中广告版本可能需要依赖于一个广告SDK，但是另一个不需要。
dependencies {
    flavor1Implementation "..."
}
在这个例子中，src/flavor1/AndroidManifest.xml文件中可能需要声明访问网络的权限。

3、Flavor的sourceSet拥有比Build Type的sourceSet更高的优先级！！！
------------------
如创建 flavor1 文件夹，级别跟main同级，在渠道文件夹下有两种类型的文件会被用到，一种是资源文件，一种是java代码文件，资源文件跟在main中的资源文件使用的方式方法是一样的，有不同的资源文件时，只要命名跟main文件中的资源命名是样的，就会自动覆盖main中的资源文件，不过java文件夹下面的java文件不太一样，不会自动覆盖main中的java文件，所以使用的时候，如果是渠道独有的java文件的话，在main中就不要存在该java文件就行，否则会报文件重复的错误

### flavor dimension及上面所述可以参考入下
[Android Studio 和Gradle Plugin 3.0 迁移不完全指南](https://zhuanlan.zhihu.com/p/30722989)
[Android Gradle 使用教程](https://www.jianshu.com/p/e83baf81c57a)


