###### 基本命令

```shell
git init			# 初始化仓库
git add .			# 添加文件到暂存区
git commit			# 将暂存区内容添加到本地仓库中
git status 			# 查看仓库当前的状态，显示有变更的文件

git clone [url]		# 拷贝一个 Git 仓库到本地，让自己能够查看该项目，或者进行修改

```



###### 版本查看

```shell
git log 					# 查看历史所有版本信息
git log -x 					# 查看最新的历史的x个版本信息
git log -x filename 		# 查看某个文件filename最新的x个版本信息（需要进入该文件所在目录）
git log --pretty=oneline	# 查看历史所有版本的信息，只包含版本号和记录信息

git reflog					# 可以查看所有分支的所有操作记录（包括已经被删除的 commit 记录和 reset 的操作）
```



###### 版本回退

```shell
git reset --hard HEAD^				# 回退所有的文件到上一个版本
git reset --hard 版本id	   	   	   # 回退到指定版本

#
```

