# The Shell

bash/the born again shell

**获取日期**

```
$ date
```

有参数的程序

echo：像print的打印功能

多个单词可以用引号（双引号或单引号）

```
$ echo hello
$ echo "hello world"
```

shell通过不变的环境变量定位应用程序实现对echo的识别

echo的程序地址

```
$ echo $PATH
```

```
which echo
```

**地址**：macos linux 正斜杠 /usr/bin/echo（最开始的斜杠：文件系统顶部）有完整的命名空间

windows 反斜杠 D:\Users，所有绝对路径都有分区

绝对路径：完整的路径

相对路径:相对当前位置的路径

**pwd显示当前目录**

```
pwd
```

**使用cd来进入或切换路径**

```
cd /home
```

”.“表示当前目录

”..“表示是上级目录

```
cd ./home
```

```
📁 Linux 路径总结：

	cd ~
    ~     --> 当前用户家目录（如 /home/zhangsan）
    cd -
    -     --> 上一个目录（cd -）
    /     --> 根目录（所有绝对路径的起点）

✅ 绝对路径：从 / 开头，比如 /home/user
❗ 相对路径：不加 /，比如 ../bin、./script.sh

🔧 命令执行：
  1. 直接敲 echo -> 会去 PATH 目录下找 echo
  2. 给出路径 ./echo -> 就用你提供的那个文件来执行
```

rename

```
mv task1 test1
```

copy 可以创造一个新的文件

```
cp file1 ../file2
```

删除文件

```
rm file
```

删除目录（空目录）

```
rmdir
```

创建目录

```
mkdir “My photos”
mkdir My\ photos
```

获取ls程序说明

```
man ls
```

和ls --help差不多

> [!NOTE]
>
> Ctrl+L清除回到顶部

输入流input stream（键盘）

输出流 output stream

```
<file> file
```

```
echo hello > hello.txt
cat hello.txt
cat < hello.txt
```

![image-20250424223120397](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250424223120397.png)

```
cat < hello.txt > hello2.txt
cat < hello.txt >> hello2.txt
```

![image-20250424223448157](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250424223448157.png)

```
$    output|input
```

管道符 `|` 会把**前一个命令的输出，交给后一个命令当输入**。

```
ls -l / |tail -n1 > ls.txt
```

sudo——super user

![image-20250424231446857](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250424231446857.png)

sys里是内核参数



![image-20250424232650467](C:\Users\Anna\AppData\Roaming\Typora\typora-user-images\image-20250424232650467.png)



打开一个文件

```
xdg-open ls.txt
```

## Exercises

All classes in this course are accompanied by a series of exercises. Some give you a specific task to do, while others are open-ended, like “try using X and Y programs”. We highly encourage you to try them out.

We have not written solutions for the exercises. If you are stuck on anything in particular, feel free to send us an email describing what you’ve tried so far, and we will try to help you out.

1. For this course, you need to be using a Unix shell like Bash or ZSH. If you are on Linux or macOS, you don’t have to do anything special. If you are on Windows, you need to make sure you are not running cmd.exe or PowerShell; you can use [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/) or a Linux virtual machine to use Unix-style command-line tools. To make sure you’re running an appropriate shell, you can try the command `echo $SHELL`. If it says something like `/bin/bash` or `/usr/bin/zsh`, that means you’re running the right program.

2. Create a new directory called `missing` under `/tmp`.

   ```
   cd /
   ls
   cd /tmp
   mkdir missing
   ```

3. Look up the `touch` program. The `man` program is your friend.

   ```
   man touch
   ```

4. Use `touch` to create a new file called `semester` in `missing`.

   ```
   touch semester.txt
   ```

5. Write the following into that file, one line at a time:

   ```
   #!/bin/sh
   curl --head --silent https://missing.csail.mit.edu
   ```

   The first line might be tricky to get working. It’s helpful to know that `#` starts a comment in Bash, and `!` has a special meaning even within double-quoted (`"`) strings. Bash treats single-quoted strings (`'`) differently: they will do the trick in this case. See the Bash [quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html) manual page for more information.

6. Try to execute the file, i.e. type the path to the script (`./semester`) into your shell and press enter. Understand why it doesn’t work by consulting the output of `ls` (hint: look at the permission bits of the file).

7. Run the command by explicitly starting the `sh` interpreter, and giving it the file `semester` as the first argument, i.e. `sh semester`. Why does this work, while `./semester` didn’t?

8. Look up the `chmod` program (e.g. use `man chmod`).

9. Use `chmod` to make it possible to run the command `./semester` rather than having to type `sh semester`. How does your shell know that the file is supposed to be interpreted using `sh`? See this page on the [shebang](https://en.wikipedia.org/wiki/Shebang_(Unix)) line for more information.

10. Use `|` and `>` to write the “last modified” date output by `semester` into a file called `last-modified.txt` in your home directory.

11. Write a command that reads out your laptop battery’s power level or your desktop machine’s CPU temperature from `/sys`. Note: if you’re a macOS user, your OS doesn’t have sysfs, so you can skip this exercise.