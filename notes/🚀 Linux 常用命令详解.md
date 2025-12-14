### 🚀 **Linux 常用命令详解**

------

### ✅ **1️⃣ 文件和目录操作**

| 命令    | 功能描述                     | 示例                         |
| ------- | ---------------------------- | ---------------------------- |
| `ls`    | 查看当前目录下的文件和文件夹 | `ls -l` 查看详细信息         |
| `pwd`   | 显示当前工作目录             | `pwd`                        |
| `cd`    | 切换目录                     | `cd /home/user`              |
| `mkdir` | 创建新目录                   | `mkdir myFolder`             |
| `rmdir` | 删除空目录                   | `rmdir emptyFolder`          |
| `rm`    | 删除文件或目录               | `rm -rf folder` 强制删除目录 |
| `cp`    | 复制文件或目录               | `cp file1 file2`             |
| `mv`    | 移动或重命名文件/目录        | `mv oldname newname`         |
| `find`  | 搜索文件                     | `find / -name file.txt`      |
| `tree`  | 以树状格式显示目录结构       | `tree /home`                 |

✅ **常用参数**

- `ls -a` → 显示隐藏文件
- `ls -lh` → 以人类可读格式显示大小
- `cp -r` → 递归复制整个目录
- `rm -rf` → 强制递归删除目录

------

### ✅ **2️⃣ 文件内容操作**

| 命令   | 功能描述               | 示例                          |
| ------ | ---------------------- | ----------------------------- |
| `cat`  | 查看文件内容           | `cat file.txt`                |
| `tac`  | 反向查看文件内容       | `tac file.txt`                |
| `more` | 分页查看文件           | `more largefile.txt`          |
| `less` | 分页查看（可向上翻页） | `less largefile.txt`          |
| `head` | 查看文件开头部分       | `head -n 10 file.txt`         |
| `tail` | 查看文件结尾部分       | `tail -f log.txt`             |
| `nano` | 编辑文本文件           | `nano file.txt`               |
| `vim`  | 强大的文本编辑器       | `vim file.txt`                |
| `grep` | 搜索文件中的内容       | `grep "hello" file.txt`       |
| `cut`  | 按列截取文件内容       | `cut -d ":" -f 1 /etc/passwd` |

✅ **常用参数**

- `cat file.txt | grep "error"` → 查找包含 `error` 的行
- `head -n 5 file.txt` → 查看前 5 行
- `tail -f log.txt` → 动态显示日志

------

### ✅ **3️⃣ 权限管理**

| 命令    | 功能描述             | 示例                       |
| ------- | -------------------- | -------------------------- |
| `chmod` | 修改文件权限         | `chmod 755 script.sh`      |
| `chown` | 修改文件所有者       | `chown user:user file.txt` |
| `chgrp` | 修改文件所属用户组   | `chgrp group file.txt`     |
| `umask` | 设置默认权限掩码     | `umask 022`                |
| `sudo`  | 以管理员权限执行命令 | `sudo apt update`          |

✅ **权限说明**

- `r` → 读取权限（4）
- `w` → 写入权限（2）
- `x` → 执行权限（1）
- 权限表示法：`rwxr-xr--`
  - 文件所有者 → `rwx`
  - 所属组 → `r-x`
  - 其他用户 → `r--`

✅ **修改权限**

```bash
chmod 755 file.sh  # 用户可读写执行，组和其他用户可读执行
chown root:root file.txt  # 修改文件所有者为 root
```

------

### ✅ **4️⃣ 系统管理**

| 命令           | 功能描述               | 示例            |
| -------------- | ---------------------- | --------------- |
| `ps`           | 显示当前进程           | `ps aux`        |
| `top` / `htop` | 实时查看系统资源和进程 | `top` / `htop`  |
| `free`         | 查看内存使用情况       | `free -h`       |
| `df`           | 查看磁盘空间           | `df -h`         |
| `du`           | 查看文件或目录大小     | `du -sh folder` |
| `uptime`       | 显示系统运行时间       | `uptime`        |
| `hostname`     | 查看主机名             | `hostname`      |
| `whoami`       | 显示当前用户           | `whoami`        |
| `uname -a`     | 查看系统版本信息       | `uname -a`      |

✅ **常用参数**

- `ps aux` → 显示所有进程
- `kill PID` → 结束指定进程
- `df -h` → 查看磁盘使用情况（人类可读）
- `free -m` → 查看内存使用情况（以 MB 为单位）

------

### ✅ **5️⃣ 网络相关命令**

| 命令       | 功能描述               | 示例                             |
| ---------- | ---------------------- | -------------------------------- |
| `ping`     | 检测与目标主机的连通性 | `ping google.com`                |
| `ifconfig` | 查看网络配置信息       | `ifconfig`                       |
| `ip a`     | 查看 IP 地址           | `ip a`                           |
| `netstat`  | 显示网络连接状态       | `netstat -an`                    |
| `ss`       | 显示 socket 信息       | `ss -tuln`                       |
| `curl`     | 请求 URL 并显示数据    | `curl http://example.com`        |
| `wget`     | 下载文件               | `wget http://example.com/file`   |
| `scp`      | 远程复制文件           | `scp file.txt user@server:/path` |
| `ssh`      | 远程连接服务器         | `ssh user@server`                |

✅ **常用参数**

- `ping -c 4 google.com` → Ping 4 次
- `curl -I example.com` → 查看 HTTP 头信息
- `scp file.txt user@192.168.1.100:/home/user` → 将文件复制到远程服务器

------

### ✅ **6️⃣ 打包与压缩**

| 命令     | 功能描述             | 示例                          |
| -------- | -------------------- | ----------------------------- |
| `tar`    | 打包文件或目录       | `tar -cvf archive.tar folder` |
| `gzip`   | 压缩文件（.gz 格式） | `gzip file.txt`               |
| `gunzip` | 解压 .gz 文件        | `gunzip file.txt.gz`          |
| `zip`    | 压缩文件             | `zip archive.zip file.txt`    |
| `unzip`  | 解压 zip 文件        | `unzip archive.zip`           |

✅ **常用参数**

- `tar -cvf` → 打包
- `tar -xvf` → 解包
- `gzip -d` → 解压 gzip 文件

------

### 🚀 **总结**

✅ 这些命令覆盖了 Linux 系统常用的文件管理、权限管理、系统监控、网络管理和打包压缩等操作。你可以在终端运行：

```bash
man <命令名>
```

查看该命令的完整用法。
 💡 如果你想了解更复杂的 Linux 命令或编写脚本，可以随时问我！ 🎯