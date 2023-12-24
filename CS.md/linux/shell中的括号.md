GitHub Copilot: 在 Shell 脚本中，括号的使用主要有以下几种情况：

1. `()`：在一个子 shell 中执行命令。例如，`(cd /path/to/somewhere; command)` 会在 `/path/to/somewhere` 目录中执行 `command`，但不会改变当前 shell 的工作目录。

2. `{}`：在当前 shell 中执行命令。例如，`{ cd /path/to/somewhere; command; }` 会在 `/path/to/somewhere` 目录中执行 `command`，并改变当前 shell 的工作目录。

3. `$()`：命令替换。`$(command)` 会执行 `command` 并将其输出替换到当前位置。例如，`echo $(date)` 会输出当前日期和时间。

4. `[]`：用于测试表达式。例如，`[ $a -eq $b ]` 会测试 `$a` 是否等于 `$b`。

5. `[[ ]]`：是 `[ ]` 的增强版，提供了更多的功能，例如字符串比较和模式匹配。

6. `(( ))`：用于算术运算。例如，`((a = b + 5))` 会计算 `b + 5` 的值并赋给 `a`。

7. `{var#pattern}` 和 `{var##pattern}`：用于从变量 `$var` 的开头删除最短/最长的 `$pattern`。例如，如果 `var=abcdef`，那么 `{var#a*}` 的结果是 `bcdef`。

8. `{var%pattern}` 和 `{var%%pattern}`：用于从变量 `$var` 的结尾删除最短/最长的 `$pattern`。例如，如果 `var=abcdef`，那么 `{var%f*}` 的结果是 `abcde`。

9. `{var/pattern/string}`：用于替换第一个匹配的 `$pattern` 为 `$string`。如果 `$pattern` 以 `/` 开头，那么所有匹配的 `$pattern` 都会被替换。

10. `<>`：用于文件描述符的重定向。例如，`command 2>&1` 会将标准错误重定向到标准输出。

以上就是 Shell 脚本中括号的主要用法。