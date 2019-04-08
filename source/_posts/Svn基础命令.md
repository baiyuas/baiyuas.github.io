
# Ubuntu 18 使用svn命令

### 查看svn版本库列表

    $ svn list svn://192.168.0.218/andlife/源代码/trunk/hb_app/Android/db
    $ svn list svn://192.168.0.218/andlife/realse/package

### 检出svn版本库中指定目录到本地
	
	$ svn co svn://192.168.0.218/andlife/app 接口报文/ ~/文档/svn/

### 在svn版本库中创建目录

	$ svn mkdir svn://192.168.0.218/andlife/realse/package/Android/V2.0.2 -m "说明"

--parents 前面目录没有的话可以自动创建

	$ svn --parents mkdir svn://192.168.0.218/andlife/realse/package/Android/V2.0.2 -m "说明"

### 检出单个文件

因为`svn`的`checkout`命令是检出目录的，如果你指定文件检出会报错，所以要先检出空的目录在更新文件到本地

	$ svn co --depth=empty svn://192.168.0.x/project/test/ ~/test
	$ cd ~/test
	$ svn up xxxx.md

### 创建分支，合并分支

首先我们在SVN服务器上创建版本库 svn://xxx.xxx.x.xxx/repo/master

将主分支co下来

	leo:~/repo/$ svn co svn://xxx.xxx.x.xxx/repo/master

切换到主分支本地文件夹

	$ cd master 

创建文件，并写入文本,同时提交文件

	$ touch a.txt&&echo "This  is master file">a.txt && svn add a.txt && svn ci -m "master file"

创建分支

	$ svn copy svn://xxx.xxx.x.xxx/repo/master svn://xxx.xxx.x.xxx/repo/branch

这样在服务上就有了分支branch，我们可以将分支co本地，切换到分支

	$ svn co svn://xxx.xxx.x.xxx/repo/branch && cd branch

在分支创建文件，并提交

	$ touch b.txt&&echo "This  is branch file">b.txt && svn add b.txt && svn ci -m "branch file"

将分支增加的内容合并到主分支，首先切换到主分支的本地目录

	$ cd master

合并分支

	$ svn merge svn://xxx.xxx.x.xxx/repo/branch
会
查看合并文件的信息

	$ svn log -v b.txt

指定版本合并：

	$ svn merge url -c xxxx ./         (把svn 版本号为xxxx的改动合到你的本地)

	$ svn merge url -r xxxx:yyyy ./  (将url指定的code的xxxx版本到yyyy版本，merge到本地（注意：该方式不包括xxxx版本！！）)

### 解决冲突

	$ svn resolve –accept working a.txt 

### 回滚未提交的文件

	$ svn revert 

