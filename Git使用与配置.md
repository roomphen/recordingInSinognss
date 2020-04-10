git类似于SVN的代码托管平台

1. 注册github账号
2. 电脑端下载git软件能实现本地代码上传到云端
3. git上开源项目能直接下载



git本地配置

1. 官网下载git

2. 设置用户名和登陆邮箱

3. 生成密钥

4. 网页上登陆GitHub配置ssh key

5. 代码查看是否配置成功

   注：在设置账号名称时注意代码之间的空格，不然会设置不成功，仍提示tell me who are you





git本地使用

注：注意空格

1. 在目标文件夹下初始化git

2. git add文件添加文件

3. git commit 添加日志

4. git check “名称”   检出文件夹

5. git diff查看不同

6. 记得看提示，比如每次add成功后都会提示changers to be commited，我们在add后需要添加commit日志

7. git log 查看日志，与版本号

8. git checkout -- .来取消对当前版本的修改回溯到上一最近一次版本的原本模样

   注：注意每个元素命令之间的空格

9. 通过log查看的版本号，我们可以进行版本回溯，使用”git reset --hard 版本号“版本号只需要填写前七位即可。它会连我们的commit日志都会回溯到之前的版本状态

10. git reflog查看版本内容

    查询到版本内容时，我们可以看见（head -> master)他表示了当前版本号

11. 解决中文乱码问题：git config --global core.quotepath false





##### 本地git和github关联使用，在我们配置好GitHub条件下

1. 关联项目：git remote add origin 地址

   若是在创建我们的repositordme.md或者LICENSE，那么GitHub会拒绝我的push，需要执行：git push -u origin master来将本地仓库上传到GitHub仓库进行关联

2. 我们在commit后想要同步到GitHub上，只需要执行: git push就行了(可能会出错，一般是丢失目的分支，我们在后面加上 origin master)

3. 第二步可能会出错，找不到origin下的master分支，我们可以通过git push -u origin master推送至origin master分支下

4. git本地新建分支

   ​		新建本地分支

   1. git branch hello_git_branch

   2. git checkout hello_git_branch

      push到远程仓库上

   3. git push origin hello_git_branch

      将分支提交到远程仓库的master上面

   4. git push origin hello_git_branch:master



git remote add origin

吧后面地址关联的来作为origin