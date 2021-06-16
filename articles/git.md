# git 常用命令

git branch -a
git checkout 远程


- 创建本地分支并切换过去：  
git branch iss53  
git checkout iss53



- 更新代码：  
在**本地分支**如下操作：  
git fetch origin 远程  
git merge origin/远程

- 提交代码：  
git checkout 远程  
git pull --rebase  
git merge 本地  
git push origin HEAD:refs/for/远程

通过命令行fetch拉取原仓库更新:
1. 配置当前当前fork的仓库的原仓库地址
`git remote add upstream <原仓库github地址>`
2. 查看当前仓库的远程仓库地址和原仓库地址
`git remote -v`
3. 获取原仓库的更新。使用`fetch`更新，`fetch`后会被存储在一个本地分支`upstream/master`上。
`git fetch upstream`
4. 合并到本地分支。切换到本地`master`分支，合并`upstream/master`分支。
`git merge upstream/master`
5. 这时候使用`git log`就能看到原仓库的更新了。
`git log`
6. 如果需要自己github上的fork的仓库需要保持同步更新，执行`git push`进行推送
`git push origin master`


如何在vscode中使用git命令行：
1. 设置
2. settings.json
3. 添加一行`"terminal.integrated.shell.windows": "D:\\Program Files\\Git\\bin\\bash.exe"`
