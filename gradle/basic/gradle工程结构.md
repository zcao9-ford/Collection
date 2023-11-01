# gradle工程结构

当我们执行`gradle init`命令后,gradle工具初始化项目工程，结构大致如下
```
├── gradle 
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew 
├── gradlew.bat 
├── settings.gradle 
├── build.gradle
├── app
│    ├── src
│    └── build.gradle
├── gradle.properties
```

## 介绍
### gradle wrapper目录
gradle文件中存在着wrapper文件夹，在wrapper下存在以下两个文件(gradle-wrapper.jar,gradle-wrapper.properties) ;gradle-wrapper.properties文件配置如下
```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-7.0.2-bin.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```
通常会定义gradle工具包的下载地址

### gradlew和gradlew.bat
可执行文件。如果没有在系统中配置gradle路径，那么我们可以使用./gradlew来执行任务，他会根据gradle-wrapper.properties中的下载地址，去下载gradle工具，完成下载后再执行任务

### setting.gradle
整个项目的管理，包含哪些模块等。
```
include ':app'

project(':utils').projectDir = file('utils')
```

### gradle.properties
项目的一些配置属性,以key=value的形式存在。还可以自定义一些属性，构建时使用
```
systemProp.http.proxyHost=
systemProp.http.proxyPort=

android.useAndroidX=true

org.gradle.jvmargs=-Xmx1536m
```

### build.gradle
构建脚本。除了工程根目录下有build.gradle外，每个module下也都有一个build.gradle。
