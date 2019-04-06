### git remote修改

git commit -m "Change repo." # 先把所有未保存的修改打包为一个commit
git remote remove origin # 删掉原来git源
git remote add origin [YOUR NEW .GIT URL] # 将新源地址写入本地版本库配置文件
git push -u origin master # 提交所有代码


git stash
> git stash save 'message...'可以添加一些注释

git stash list
显示保存进度的列表。也就意味着，git stash命令可以多次执行。

git stash pop [–index] [stash_id]
> git stash pop 恢复最新的进度到工作区。git默认会把工作区和暂存区的改动都恢复到工作区。
git stash pop --index 恢复最新的进度到工作区和暂存区。（尝试将原来暂存区的改动还恢复到暂存区）
git stash pop stash@{1}恢复指定的进度到工作区。stash_id是通过git stash list命令得到的 
通过git stash pop命令恢复进度后，会删除当前进度。
git stash apply [–index] [stash_id]
除了不删除恢复的进度之外，其余和git stash pop 命令一样。

git stash drop [stash_id]
> 删除一个存储的进度。如果不指定stash_id，则默认删除最新的存储进度。

git stash clear
> 删除所有存储的进度。


### git删除远程分支
git branch -r -d origin/branch-name
git push origin :branch-name
