# git study

[TOC]

## 初始化一个github的test-git repository
```git
echo "# test-git" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/fingo24/test-git.git
git push -u origin main
```

### 以下是对上述每个Git命令的详细解释，结合官方文档及搜索结果进行说明：
 

1. `echo "# test-git" >> README.md`
	- 作用：创建项目说明文件  
	- 解析：
    - `echo`：命令行输出指令  
    - `# test-git`：写入的Markdown标题内容  
    - `>>`：将输出内容追加到文件末尾（若文件不存在则自动创建）  
    - 注意：若想覆盖原内容，应使用单个`>`符号  
    - 如何换行输出: echo要支持同C语言一样的\转义功能，只需要加上参数-e，如下所示：

      ```echo "Hello world.\nHello sea"```
	  Hello world.\nHello sea
	  ```echo -e "Hello world.\nHello sea"```
	  Hello world.
	  Hello sea

---
 
2. `git init` 
    - 作用：初始化本地Git仓库  
    - 原理：
      - 在项目根目录创建隐藏的`.git`文件夹，包含版本控制所需的所有元数据 
      - 生成默认分支（旧版本为`master`，新版本为`main`）  
    - 扩展用法：
      - `git init --bare`：创建裸仓库（无工作目录，用于服务器端共享）  
 
---

3. `git add README.md` 
	- 作用：将文件添加到暂存区（Staging Area）  
	- 阶段意义：
	  - 暂存区是提交前的"预检区"，允许选择性提交文件  
	  - 支持通配符操作：`git add *.md` 添加所有Markdown文件  
	- 高级操作：
	  - `git add readme.txt ant.txt`: 添加多个文件
	  - `git add -A`或者`git add --all`：添加所有修改/新增文件  
	  - `git add -p`：交互式选择部分代码片段暂存  
	  - `git add .`: 将所有文件添加到暂存区域 
	  - 区别: 
		  git add .：这个命令会将当前目录下的所有文件和文件夹的更改添加到暂存区，但不包括被忽略的文件（通过 .gitignore 文件指定）。它只会添加新创建的文件、修改过的文件和已删除的文件，对于未修改的文件不会进行操作。它不会处理重命名或移动的文件。
		  git add --all 或 git add -A：这个命令会将当前目录下的所有文件和文件夹的更改添加到暂存区，包括新创建的文件、修改过的文件、已删除的文件以及重命名或移动的文件。它会对整个工作树进行遍历，确保所有变化都被添加到暂存区。

---

4. `git commit -m "first commit"` 
	- 作用：将暂存区内容永久记录到本地仓库  
	- 参数解析：
	  - `-m`：直接附加提交信息（省略则会打开编辑器输入）  
	  - `"first commit"`：提交信息规范：首字母大写，使用祈使句（如"Add README"）  
	- 数据存储：
	  - 生成40位SHA-1哈希值（如`d0d1c5f`）作为唯一标识  
  	  - 可通过`git log`查看完整提交历史  
 
---
 
5. `git branch -M main`
	- 作用：重命名当前分支为`main`  
	- 背景说明：
	  - GitHub于2020年将默认分支名从`master`改为`main`  
	  - `-M`参数：强制重命名（若存在同名分支则覆盖）  
	- 分支管理：
	  - `git branch -a`：查看所有分支  
 	  - `git checkout -b dev`：创建并切换到`dev`分支  
 
---
 
6. `git remote add origin https://github.com/fingo24/test-git.git` 
	- 作用：关联远程仓库  
	- 组件解析：
	  - `origin`：远程仓库的默认别名（可自定义为`upstream`等）  
	  - URL格式：支持HTTPS/SSH两种协议（SSH需配置密钥）  
	- 验证命令：
	  - `git remote -v`：查看已配置的远程仓库地址  
 	 - `git remote rm origin`：删除错误配置的远程连接  
 
---
 
7. `git push -u origin main` 
	- 作用：将本地`main`分支推送到远程仓库  
	- 参数详解：
	  - `-u`（或`--set-upstream`）：建立追踪关系，后续可直接使用`git push`  
	  - `origin main`：远程仓库别名 + 目标分支  
	- 常见错误处理：
	  - 若远程存在旧版本：先执行`git pull --rebase origin main`  
	  - 强制覆盖：`git push -f origin main`（慎用，会丢失远程历史）  
 
---
 
8. 其他操作建议
	1) 首次推送前建议执行`git config --global user.name "Your Name"`设置用户名
	2) 使用`.gitignore`文件排除不需要版本控制的文件（如`node_modules/`）
	3) 推荐安装Git GUI工具（如SourceTree）辅助可视化操作 
 
	遇到问题可通过`git --help`查看帮助文档，或使用`git status`实时查看仓库状态。

---
## 如何把项目从git里移除
1. 本地移除项目
	如果你只是想从本地Git仓库中移除项目，但不打算影响远程仓库，你可以使用以下命令：
	移除项目文件夹：
	```rm -rf path/to/project```
	这将从你的文件系统中移除项目文件夹。
	从Git索引中移除：
	```git rm -r --cached path/to/project```
	这将从Git的跟踪列表中移除项目，但文件本身仍然存在于你的文件系统中。
	提交更改：
	```git commit -m "Remove project from tracking"```
	这将提交你的更改，使项目不再被Git跟踪。

2. 远程仓库中移除项目
	如果你希望在远程仓库中移除该项目，但保留在本地，你可以使用以下步骤：
	删除远程分支（如果该项目有单独的分支）：
	```git push origin --delete branch-name```
	这将删除远程仓库中的特定分支。
	如果整个仓库不再需要：
	如果你的整个仓库不再需要，你可能需要创建一个新的仓库或者迁移到另一个仓库。这种情况下，你可以：
	在新仓库中初始化一个新的项目。
	将旧仓库的内容手动复制到新仓库。
	提交并推送到新仓库。
	在旧仓库中删除所有内容，然后推送到远程以清理旧数据。这通常涉及到创建一个空的提交来删除所有文件，然后推送到远程：
	```bash
	git rm -rf .
	git commit --allow-empty -m "Remove all files"
	git push origin master --force
	```
	注意：使用--force选项会重写远程仓库的历史记录，这可能会对其他协作者造成影响，确保这是你真正想要做的。通常更好的做法是创建一个新的仓库并迁移内容。

3. 彻底删除所有痕迹（包括历史记录）
	如果你确实想要彻底删除项目及其所有历史记录（这在大多数情况下是不推荐的，因为这会丢失项目的完整历史），你可以考虑创建一个新的空提交来覆盖所有的历史记录：
	创建一个新的空提交：
	```echo 'Removing project history' | git commit-tree HEAD^{tree} -m "Remove project history" -p HEAD -f```
	强制推送到远程：
	```git push origin master --force```
	这将彻底删除项目的所有历史记录，包括所有的提交和文件更改。请确保这是你真正想要做的，因为它会永久删除项目的历史记录。
	总结
	选择哪种方法取决于你的具体需求。如果你只是想从本地或远程仓库中移除项目但不删除其历史记录，使用git rm和git push命令即可。如果你需要彻底删除项目的所有痕迹，包括其历史记录，那么创建一个新的空提交并强制推送到远程是一种方法，但请务必谨慎操作，确保不会意外丢失重要数据。对于大多数情况，建议先与团队成员沟通，确保这是最佳的操作方式。

4. ```git rm -rf .```用法解析
	```git rm```
		Git 的删除命令，用于从版本控制中移除文件（包括工作目录和暂存区）。
		-r（递归）
		递归删除目录及其子目录中的文件。如果删除的是一个目录，必须加此选项。
		-f（强制）
		强制删除，即使文件有未提交的修改也会被删除（覆盖 Git 的安全检查）。
		.（当前目录）
		表示操作范围是当前目录下的所有文件/目录。
		
	影响范围：
		仅删除已提交到 Git 仓库的文件（即已被 git add 或 git commit 过的文件）。未被跟踪的文件（如新创建但未 git add 的文件）不会被删除。

	对比其他命令:
		```rm -rf .```
		仅删除本地文件，不会更新 Git 的暂存区。
		需额外执行 ```git add -A``` 或 ```git rm``` 才能同步到 Git。
		```git clean -df```
		仅删除未被 Git 跟踪的文件（与 ```ggit rm```g 互补）。


---
## 几个常用的git语句
   1. 设置姓名
	`git config --global user.name ""`

   2. 设置邮箱
	`git config --global user.email "< your_email@email.com>"`

   3. 查看文件内容
	`cat testfile.txt`

   4. 重定向符号>和>>
	">" 符号：覆盖目标文件的内容。如果文件不存在，则创建文件。
	">>" 符号：追加内容到目标文件的末尾。如果文件不存在，则创建文件。

---

## 几个shell常用语句

  1. printf命令比echo更加强大，支持格式化输出。
   ```printf "这是一些文本内容\n" > filename.txt```
   
  2. 使用rm命令
   
   ```rm filename.txt```
   ```rm *.txt```
   
  3. 创建文件夹
   ```mkdir 新文件夹名```
   -p：递归创建目录，即创建多级目录。如果上级目录不存在，则会一并创建; 否则则会提示错误，并且无法创建出你所要创建的目录
   ```mkdir -p 父目录/子目录/子子目录```
   
  4. 删除文件夹
   删除空文件夹
   ```rmdir 文件夹名```
   删除非空文件夹(-r: 递归删除，用于删除子目录及文件)
   ```rm -r 文件夹名```

  5. ls详解
   [Linux系统命令-ls详解 ](https://www.cnblogs.com/weq0805/p/14873267.htm) 

		|  选项名称   | 说明  |
		|  -------  | --------------------------------------------------- |
		| -l  | 以长格式显示文件和目录的列表，包括权限、大小、最后更新时间等详细信息 |
		| -a  | 显示所有子目录和文件的信息，包括名称以点号“.”开头的隐藏目录和隐藏文件 |
		| -A  | 与-a选项的作用基本类似，但有两个特殊隐藏目录不会显示，即表示当前目录的“.”和表示父目录的“..” |
		| -d  | 显示目录本身的属性，而不是显示目录中的内容 |
		| -h  | 以更人性化的方式显示目录或文件的大小，默认的大小单位为字节(B)，使用-h选项后将显示为KB、MB等单位。此选项需要结合-l选项一起使用(-lh)|
		| -R  | 以递归(Recursive)的方式显示指定目录及其子目录中的所有内容|   		
		| -color  | 在字符模式中以颜色区分不同的文件。默认已开启 |
		| -i  | index node, inode 显示文件的缩影节点号 |

    - `ls -l`输出关联性 
			 完整字段示例：
			   ```shell 
			   -rw-r--r-- 1 user group 1024 Mar 13 15:33 example.txt 
			   drwxr-xr-x 2 user group 4096 Mar 13 15:34 mydir 
			   ```
			   - 第1字段：`-rw-r--r--`或`drwxr-xr-x`表示类型+权限 
			   - 后续字段：依次为硬链接数、所有者、所属组、大小、修改时间、名称

   - 补充说明:Linux文件权限
   	1) `rwxr-xr-x (755)` 解析 
			1. 结构分解
			   - 首字符：`d`或`-`（此处缺失，推测为目录）
			     - `d`代表目录，`-`代表普通文件（根据`ls -l`输出首字符判断）
			   - 权限分组：
			     - 用户(owner): `rwx` → 可读(r=4)、可写(w=2)、可执行(x=1) → 4+2+1=7
			     - 用户组(group): `r-x` → 可读、不可写、可执行 → 4+0+1=5 
			     - 其他用户(others): `r-x` → 同上 → 5 
			 
			2. 典型应用场景
			   - 目录权限：常见于系统目录如`/usr/bin`，允许用户浏览和执行文件，但禁止非所有者修改
			   - 可执行文件：如脚本文件需`x`权限才能运行 

		2) `-rw-r--r-- (644)` 解析 
			1. 结构分解
			   - 首字符：`-`明确表示为普通文件 
			   - 权限分组：
			     - 用户: `rw-` → 可读、可写、不可执行 → 4+2+0=6 
			     - 用户组: `r--` → 仅可读 → 4 
			     - 其他用户: `r--` → 同上 → 4 
			 
			2. 典型应用场景
			   - 配置文件：如`.bashrc`允许用户修改，其他用户仅可查看
			   - 文本文件：日志文件常设此权限防止误删改 

		3) 权限修改方法 
			1. 符号模式：
			   ```shell 
			   chmod u+x file    # 给所有者添加执行权限 
			   chmod go-w dir    # 移除组和其他用户的写权限 
			   ```
			2. 数字模式：
			   ```shell 
			   chmod 755 script.sh  # 设为rwxr-xr-x 
			   chmod 644 config.ini # 设为rw-r--r--
			   ```

		4) 特殊场景说明 
			1. 权限生成机制
			   - 新建文件默认权限为`666 - umask`（通常umask=022 → 644）
			   - 新建目录默认权限为`777 - umask`（通常umask=022 → 755）
			2. 可执行权限差异：
			  - 对目录：`x`表示可进入（如`cd mydir`需此权限）
			  - 对文件：`x`表示可直接运行（如`./app`）
			3. 危险权限组合：
			  - `rwxrwxrwx (777)`：所有用户可任意修改，需谨慎使用

	6. 通配符
		|  通配符   | 含义  |
		|  -------  | ---------------------|
		| ？（问号） | 代表任意-个字符，有且一个字符 |
		| * （星号） | 代表任意数个字符，可以是0个或者1个或者多个 |
		| [ ]（中括号） | 表示可以匹配字符组中的任意一个字符 |
	
	7. .的作用
		指代表示操作范围是当前目录下的所有文件/目录。
		如```open .```	
		会在finder打开当前目录的文件夹;
		如```git rm -rf .```
		强制递归删除当前目录下所有已被 Git 跟踪的文件和子目录。
	

---
# 杂项
## 关于.DS_Store文件
   核心功能: 存储文件夹的视图配置：包括图标位置、窗口大小、排序方式、背景颜色等.
   可安全删除: 删除后仅丢失文件夹的显示自定义设置，不会影响文件内容或系统稳定性.
   
   - 如何显示: 在macOS Mojave（10.14）及以上版本，即使启用隐藏文件显示，系统仍会强制隐藏.DS_Store文件。
	   需通过终端命令强制显示：
	   ```Bash
	   defaults write com.apple.Finder  AppleShowAllFiles true 
	   killall Finder  # 重启Finder生效 
	   ```
	   恢复隐藏：
	   ```Bash
	   defaults delete com.apple.Finder  AppleShowAllFiles 
	   killall Finder 
	   ```
   - Git仓库管理忽略 DS_Store 文件
    1. 将 . DS_Store 加入全局的 .gitignore 文件，执行命令：
	```echo .DS_Store >> ~/.gitignore_global```
	2. 将这个全局的 .gitignore 文件加入Git的全局config文件中，执行命令：
	```git config --global core.excludesfile ~/.gitignore_global```

echo .DS_Store >> ~/.gitignore_global

git config --global core.excludesfile ~/.gitignore_global

