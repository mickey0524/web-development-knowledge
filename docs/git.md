* gitlab或github下fork后如何同步源的新更新内容

    1. 使用`git remote -v`查看远程状态
    2. 确定一个将被同步给fork远程的上游仓库`git remote add upstream https://github.com/ORIGINAL_OWNER/ORIGINAL_REPOSITOR`
    3. 再次查看状态确认是否配置成功
    4. 从上游仓库fetch分支和提交点，提交给本地master，并会被存储在一个本地分支upstream/master`git fetch upstream`
    5. 切换到本地主分支(如果不在的话)，`git checkout master`
    6. `git rebase upstream/master`这里不要用merge，会留下一条merge记录，pr会被拒绝的
    7. 如果想更新自己fork的分支，rebase之后`git push origin master`即可

* 如何在一台电脑上使用两个github账号

    有时候我们需要在一台电脑上push到两个github的repo，但是默认情况下github只适用于一个账号，这里介绍一下如何在一台电脑上向两个github提交更改的方法

    1. 生成新的ssh-key`$ ssh-keygen -t rsa -C "your_email@example.com"`
    2. 添加ssh key到github
    3. 配置多个ssh-key
        
        ```
        # default github
        Host github.com
        HostName github.com
        IdentityFile ~/.ssh/id_rsa

        Host github_second
        HostName github.com
        IdentityFile ~/.ssh/id_rsa_second
        ```

    4. 使用别名pull/push代码`git clone git@github_second:username/reponame.git`
    5. 如果设置了全局的git name/email 配置，还需要在刚拉下来的仓库内设置config

        ```
        git config user.name "xxxx"
        git config user.email "xxxx@xx.com"
        ```
