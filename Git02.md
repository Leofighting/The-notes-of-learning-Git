## Git 命令行操作

#### 本地库初始化

`git init`

- 注意：`.git` 目录中存放的是本地库相关的子目录和文件，不要删除，也不要胡乱修改。

#### 设置签名

**配置用户名：**`git config --global user.name "你的用户名"`

- 修改用户名：`git config --global --replace-all user.name "新的用户名"`

**配置邮箱：**`git config --global user.email "你的邮箱"`

- 修改用户名：`git config --global --replace-all user.email "新的邮箱"`

**查看 Git 配置：**`git config --list`

**作用：**区分不同开发人员的身份

**辨析：****这里设置的签名和登录远程库(代码托管中心)的账号、密码没有任何关系**。

**级别：**

- 项目级别/仓库级别：仅在当前本地库范围内有效
  - `git config user.name "你的用户名"`
  - `git config user.email "你的用户名"`
  - **信息保存位置：`项目所在目录/.git/config` 文件**
- 系统用户级别：登录当前操作系统的用户范围（注意：`--global`）
  - `git config --global user.name "你的用户名"`
  - `git config --global user.email "你的用户名"`
  - **信息保存位置：`~/.gitconfig` 文件**

- 级别优先级

  - **就近原则：**项目级别优先于系统用户级别，二者都有时采用项目级别的签名

  - 如果只有系统用户级别的签名，就以系统用户级别的签名为准

  - 二者都没有不允许

## 基本操作

**状态查看**

`git status`：查看工作区、暂存区状态

**添加**

`git add [file name]`：将工作区的“新建/修改”添加到暂存区

**提交**

`git commit -m "commit message" [file name]`：将暂存区的内容提交到本地库

**查看历史记录**：`git log`

- `git log --pretty=oneline`：每次历史记录只显示一行信息

  ```shell
  3d64ab9458248bbb85d4ac136db3d4d35e7e71a8 (HEAD -> master) add a line
  b25e287954a6fc95de2527adaee40dc4e04f2974 first commit
  ```

- `git log --oneline`：功能与上一行命令类似，第一部分的 `hash值` 只显示一小部分

  ```shell
  3d64ab9 (HEAD -> master) add a line
  b25e287 first commit
  ```

- `git reflog`：显示移动数据

  ```shell
  3d64ab9 (HEAD -> master) HEAD@{0}: commit: add a line
  b25e287 HEAD@{1}: commit (initial): first commit
  ```

**前进后退**

基于索引值操作**[推荐]**

- git reset --hard [局部索引值]

- git reset --hard a6ace91
- **reset** **命令的三个参数对比**
  - `--soft 参数`：仅仅在本地库移动 HEAD 指针
  - `--mixed 参数`：在本地库移动 HEAD 指针，重置暂存区
  - `--hard 参数`：在本地库移动 HEAD 指针，重置暂存区，重置工作区

使用^符号：**只能后退**

- `git reset --hard HEAD^ `
- 注：一个^表示后退一步，n 个表示后退 n 步

使用~符号：**只能后退**

- `git reset --hard HEAD~n`

- 注：表示后退 n 步

**删除文件并找回**

- 前提：删除前，文件存在时的状态提交到了本地库。

- 操作：`git reset --hard [指针位置]`
  - 删除操作已经提交到本地库：指针位置指向历史记录
  - 删除操作尚未提交到本地库：指针位置使用 HEAD

**比较文件差异**

- `git diff [文件名]`：将工作区中的文件和暂存区进行比较
- `git diff [本地库中历史版本] [文件名]`：将工作区中的文件和本地库历史记录比较
- 不带文件名比较多个文件



## **分支管理**

分支：在版本控制过程中，使用多条线同时推进多个任务

**分支的好处**

- 同时并行推进多个功能开发，提高开发效率

- 各个分支在开发过程中，如果某一个分支开发失败，不会对其他分支有任何影响。失败的分支删除重新开始即可。

**分支操作**

- 创建分支：`git branch [分支名]`
- 查看分支：`git branch -v`
- 切换分支：`git checkout [分支名]`
- 合并分支
  - 第一步：切换到接受修改的分支（被合并，增加新内容）上：`git checkout [被合并分支名]`
  - 第二步：执行 merge 命令：`git merge [有新内容分支名]`
- 解决冲突：场景：不同分支对同一文件的同一地方进行修改
  - 第一步：编辑文件，删除特殊符号
  - 第二步：把文件修改到满意的程度，保存退出
  - 第三步：`git add [文件名]`
  - 第四步：`git commit -m "日志信息"` ； 注意：此时  `commit`  一定不能带具体文件名