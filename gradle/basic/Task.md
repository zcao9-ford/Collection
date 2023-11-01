# Task
每个 project 都由多个 tasks 组成。每个 task 都代表了构建执行过程中的一个原子性操作。如编译，打包，生成 javadoc，发布到某个仓库等操作。

## 查看默认的tasks
```
./gradlew tasks --all
```
可以查看所有的tasks
```
Build tasks
-----------
app:assemble - Assembles the outputs of this project.
app:build - Assembles and tests this project.
app:buildDependents - Assembles and tests this project and all projects that depend on it.
app:buildNeeded - Assembles and tests this project and all projects it depends on.
app:classes - Assembles main classes.
app:clean - Deletes the build directory.
app:jar - Assembles a jar archive containing the main classes.
app:testClasses - Assembles test classes.
...
Verification tasks
------------------
app:check - Runs all checks.
app:test - Runs the unit tests.
...
...
```
可以发现其他比较有用的task
```
app:properties - Displays the properties of project ':app'.
app:dependencies - Displays all dependencies declared in project ':app'
```

## 自定义一个task
```
task hello {
    println System.getenv("PATH")
    doLast {
        println 'Hello world!'
    }
}
```
然后执行`./gradlew [Task名]`就可以调用该task。
上面的示例中，doLast,doFirst指的是在执行阶段，而不是Configure阶段。

```
> Configure project :app
/usr/bin:/bin:/usr/sbin:/sbin:/usr/local/go/bin:/Library/Apple/usr/bin

> Task :app:printTaskProperties
Hello world!
```

## groovy代码
task中可以采用groovy代码
```
task outputDescription{
    new File("description.txt").withWriter('utf-8'){
        writer -> writer.writeLine "line"
    }
}

task count{
    4.times { print "$it " }
}
```
输出一个文件description.txt，同时终端输出
```
-> % ./gradlew outputDescription

> Configure project :app
0 1 2 3
BUILD SUCCESSFUL in 487ms
```

## 为task自定义属性
你可以为一个任务添加额外的属性。例如,新增一个叫做 myProperty 的属性，用 ext.myProperty 的方式给他一个初始值。这样便增加了一个自定义属性
```
task myTask {
    ext.myProperty = "myValue"
}

task printTaskProperties {
    doFirst{
        println myTask.myProperty
    }
}
```