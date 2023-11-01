# 自定义gradle插件
对于一些复杂的构建功能，我们需要通过自定义gradle插件来扩展
## buildSrc
在 Gradle 中，buildSrc是一个特殊的目录，用于存放构建脚本的代码，它不需要在setting.gradle中include进来，也会被识别。

buildSrc目录下的代码可以被构建脚本直接引用。

buildSrc目录下的代码会在构建脚本之前编译和执行。所以他不会读到gradle.properties中定义的全局变量。

我们在buildSrc/src/main/groovy中创建HelloPlugin.groovy文件
```
import org.gradle.api.Plugin;
import org.gradle.api.Project;
import org.gradle.api.Task;
import org.gradle.api.Action;

public class HelloPlugin implements Plugin<Project> {
    @Override
    public void apply(Project project) {
        println("this is HelloPlugin");
        project.getTasks().create("HelloTask", new Action<Task>() {
            @Override
            public void execute(Task task) {
                println("this is HelloPluginTask");
            }
        });
    }
}
```
如上代码，我们就创建了一个HelloTask。

然后在app/build.gradle中引用该插件`apply plugin: HelloPlugin`,然后我们执行`./gradlew tasks --all`。结果如下:
```
-> % ./gradlew tasks --all
初始化阶段开始...
settingsEvaluated
projectsLoaded

> Configure project :app
子项目beforeEvaluate回调...
this is HelloPlugin
this is HelloPluginTask
...
```

## 第三方插件
仿照buildSrc目录的build.gradle，我们也需要增加groovy-gradle-plugin插件，这样我们就可以对groovy代码进行构建
```
plugins {
    id 'groovy-gradle-plugin'
}

repositories {
    gradlePluginPortal()
}
```
而gradlePluginPortal 是一个在线的 Gradle 插件仓库，它提供了大量的 Gradle 插件供开发者使用。在 Gradle 5.0 及以上版本中，gradlePluginPortal 被默认添加到了 Gradle 的插件仓库列表中。

使用 gradlePluginPortal 可以方便地查找和引用第三方 Gradle 插件。当你在 Gradle 构建文件中声明一个插件时，Gradle 会自动从 gradlePluginPortal 中下载该插件的相关信息和依赖项，并在构建过程中使用该插件

完成插件代码后。还需要指明插件入口。
在src/main/目录下创建resources/META-INF/gradle-plugins/xxxx.properties
其中xxxx将是apply plugin: xxxx的名称。

而properties的具体内容是
```
implementation-class=插件的全类名
```

例如：android gradle 插件
```
com.android.application.properties:

implementation-class=com.android.build.gradle.AppPlugin
```

