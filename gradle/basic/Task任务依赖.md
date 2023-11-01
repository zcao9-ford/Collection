# 任务依赖
## dependsOn
在两个任务之间可以指明依赖关系。
动态创建4个task,然后指定task0依赖task2，task3
```
4.times { counter ->
    task "task$counter" {
        doLast{
        println "I'm task number $counter"
        }
    }
}
task0.dependsOn task2, task3
```
接着执行task0，他会优先执行task2，task3
```
-> % ./gradlew task0

> Task :app:task2
I'm task number 2

> Task :app:task3
I'm task number 3

> Task :app:task0
I'm task number 0

BUILD SUCCESSFUL in 375ms
3 actionable tasks: 3 executed

```

## finalizedBy
finalizedBy 是指一个 Task 在执行完毕后，会执行另一个 Task。也就是说，被依赖的 Task 后执行，然后才能执行依赖它的 Task
```
task myFirstTask {
    doLast {
        println "This is my first task"
    }
}

task mySecondTask {
    finalizedBy myFirstTask
    doLast {
        println "This is my second task"
    }
}
```
在这个示例中，我们创建了两个 Task，myFirstTask 和 mySecondTask。在 mySecondTask 中使用 finalizedBy 方法来指定它在执行完毕后会执行 myFirstTask。因此，mySecondTask 先执行，然后才能执行 myFirstTask