# gradle插件
所有有用的功能，例如以能够编译 Java 代码为例，都是通过插件进行添加的。

## 应用二进制插件
什么是二进制插件呢？二进制插件就是实现了org.gradle.api.Plugin接口的插件，他们可以有plugin id，下面我们看下如何应用一个java插件。
```
apply plugin:'java'
```
这样我们把java插件应用到我们的项目中了，其中'java'是Java插件的plugin id，他是唯一的，对于Gradle自带的核心插件都有一个容易记的短名称作为其plugin id，比如这里的java，其实它对应类型的是org.gradle.api.plugins.JavaPlugin，所以通过该类型我们也可以应用这个插件。

又因为包org.gradle.api.plugins是默认导入的，所以我们可以去掉包名直接写为
```
apply plugin: 'org.gradle.api.plugins.JavaPlugin'
apply plugin: 'JavaPlugin'
```

以前三种写法是等价的

## 应用脚本插件
应用脚本插件，其实就是把这个脚本加载进来，和二进制 插件不同的是它使用的是from关键字，后面紧跟的是一个脚本文件，可以是本地的，也可以是网络的，如果是网络上的话要使用HTTP URL。

虽然它不是一个真正的插件，但是不能忽视它的作用，它是脚本文件模块化的基础，我们可以把庞大的脚本文件，进行分块、分段整理，拆分成一个个共用、职责分明的文件，然后使用apply from来引用他们，比如我们可以把常用的函数放在一个utils.gradle脚本里，供其他脚本文件引用。

我们把App的版本名称和版本号单独放在一个脚本文件里，这样我们每次只需要在这个文件修改App的版本名称和版本号即可，清晰、简单、方便、快捷，也可以使用自动化对该文件自动处理生成版本等等。

## 应用第三方插件
第三方发布的作为jar的二进制插件，我们在应用的时候，必须要现在buildscript{}里配置其classpath才能使用，这个不像Gradle为我们提供的内置插件。比如我们的Android Gradle插件，就属于Android发布的第三方插件

```
apply plugin: 'com.android.application'
```

## 插件作用
插件为我们的gradle添加了新的任务，配置了一些构建需要的属性。比如，Java 插件已经向项目添加了 compileJava 任务和 processResources 任务，并且配置了这两个任务的 destinationDir 属性
```
println relativePath(compileJava.destinationDir)
println relativePath(processResources.destinationDir)
```