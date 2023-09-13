# 函数定义
在Shell脚本中，可以使用function关键字来定义函数。以下是一个简单的示例
```
function say_hello() {
    echo "Hello, world!"
}

# 调用函数
say_hello
```
在上面的示例中，定义了一个名为say_hello的函数，函数体中使用echo语句输出字符串Hello, world!。在函数定义后，使用函数名来调用函数，例如say_hello。

如果函数需要传递参数，则可以在函数名后面添加参数列表。以下是一个带参数的函数示例：
```
function say_hello_to() {
    echo "Hello, $1!"
}

# 调用函数
say_hello_to "John"
```
在上面的示例中，定义了一个名为say_hello_to的函数，并在函数名后面添加了参数列表。函数体中使用echo语句输出字符串Hello, $1!，其中$1表示第一个参数的值。在函数调用时，将要传递的参数作为参数列表的值传递给函数，例如say_hello_to "John"。

除了使用function关键字来定义函数之外，还可以使用函数简写语法来定义函数。例如：
```
say_hello() {
    echo "Hello, world!"
}

# 调用函数
say_hello
```

# 函数返回值
在Shell脚本中，可以使用return语句来返回函数的值。以下是一个简单的示例：
```
function add() {
    local sum=$(($1 + $2))
    return $sum
}

# 调用函数并获取返回值
add 10 20
result=$?

echo "The sum is: $result"
```
在上面的示例中，定义了一个名为add的函数，函数体中计算两个参数的和并将结果保存在变量sum中。使用return语句将变量sum的值作为函数返回值。在函数调用时，将要传递的参数作为参数列表的值传递给函数，例如add 10 20。使用$?来获取函数的返回值，将其保存在变量result中，并使用echo语句输出返回值。

注意，在Shell脚本中，函数的返回值只能是一个整数值。如果需要返回其他类型的值（如字符串），可以将其保存在一个变量中，并在函数结束时输出该变量的值。例如：
```
function get_greeting() {
    local name=$1
    local greeting="Hello, $name!"
    echo $greeting
}

# 调用函数并获取返回值
result=$(get_greeting "John")
echo $result
```
在上面的示例中，定义了一个名为get_greeting的函数，函数体中使用参数name构建问候语，并将其保存在变量greeting中。在函数结束时，使用echo语句输出变量greeting的值。在函数调用时，将要传递的参数作为参数列表的值传递给函数，例如get_greeting "John"。将函数的输出保存在变量result中，并使用echo语句输出返回值。
