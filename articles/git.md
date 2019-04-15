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

