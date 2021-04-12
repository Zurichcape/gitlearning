## git(version 2.30.0.windows.1)学习笔记
----

1. **git 基本上传文件操作步骤**  

    - 创建需要上传的文件，然后执行以下操作  
    - `git init`  
    - `git add <file>`    
    - `git commit -m <message>("this is the description of this commit")`  

2. **git 版本回退**
    
    - 我们可以使用 `git log --pretty=oneline` 查看该git库提交历史。
    - `git reset --hard head~n` 用来执行回退操作，n代表回退的步长

    ![回退示意图1](./images/gitReset.png)  
    ![回退示意图2](./images/gitReset2.png)

    - 使用 `git reflog` 来查看该git库的提交命令历史，通过确定`commit_id`来确认需要回退的版本id

    ![快进指令图](./images/gitVersionCode.png)  

    - 因为有head指针，所以回退速度很快；对于已经上传到远程库的如果想做改动如(A->B->C，现在想回到B，那么只能做一个B的快照变为A->B->C->B，不能直接到B然后强行删除C`git push -f`(除非是个人项目))，只能在本地先做改动之后再执行如下命令。
    - `git pull`
    - `git push`
 
3. **工作区和暂存区**  

    ![工作区和版本库](./images/workArea.jpg)
    
    - 工作区就是电脑本地能看到的目录,包括隐藏目录`.git`，暂存区是暂时存放的已修改文件，
    - `git add` 实际上就是将文件修改添加到暂存区;`git commit`就是将暂存区的所有内容提交到当前分支。
    - 利用 `git status` 查看现有的文件是否被修改或是新创建的文件。
    
    [工作区和版本库2](./images/workArea2.jpg)  
    
    - `git diff` 比较工作区(work dict)和暂存区(stage)区域快照之间的差异，只显示已修改但尚未暂存的改动，即执行`git add`后该差异就会消失。
    - `git diff --cached`是暂存区(stage)和分支(branch)的比较。

4. **管理修改和撤销修改**
   
    - Git跟踪管理的是文件修改，而非文件本身。
    - 若只修改了文件内容而未执行`git add`命令，那么执行`git commit -m <message>`时并不会提交修改，因为没有执行`git add`的内容不会上传到暂存区，而只有上传到暂存区的内容才会被提交。对于多个需要上传的内容可以先全部执行`git add`后再执行`git commit -m  <message> 进行一次性上传`
    - 未保存到暂存区的文件可以执行`git restore <filename>`来撤销修改。
    - `git restore --worktree <filename>`从暂存区(stage)恢复到工作区(work dict)。
    - `git restore --staged <filename>`从master恢复到暂存区。
    - `git restore --soure=head --staged --worktree <filename>`从master恢复到暂存区和工作区。

5. **删除文件**

    - 对于已经提交到`branch`的文件，若只在工作区删除了文件可以通过4的命令来从暂存区或者branch恢复到工作区。
    - 如果已经执行了`git rm <filename>`即在暂存区删除了该文件，那么可以通过执行`git reset --hard head~`来回退版本号。

6. **建立远程库**

    - 首先在github上创建一个新的repository，然后通过`git remote origin git@github.com:xx/xx.git`与远程库建立联系，如果github库输入错误的话，需要执行`git remote rm origin`，然后再重新建立联系；可以使用`git remote -v`查看远程库信息。
    - `git pull --rebase origin <branch>`，当本地库和远程库不一致时需要先执行`pull`命令将远程库不一致的内容拷贝到本地，使用`git branch`可以查看本地branch的名字。
    - `git push -u origin <branch>`，将本地的`main`分支的内容上传到远程库`master`分支，参数`-u`能使两者关联起来以简化以后的命令。
7. **ssh-key的创建**

    - 在`git-bash`中执行`ssh-keygen -t rsa -C "youremail"`，然后执行`cat ~/.ssh/id_rsa.pub`，复制内容粘贴,然后打开`github/settings/SSH and GPG keys`新建一个`ssh`并粘贴。
    - 使用`ssh -T git@github.com`进行测试。
8. **远程库克隆**

    - `git clone git@github.com:xxxx/xxx.git`，git支持多种协议，其中`ssh`协议的速度最快，`https`协议速度最慢。

9. **创建于合并分支**

    - `git branch <branchname>`，创建新的分支
    - `git checkout <branchname>`，切换到新分支