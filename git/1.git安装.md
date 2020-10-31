git下载地址：https://git-scm.com/download

###### 账号环境配置

```shell
# 配置用户名
git config --global user.name "williamchen"    //（ "username"是自己的账户名，）
# 配置邮箱
git config --global user.email "chenzhiwei93@aliyun.com"  //("username@email.com"注册账号时用的邮箱)
# 查看配置
git config --global --list 
```

###### 生成ssh

```shell
# 1、命令生成ssh
ssh-keygen -t rsa

# 2、输入命令后回车三连
lenovo@DESKTOP-L683HO4 MINGW64 ~/Desktop/mynote (master)
$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/lenovo/.ssh/id_rsa):
Created directory '/c/Users/lenovo/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/lenovo/.ssh/id_rsa
Your public key has been saved in /c/Users/lenovo/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:flYswogqVktPYZlfhE8/gzaTWNmVXjbwGlPrwG9epA8 lenovo@DESKTOP-L683HO4
The key's randomart image is:
+---[RSA 3072]----+
|        ..o .oo. |
|      o..+ ...o+.|
|     =  =.+ .=ooo|
|    ..o+.B = .Bo |
|   o....S + =.E+.|
|  o.+  . . o  oo.|
|.... .  . o    ..|
|..       o       |
|                 |
+----[SHA256]-----+

# 3、进入C:\Users\用户名\.ssh 文件夹，然后将id_rsa.pub文件的内容复制到远程仓库的SSH keys

```

###### 本地操作

```shell
# 创建远程仓库 复制远程仓库的ssh地址 我的是 git@github.com:WlisonChan/mynote.git
git remote add origin git@github.com:WlisonChan/mynote.git		# 添加远程仓库的地址

git add . 					# 将本地文件添加到暂存区
git commit -am 				# 将暂存区的文件提价到本地仓库
git push -u origin master	# 将本地仓库的文件提交到远程仓库
# git push -f origin master 如果报错，可能存在远程仓库有文件，先pull再push，//tips：第一次提交可以强制提交到远程仓库，前提是远程仓库没有其他文件或者备份远程仓库。
```

