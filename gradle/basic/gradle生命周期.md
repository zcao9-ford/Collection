Gradle的生命周期可以分为三个部分：初始化阶段、配置阶段和执行阶段

## 初始化阶段
Initialization初始化阶段
Gradle为每个module创建了一个Project实例，在多module的项目构建过程中，Gradle会找出哪些Project实例需要参与到项目的构建中，本质上也就是执行settings.gradle脚本，从而读取整个项目中有多少个Project实例。

## Configuration
配置阶段的任务是执行各module下的build.gradle脚本，从而完成Project的配置，并且构造Task任务依赖关系图以便在执行阶段按照依赖关系执行Task。

## Execution
在配置阶段结束后，Gradle会根据任务Task的依赖关系会创建一个有向无环图，还可以通过Gradle对象的getTaskGraph方法访问，对应的类是TaskExecutionGraph，然后通过调用./gradlew <任务> 来执行对应的任务

## project的两个监听方法

```
//在 Project 进行配置前调用 
void beforeEvaluate(Closure closure)

//在 Project 配置结束后调用 
void afterEvaluate(Closure closure)
```

## 生命周期流程展示
在setting.gradle中进行如下配置,并在gradle.properties中定义**MYVAR=abcd**
```
rootProject.name = 'root'
include('app', 'list', 'utilities')

println '初始化阶段开始...'
println MYVAR
```
在项目根目录添加
```
// 对子模块进行配置
subprojects { sub ->
    sub.beforeEvaluate { proj ->
        println "子项目beforeEvaluate回调..."
    }
}
```
直接执行`./gradlew projects`
```
-> % ./gradlew projects
初始化阶段开始...
abcd

> Configure project :app
子项目beforeEvaluate回调...

> Configure project :list
子项目beforeEvaluate回调...

> Configure project :utilities
子项目beforeEvaluate回调...

> Task :projects
------------------------------------------------------------
Root project 'root'
------------------------------------------------------------

Root project 'root'
+--- Project ':app'
+--- Project ':list'
\--- Project ':utilities'
```
可以看到，在Initialization阶段，settings.gradle中的打印先输出,并且读取了gradle.properties中的变量。然后是读各个子项目的配置，最后是我们要运行的task。

然后我们在app的build.gradle中配置
```
println "app project配置开始"

afterEvaluate {
    println "app project afterEvaluate回调..."
}

task appTest {
    println "app project appTest任务配置"
    doLast {
        println "执行子项目任务..."
    }
}
```
继续执行./gradlew projects
```
-> % ./gradlew projects
初始化阶段开始...
abcd

> Configure project :app
子项目beforeEvaluate回调...
app project配置开始
app project appTest任务配置
app project afterEvaluate回调...

> Configure project :list
子项目beforeEvaluate回调...

> Configure project :utilities
子项目beforeEvaluate回调...

> Task :projects
```
可以看到即使是appTest这个task的闭包中的配置项也是被调用的。但是doLast并没有执行。只有./gradlew appTest才会有该日志

## 监听gradle生命周期
在settings.gradle中添加如下代码。

```
core-api/org/gradle/api/invocation/Gradle.java
core-api/org/gradle/BuildListener.java
gradle.addBuildListener(new BuildListener(){
    void buildStarted(Gradle gradle){
        println "buildStarted"
    }

    //settings.gradle配置完后调用，只对settings.gradle设置生效，此时未完成 Project 的初始化
    void settingsEvaluated(Settings settings){
        println "settingsEvaluated"
    }

    //初始化阶段结束
    void projectsLoaded(Gradle gradle){
        println "projectsLoaded"
    }

    //所有project配置完成后调用
    void projectsEvaluated(Gradle gradle){
        println "projectsEvaluated"
    }

    void buildFinished(BuildResult result){
        println "buildFinished"
    }
})
```

继续执行./gradlew appTest
```
-> % ./gradlew appTest
初始化阶段开始...
settingsEvaluated
projectsLoaded

> Configure project :app
子项目beforeEvaluate回调...
app project配置开始
app project appTest任务配置
app project afterEvaluate回调...

> Configure project :list
子项目beforeEvaluate回调...

> Configure project :utilities
子项目beforeEvaluate回调...
projectsEvaluated

> Task :app:appTest
执行子项目任务...
buildFinished
```

## 监听task的生命周期
Gradle在配置完成之后，会对所有的task生成一个有向无环图，这里叫做task执行图。也就是TaskExecutionGraph这个类。添加如下代码
```
gradle.taskGraph.whenReady{
    println "task whenReady"
}
```
执行后可以看到他是在projectsEvaluated完成后调用。

对于任务的监听，可以通过TaskExecutionListener接口来实现。这个接口定义了4个方法：
* beforeExecute(Task task)：在任务执行之前调用。
* afterExecute(Task task, TaskState state)：在任务执行之后调用。
* beforeActions(Task task)：在任务执行动作之前调用。
* afterActions(Task task)：在任务执行动作之后调用。

具体可见core-api/org/gradle/api/execution/TaskExecutionListener.java

而taskGraph提供了添加增加监听的方法addTaskExecutionListener
```
gradle.taskGraph.addTaskExecutionListener(new TaskExecutionListener(){
            @Override
            void beforeExecute(Task t) {
                println "Task ${t.name} is about to execute"
            }

            @Override
            void afterExecute(Task t, TaskState state) {
                println "Task ${t.name} has finished executing"
            }
        })
```


