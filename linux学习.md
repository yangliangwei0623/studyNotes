# linux学习

## 1.常用命令

(1) ctrl c: 取消命令，并且换行
(2) ctrl u: 清空本行命令
(3) tab键：可以补全命令和文件名，如果补全不了快速按两下tab键，可以显示备选选项
(4) ls: 列出当前目录下所有文件，蓝色的是文件夹，白色的是普通文件，绿色的是可执行文件
(5) pwd: 显示当前路径
(6) cd XXX: 进入XXX目录下, cd .. 返回上层目录
(7) cp XXX YYY: 将XXX文件复制成YYY，XXX和YYY可以是一个路径，比如../dir_c/a.txt，表示上层目录下的dir_c文件夹下的文件a.txt
(8) mkdir XXX: 创建目录XXX
(9) rm XXX: 删除普通文件;  rm XXX -r: 删除文件夹
(10) mv XXX YYY: 将XXX文件移动到YYY，和cp命令一样，XXX和YYY可以是一个路径；重命名也是用这个命令
(11) touch XXX: 创建一个文件
(12) cat XXX: 展示文件XXX中的内容
(13) 复制文本
    windows/Linux下：Ctrl + insert，Mac下：command + c
(14) 粘贴文本
    windows/Linux下：Shift + insert，Mac下：command + v

## 2.tmux 、vim



## 3.shell



## 4.ssh

ssh-keygon 创建密钥





## 5 git

### 5.1 **git基本概念**

工作区：仓库的目录。工作区是独立于各个分支的。
暂存区：数据暂时存放的区域，类似于工作区写入版本库前的缓存区。暂存区是独立于各个分支的。
版本库：存放所有已经提交到本地仓库的代码版本
版本结构：树结构，树中每个节点代表一个代码版本。

### 5.2 git常用命令

再从一个账号push或者pull一个仓库时，要先将本地的公钥与账户相绑定

**本地处理**
1.git config --global user.name xxx：设置全局用户名，信息记录在~/.gitconfig文件中
2.git config --global user.email xxx@xxx.com：设置全局邮箱地址，信息记录在~/.gitconfig文件中
3.git init：将当前目录配置成git仓库，信息记录在隐藏的.git文件夹中
git add XX：将XX文件添加到暂存区
4.git add .：将所有待加入暂存区的文件加入暂存区
5.git rm --cached XX：将文件从暂存区中删掉，并且认为其不再被管理
git restore --staged XX: 将文件从暂存区中删掉,但认为其仍需被管理
6.git commit -m "给自己看的备注信息"：将暂存区的内容提交到当前分支
7.git status：查看仓库状态
8.git diff XX：查看XX文件相对于暂存区修改了哪些内容
9.git log：查看当前分支的所有版本
10.git reflog：查看HEAD指针的移动历史（包括被回滚的版本）
11.git reset --hard HEAD^ 或 git reset --hard HEAD~：将代码库回滚到上一个版本
git reset --hard HEAD^^：往上回滚两次，以此类推
git reset --hard HEAD~100：往上回滚100个版本
git reset --hard 版本号：回滚到某一特定版本、

**分支处理**

16.git checkout -b branch_name：创建并切换到branch_name这个分支
17.git branch：查看所有分支和当前所处分支
18.git checkout branch_name：切换到branch_name这个分支
19.git merge branch_name：将分支branch_name合并到当前分支上
20.git branch -d branch_name：删除本地仓库的branch_name分支
21.git branch branch_name：创建新分支

**云端处理**

12.git checkout — XX或git restore XX：将XX文件尚未加入暂存区的修改全部撤销（变回暂存区里的内容，暂存区没有内容就回到最新的版本）
13.git remote add origin git@git.acwing.com:xxx/XXX.git：将本地仓库关联到远程仓库
14.git push -u (第一次需要-u以后不需要)：将当前分支推送到远程仓库
git push origin branch_name：将本地的某个分支推送到远程仓库
15.git clone git@git.acwing.com:xxx/XXX.git：将远程仓库XXX下载到当前目录下
22.git push --set-upstream origin branch_name：设置本地的branch_name分支对应远程仓库的branch_name分支
23.git push -d origin branch_name：删除远程仓库的branch_name分支
24.git pull：将远程仓库的当前分支与本地仓库的当前分支合并
git pull origin branch_name：将远程仓库的branch_name分支与本地仓库的当前分支合并
25.git branch --set-upstream-to=origin/branch_name1 branch_name2：将远程的branch_name1分支与本地的branch_name2分支对应
26.git checkout -t origin/branch_name 将远程的branch_name分支拉取到本地

**git stash** （不咋常用）

27.git stash：将工作区和暂存区中尚未提交的修改存入栈中
28.git stash apply：将栈顶存储的修改恢复到当前分支，但不删除栈顶元素
29.git stash drop：删除栈顶存储的修改
30.git stash pop：将栈顶存储的修改恢复到当前分支，同时删除栈顶元素
31.git stash list：查看栈中所有元素