# gradle 固定资源id

此文档基于`com.android.tools.build:gradle:3.1.2`，其他版本可能不适用

## 资源固定方式
- `<public />` 指定单个id
- `<public-padding />` 指定批量id

## PPTTNNNN
aapt 编译生成资源时，自动生成id的格式是PPTTNNNN，对于同一种类型的资源id 而言，生成id 的PPTT 段要相同，这个是前提。

基于这个前提，由于public 事先制定了资源的id，也即指定了某类资源的PPTT段。因此aapt 再生成id 时为了保持同一种类型资源id 的格式一致性，会以public 中预先指定的资源id 的PPTT 为标杆生成资源id。

也即可以通过public 来指定某类型的资源id 以什么PPTT 开头，达到资源id 分组效果。

***注意：通过public指定PPTT时，attr的TT一定需要是所有类型中最小的，其他几种资源类型顺序可以随意。***

## 例子

public.xml

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <public type="color" name="colorAccent" id="0x7f3a0001" />
    <public type="color" name="colorPrimary" id="0x7f3a0002" />
    <public type="color" name="colorPrimaryDark" id="0x7f3a0003" />
</resources>
```

public.gradle

```gradle
# 将public.xml输出到 build/outputs/generated_exported_all_resouces.xml
android.aaptOptions.additionalParameters("-P", project.buildDir.absolutePath + "/outputs/generated_exported_all_resouces.xml")

afterEvaluate {

    if (project.plugins.hasPlugin('com.android.application')) {

        def android = project.extensions.getByName('android')
        for (variant in android.applicationVariants) {

            def mergeTask = variant.getMergeResources()

            mergeTask.doLast {
                def destPath = mergeTask.outputDir.absolutePath + '/values/'

                println "destPath: $destPath"

                def parentDir = buildscript.sourceFile.parentFile
                def publicXmlPath = parentDir.absolutePath + '/public.xml'
                println '编译宿主资源时插入' + publicXmlPath + ' 到 ' + destPath + ', 用来控制宿主资源id分组'

                copy {
                    from(parentDir) {
                        include 'public.xml'
                    }
                    into(destPath)

                }
            }
        }
    }
}
```

为了保持主app和插件app的资源id相同，需要将`public.gradle` 引入到 module 的 `build.grale` 中。
如

app `build.gradle`

```gradle
apply plugin: 'com.android.application'

# 引入 public.gradle
apply from: '../public.gradle'

android {
    compileSdkVersion 28

    defaultConfig {
        ...
    }

    buildTypes {
        ...
    }
}

```

## 参考

- [Android-Plugin-Framework] (https://github.com/limpoxe/Android-Plugin-Framework/blob/master/FairyPlugin/host.gradle)