[TOC]

# git study

## 初始化一个github的test-git repository

```bash
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

## 如何把某个项目从git里移除

1. 本地移除项目
	如果你只是想从本地Git仓库中移除项目，但不打算影响远程仓库，你可以使用以下命令：
	移除项目文件夹：
	`rm -rf path/to/project`
	这将从你的文件系统中移除项目文件夹。

	从Git索引中移除：
	`git rm -r --cached path/to/project`
	这将从Git的跟踪列表中移除项目，但文件本身仍然存在于你的文件系统中。

	提交更改：
	`git commit -m "Remove project from tracking"`
	这将提交你的更改，使项目不再被Git跟踪。

2. 远程仓库中移除项目
	如果你希望在远程仓库中移除该项目，但保留在本地，你可以使用以下步骤：
	删除远程分支（如果该项目有单独的分支）：
	`git push origin --delete branch-name`
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
	`echo 'Removing project history' | git commit-tree HEAD^{tree} -m "Remove project history" -p HEAD -f`
	强制推送到远程：
	`git push origin master --force`
	这将彻底删除项目的所有历史记录，包括所有的提交和文件更改。请确保这是你真正想要做的，因为它会永久删除项目的历史记录。
	总结
	选择哪种方法取决于你的具体需求。如果你只是想从本地或远程仓库中移除项目但不删除其历史记录，使用git rm和git push命令即可。如果你需要彻底删除项目的所有痕迹，包括其历史记录，那么创建一个新的空提交并强制推送到远程是一种方法，但请务必谨慎操作，确保不会意外丢失重要数据。对于大多数情况，建议先与团队成员沟通，确保这是最佳的操作方式。

4. `git rm -rf .`用法解析
	`git rm`
	Git的删除命令，用于从版本控制中移除文件（包括工作目录和暂存区）。
	-r（递归）
	递归删除目录及其子目录中的文件。如果删除的是一个目录，必须加此选项。
	-f（强制）
	强制删除，即使文件有未提交的修改也会被删除（覆盖 Git 的安全检查）。
	.（当前目录）
	表示操作范围是当前目录下的所有文件/目录。

	影响范围：
		仅删除已提交到 Git 仓库的文件（即已被 git add 或 git commit 过的文件）。未被跟踪的文件（如新创建但未 git add 的文件）不会被删除。

	对比其他命令:
		`rm -rf .`
		仅删除本地文件，不会更新 Git 的暂存区。
		需额外执行 `git add -A` 或 `git rm` 才能同步到 Git。
		`git clean -df`
		仅删除未被 Git 跟踪的文件（与 `git rm` 互补）。

---

## 如何把不小心init的目录（如根目录）从git中移除

1. 根目录:
	1) 验证根目录下是否有git文件(/: 根目录，可以改为其他目录; grep:文本搜索工具)
	`ls -al / | grep .git`
	精确匹配：若需严格匹配目录 .git（而非包含 .git 的其他文件），可改用：
	`ls -al / | grep ' \.git$'  # 匹配以 ".git" 结尾的行`

	2) 验证根目录验Git仓库状态 
	```bash
	cd /
	git status
	```
	3) 强制删除 .git 文件 (由于根目录权限较高，需使用 sudo 执行)
	```sudo rm -rf /.git```

2. 某个目录
	1) 进入某个目录然后验证git文件及git状态:
	```bash
	cd /path
	ls -al | grep .git
	git status
	```
	2) 删除git文件
	`rm -rf /path/to/parent/*/.git`（这会删除所有子目录的 Git 仓库）
	`rm -rf /path/to/parent/.git`（仅删除父目录的 Git 仓库）

3. 要查询本地所有被 Git 控制的文件夹位置（即包含 .git 目录的文件夹），可以通过以下方法实现:  
	```bash
	# 递归搜索当前目录下的所有 .git 文件夹，并输出其父目录路径
	find /path/to/search -name ".git" -type d 2>/dev/null | sed 's/\/.git//'
	```
	替换 /path/to/search：如搜索整个硬盘用 /（macOS/Linux），或指定目录如 ~/Documents;排除某些路径（如缓存目录）：
	```bash
	find / -name ".git" -type d 2>/dev/null -not -path "/tmp/*" -not -path "/node_modules/*"
	```

---

## 几个常用的git语句

1. 设置用户名及邮箱  
	在Git中设置用户名通常有两种方式：全局设置和项目（仓库）特定设置。
	全局设置:    
		```bash
		git config --global user.name "<your name>"
		git config --global user.email "< your_email@email.com>"
		git config --global --list		#查看全局配置
		```

	局部设置:
		```bash
		git config user.name "<your name>"
		git config user.email "< your_email@email.com>"
		git config --local --list		#查看局部配置或者git config --list
		```		

2. git fetch  
	`git fetch`：仅获取远程更新，不修改本地代码
		**示例**：
		```bash
		# 拉取所有远程分支的更新
		git fetch origin

		# 查看远程分支 origin/main 的更新记录
		git log origin/main
		```

	**关键区别总结**

		| 特性                | `git fetch`                          | `git pull`                          |
		|---------------------|--------------------------------------|--------------------------------------|
		| **操作类型**        | 仅下载远程更新                       | 下载更新 + 合并到本地分支            |
		| **本地分支修改**    | 不修改本地分支                       | 自动合并到当前本地分支               |
		| **工作目录变化**    | 无变化                               | 可能触发合并冲突并修改代码           |
		| **控制权**          | 需要手动决定如何合并（更安全）       | 自动合并（可能需解决冲突）           |
		| **适用场景**        | 需先检查远程变更再合并               | 快速同步本地分支                     |


	**何时用 `git fetch`？何时用 `git pull`？**
		1) **推荐使用 `git fetch`**：
		   - 当你需要查看远程仓库的变更内容，再决定是否合并时。
		   - 当本地有未提交的修改，且不想触发自动合并冲突时。
		   - 当你希望手动控制合并或使用 `git rebase` 保持提交历史线性时。

		2) **推荐使用 `git pull`**：
		   - 当你确定远程分支的更新可以直接合并到本地时。
		   - 当你想快速同步本地分支，且没有未提交的修改时。

	**高级用法**
		1) **`git pull --rebase`**：
		   - 拉取远程更新后，用 `rebase` 代替 `merge` 合并提交，保持提交历史线性。
		   - 等同于 `git fetch` + `git rebase origin/main`。

		2) **手动合并流程**（替代 `git pull`）：
		   ```bash
		   git fetch origin        # 拉取远程更新
		   git merge origin/main   # 手动合并到当前分支（或改用 git rebase）
		   ```

3. 修正远程仓库关联
	```bash
	git remote remove origin
	git remote add origin <正确的远程仓库URL>
	```

4. git diff
	用于比较代码的差异。它可以比较 工作目录、暂存区（索引）、本地仓库提交 之间的文件变化。

	1) 比较工作目录和暂存区（未暂存的修改）:
		```bash
		git diff           # 查看未解决的冲突
		git diff --ours    # 查看“当前分支”的修改
		git diff --theirs  # 查看“合并进来的分支”的修改
		```

		diff 常用选项：

			-q：只输出两个文件内容是否是一致的，不输出具体哪些内容不一致。

			-i：忽略大小写的差异。

			-w：忽略所有空白字符的差异。

			-b：忽略空格的数量差异。

	2) 比较暂存区和最新提交（已暂存的修改）:
		`git diff --staged`或者`git diff --cached`

	3) 比较不同提交或分支
		比较两个提交之间的差异:
			`git diff <commit1> <commit2>`
			示例：
			```bash
			git diff abc123 def456      # 比较两个提交的差异
			git diff HEAD~2 HEAD        # 比较前两次提交和当前最新提交的差异
			```

		比较当前工作目录与某个提交:
			`git diff <commit>``
			示例：
			```bash
			git diff HEAD       # 比较工作目录与最新提交的差异（包括未暂存和已暂存的修改）
			git diff abc123     # 比较工作目录与某个历史提交的差异
			```

	4) 比较两个分支的差异
		`git diff branch1..branch2`   # 比较两个分支最新提交的差异

	5) 比较单个文件的差异
		`git diff <file-path>`
		示例：
		```bash
		git diff src/app.js        # 仅查看 app.js 文件的未暂存修改
		git diff HEAD -- README.md # 比较 README.md 在最新提交和工作目录的差异
		```

	6) 其他高级用法:
		查看统计信息（不显示具体差异）:
			`git diff --stat`

		忽略空格变化:
			```bash
			git diff --ignore-space-change   # 忽略空格数量的变化
			git diff --ignore-all-space      # 忽略所有空格变化
			```

		输出差异到文件:
			`git diff > changes.patch   # 将差异保存为补丁文件`

		输出内容过多:
			退出分页器：直接按 q 键; 临时禁用分页器：`git --no-pager diff`



---

## 几个shell常用语句

1. printf命令比echo更加强大，支持格式化输出  
   `printf "这是一些文本内容\n" > filename.txt`
 
2. 使用rm命令     
	`rm filename.txt`
	`rm *.txt`
		`rm -i *.?`交互式执行rm命令，每个匹配的文件将依次显示。如果回答“y”或“Y”，则文件将被删除。如果回答其他，则保留该文件。
 		
3. 创建文件夹     
   `mkdir 新文件夹名`
   -p：递归创建目录，即创建多级目录。如果上级目录不存在，则会一并创建; 否则则会提示错误，并且无法创建出你所要创建的目录
   `mkdir -p 父目录/子目录/子子目录`
 
4. 删除文件夹     
   删除空文件夹
   `rmdir 文件夹名`
   删除非空文件夹(-r: 递归删除，用于删除子目录及文件)
   `rm -r 文件夹名`

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

   - `ls -l`输出关联性:

			 完整字段示例:
			   ```shell 
			   -rw-r--r-- 1 user group 1024 Mar 13 15:33 example.txt 
			   drwxr-xr-x 2 user group 4096 Mar 13 15:34 mydir 
			   ```
			   - 第1字段：`-rw-r--r--`或`drwxr-xr-x`表示类型+权限 
			   - 后续字段：依次为硬链接数、所有者、所属组、大小、修改时间、名称

   - 补充说明:Linux文件权限:

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

6. 通配符:     
		|  通配符   | 含义  |
		|  -------  | ---------------------|
		| ？（问号） | 代表任意1个字符，有且一个字符 |
		| * （星号） | 代表任意数个字符，可以是0个或者1个或者多个 |
		| [ ]（中括号） | 表示可以匹配字符组中的任意一个字符 |

7. .的作用     
		指代表示操作范围是当前目录下的所有文件/目录。
		如`open .`	
		会在finder打开当前目录的文件夹;
		如`git rm -rf .`
		强制递归删除当前目录下所有已被 Git 跟踪的文件和子目录。

8. 查看文件内容     
		`cat testfile.txt`

9. 输出重定向符号>和>>     
		">" 符号：覆盖目标文件的内容。如果文件不存在，则创建文件。
		">>" 符号：追加内容到目标文件的末尾。如果文件不存在，则创建文件。	
		举例:  
		```shell
		echo "Hello, World!" > output.txt
		ls -l > files.txt    #将ls命令的输出重定向到文件
		ls /nonexistdir 2>> error.txt		#将错误输出重定向到文件（追加到原有内容后面）
		ls /nonexistdir > out.txt 2>&1  #错误输出和标准输出到同一文件（清除原有文件的数据）
		```

		- 重定向文件描述符(对于重定向中的标准输出模式，可以省略文件描述符1 不写；而错误输出模式的文件描述符 2是必须要写的):
			标准输入(stdin)重定向（文件描述符为 0）：默认从键盘输入，也可从其他文件或命令中输入
      标准输出(stdout)重定向（文件描述符为 1）：默认输出到屏幕
      错误输出(stderr)重定向（文件描述符为 2）：默认输出到屏幕
    
    - /dev/null 文件
    	如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null
    	如"who > /dev/null"
   	
10. 输入重定向符<     
		在Shell中，我们使用'<'操作符来进行输入重定向。我们可以将文件内容作为命令的输入。

		`grep "Hello" < file.txt` #此命令会将file.txt的内容作为grep命令的输入，搜索包含"Hello"的行。

		例子:
		多命令协同输入:
			`cat < input.txt  | grep "error" | sort > output.txt  # 将文件内容通过管道传递给grep过滤，再重定向给sort排序`
		
		<< tag: 将开始标记 tag 和结束标记 tag 之间的内容作为输入。
			Here Document（即时文本输入）:
			```bash
				cat << EOF > mymessage.txt
				Hello,
				This is a message.
				 
				Best Regards,
				我是一个例子
				EOF
				```

11. 管道操作符('|')与列表操作符('&&'和'||')     
		管道操作符(将一个命令的输出作为另一个命令的输入):
		`ls -l | grep "txt"` #此命令会列出当前目录的所有文件信息并寻找包含"txt"的行

		'&&'操作会在前一命令成功执行后继续执行下一个命令:
		`cd /dir && ls` #此命令会先尝试进入/dir目录，如果成功则继续执行ls命令，否则不会执行ls命令。

		'||'操作会在前一命令失败后才会执行下一个命令:
		`cd /nonexistdir || echo "Failed to change directory"` #此命令会尝试进入/nonexistdir目录，如果失败则会输出"Failed to change directory"。

12. patch用法     
		把xxx.patch和要打补丁的文件放到一起到要打补丁文件 目录下执行上述命令会把补丁打进去更新现有文件
		```bash
		# 打补丁 
		patch -p1 < project.patch  
		 
		# 回滚补丁 
		patch -R -p1 < project.patch 
		```

		-p<num>用于剥离路径层级，如-p1会忽略补丁中的第一级目录
			示例：
				若补丁文件中的路径为a/src/main.c
				-p0：保留完整路径 a/src/main.c
				-p1：截取为 src/main.c（常用场景）
				-p2：截取为 main.c（当补丁生成路径与当前目录层级不匹配时使用）

		使用find处理多个补丁文件:
			`find patches/ -name "*.patch" -exec patch -p1 < {} \;`
			解析:递归搜索 patches/ 目录下的所有 .patch 文件；对每个找到的 .patch 文件执行 patch 命令({} 是占位符，会被替换为当前找到的文件路径（例如 patches/fix-bug1.patch），`\;` 表示命令结束)

13. who和w     
		w：展示哪些用户正在登录
		who：展示哪些用户正在登录

14. wc(word count)     
		默认输出为 行数、单词数、字节数

		-l 统计文件的行数
		-w 统计文件的词数
		-m 统计文件的字符数
		-c 统计文件的字节数

		举例:
		统计所有txt文件的行数
		`wc -l *.txt`  

		```bash
		$ wc -l << EOF
    欢迎来到
    菜鸟教程
    www.runoob.com
		EOF	 # 输出结果为 3	
		```

15. find     
		find [起始目录] [查找条件] [操作]

		[查找条件]
			-name：按名称查找文件。
			-type：按类型查找文件，如 f 表示普通文件，d 表示目录。
			-size：按大小查找文件，可以使用 c（字节）、k（千字节）、M（兆字节）等单位。
			-mtime：按内容最后修改时间查找文件，以天为单位。
			-atime：按访问时间查找文件，以天为单位。
			-ctime：按元数据（如权限、所有权）最后修改时间查找文件，以天为单位。
			-mmin、-amin、-cmin：与 -mtime、-atime、-ctime 类似，但以分钟为单位。
			-perm：按权限查找文件。
			-user 和 -group：按所有者和组查找文件。
			-newer：查找比指定文件更新（修改时间更晚）的文件。

		[操作]
			-exec：对找到的文件执行指定的命令。
			-ok：与 -exec 类似，但在执行命令前会提示用户确认。
			-print：打印找到的文件路径（默认操作）。

		举例:
		```bash
		find . -name "example.txt"				#查找当前目录及其子目录中的名为 example.txt 的文件 
		find /home -type d -name "temp"   #查找 /home 目录下所有名为 temp 的目录
		find / -size +10M									#查找大于 10MB 的文件
		find /path/to/search -mtime -7		#查找最近 7 天内修改过的文件
		find / -perm 755									#查找权限为 755 的文件
		find / -user john									#查找属于用户 john 的文件
		find / -newer /etc/passwd					#查找比某个文件更新的文件
		find . -name "*.tmp" -exec rm -f {} \;	#删除当前目录及其子目录中的所有 .tmp 文件
		find /tmp -name "*.log" -exec mv {} /var/logs/ \;	#将 /tmp 目录下所有扩展名为 .log 的文件移动到 /var/logs 目录

		```

16. grep  
		grep 是 Linux 中用于搜索文本的强大命令，它能根据给定的模式在文件或标准输入中查找匹配的行，并将包含匹配模式的行输出。
		
		grep [选项] 搜索模式 文件  
		搜索模式：可以是一个字符串或正则表达式，用来匹配文件中的内容。 
		文件：指定要搜索的文件或路径。  

		[常用选项]  
			示例:  
				```shell
				[root@test test]# cat test.txt
				ni hao lisi
				hello zhangsan
				wo shi wangwu
				hello zhaoliu
				[root@test test]# grep "hello" test.txt
				hello zhangsan
				hello zhaoliu
				```
			-i：忽略大小写
			-v：反向匹配,只显示不匹配搜索模式的行
			-r 或 -R：递归搜索目录
			-n：显示行号
			-c：只显示匹配的行数,而不是具体内容。
			-l 和 -L：只显示文件名
				```bash
				[root@test test]# grep -l "hello" *.txt
				test.txt
				```
			-w：匹配整个单词,只匹配完整的单词，而不是单词的一部分。
			-x：匹配整行,只有当整行匹配搜索模式时才显示该行。
				```bash
				[root@test test]# grep -x "hello" test.txt
				[root@test test]# grep -x "hello zhaoliu" test.txt
				hello zhaoliu
				```
			-A、-B、-C：显示上下文行
				-A n：显示匹配行之后的 n 行。
				-B n：显示匹配行之前的 n 行。
				-C n：显示匹配行前后各 n 行。
			-E：使用扩展正则表达式,启用扩展正则表达式（等同于 egrep），支持更多正则表达式操作符，如 |, +, ?, {} 等。
				次数匹配模式如下：
	        \*: 任意次
	        ?：0或1次
	        +: 至少1次
	        {m}：精确匹配m次；
	        {m,n}：至少m次，至多n次；
	        {m,}：至少m次；
	        {0,n}：至多次；
	      `grep -E "hello|world" file.txt`
	    -f：从文件中读取模式,从指定文件中读取多行模式进行搜索。
	    	`grep -f pattern.txt file.txt`	在 file.txt 中查找 pattern.txt 文件中的每个模式。
	    -o：仅输出匹配部分,而不是整行。

	  grep 正则表达式示例：  
	  	匹配开头的字符串 ^：`grep "^hello" file.txt`
	  	匹配行尾的字符串 $：`grep "world$" file.txt`
	  	匹配任意单字符 .：`grep "h.llo" file.txt`
	  	匹配多个字符 \*：grep "hel\*o" file.txt
	  	匹配字符范围 [a-z]：`grep "h[a-z]llo" file.txt`
	  	匹配多个模式 |（需配合 -E 或 egrep 使用）：`grep -E "hello|world" file.txt`

	  实用示例:  
	  	在系统日志中查找错误信息:  
	  		```bash
	  		[root@test test]# grep -i "error" /var/log/messages
				Nov 10 15:04:18 test kernel: BERT: Boot Error Record Table support is disabled. Enable it by using bert_enable as kernel parameter.
				Nov 10 15:04:25 test mcelog: ERROR: AMD Processor family 23: mcelog does not support this processor.  Please use the edac_mce_amd module instead.
				```
			查找包含多个关键字的行:
				```bash
				[root@test test]# grep -i "error" /var/log/messages |grep "Unable"
				Nov 10 15:04:27 test libvirtd: 2024-11-10 07:04:27.035+0000: 1432: error : virHostCPUGetTscInfo:1389 : Unable to open /dev/kvm: No such file or directory
				Nov 10 15:04:27 test libvirtd: 2024-11-10 07:04:27.173+0000: 1432: error : virHostCPUGetTscInfo:1389 : Unable to open /dev/kvm: No such file or directory
				Nov 10 15:04:27 test libvirtd: 2024-11-10 07:04:27.174+0000: 1432: error : virHostCPUGetTscInfo:1389 : Unable to open /dev/kvm: No such file or directory
				```
			忽略二进制文件搜索文本:  
				`grep -I "hello" *`
					-I	忽略二进制文件，仅搜索文本文件。
					\*	Shell通配符，匹配当前目录下的所有非隐藏文件和子目录名。
			从命令输出中查找特定行:  
				```bash
				[root@test test]# dmesg |grep "usb"
				[    0.678613] usbcore: registered new interface driver usbfs
				[    0.678618] usbcore: registered new interface driver hub
				[    0.678720] usbcore: registered new device driver usb
				```

17. cp  
	复制文件或目录
	`cp /home/user/file.txt /home/user/Documents`

18. mv  
	移动或重命名文件或目录
	`mv /home/user/file.txt /home/user/Documents/newfile.txt`

19. ps  
	显示当前运行的进程。
	`kill [PID]` 结束某个进程

	使用kill -l 可以列举所有支持的信号：9) SIGKILL 和 15) SIGTERM 

	kill(默认值`kill -15`)和`kill -9`的区别: 
		在使用`kill -9`前，应该先使用`kill -15`，给目标进程一个清理善后工作的机会。如果没有，可能会留下一些不完整的文件或状态，从而影响服务的再次启动。
 	
 	- ps -ef 和ps aux的区别(System V和BSD代表了Unix的两种主要的Linux操作风格):
	 	ps -ef 是用标准的格式(System Ｖ风格)显示进程的、其格式如下,其中各列的内容意思如下:
	 		UID    //用户ID、但输出的是用户名 
			PID    //进程的ID 
			PPID    //父进程ID 
			C      //进程占用CPU的百分比 
			STIME  //进程启动到现在的时间 
			TTY    //该进程在那个终端上运行，若与终端无关，则显示? 若为pts/0等，则表示由网络连接主机进程。 
			CMD    //命令的名称和参数

		ps aux 是用BSD的格式来显示、其格式如下,同ps -ef 不同的有列有:
			USER      //用户名 
			%CPU      //进程占用的CPU百分比 
			%MEM      //占用内存的百分比 
			VSZ      //该进程使用的虚拟內存量（KB） 
			RSS      //该进程占用的固定內存量（KB）（驻留中页的数量） 
			STAT      //进程的状态 
			START    //该进程被触发启动时间 
			TIME      //该进程实际使用CPU运行的时间

			其中STAT状态位常见的状态字符有
				D      //无法中断的休眠状态（通常 IO 的进程）； 
				R      //正在运行可中在队列中可过行的； 
				S      //处于休眠状态； 
				T      //停止或被追踪； 
				W      //进入内存交换 （从内核2.6开始无效）； 
				X      //死掉的进程 （基本很少见）； 
				Z      //僵尸进程； 
				<      //优先级高的进程 
				N      //优先级较低的进程 
				L      //有些页被锁进内存； 
				s      //进程的领导者（在它之下有子进程）； 
				l      //多线程，克隆线程（使用 CLONE_THREAD, 类似 NPTL pthreads）； 
				+      //位于后台的进程组；

	- 在 macOS 的终端（Shell）中，如果进程被中断（例如通过 Ctrl+Z 暂停），可以通过以下方法重新进入或恢复进程:
		```bash
		jobs       # 查看作业号
		fg %1     # 恢复第一个作业到前台
		bg %2   # 恢复作业号为 2 的进程到后台
		```

	- 遇到有时候无法kill的进程，除了kill -9之外:可以先用`bg %1`使程序后台跑完或者在后台跑时候再kill

20. paste  
	它的主要用途是将多个文件的行合并在一起，默认情况下，它会将将多个文件按照列队列进行合并，并使用制表符（\t）作为分隔符。paste 命令不会修改原始文件，它只会输出合并后的内容到标准输出  
	`paste [OPTION]... [FILE]...`
	[OPTION]	
		-d, --delimiters=LIST：使用 LIST 中的字符作为分隔符，而不是制表符。例如，-d',' 将使用逗号作为分隔符。
			'-->'也只会显示'-'
		-s, --serial：将每个文件的行串行化，而不是并行合并。这会将每个文件的所有行合并为一个长行。

21. locate  
	locate命令用于在Linux系统中快速查找文件或目录的路径。
	它通过搜索一个预先构建的数据库（通常是/var-lib/mlocate/mlocate.db）来工作，而不是实时搜索整个文件系统，因此搜索速度非常快。这个数据库由updatedb命令定期更新，通常每天或根据系统配置更新一次。
	`locate example.txt`

22. which  
	which 命令在 Linux 系统中用于查找可执行文件的位置。它会在环境变量 PATH 所指定的目录中搜索指定的命令，并返回第一个找到的命令的完整路径。
	```bash
	which [选项] 命令名
	which ls
	which -a java
	```

23. whereis  
	`whereis` 是 Linux 系统中一个用于查找二进制文件、源代码文件和手册页文件的命令。它基于预先构建的数据库进行搜索，因此速度相对较快，但可能无法找到最近安装或更新的文件。  
	`whereis` 命令搜索以下三个标准位置：
  	1）. 二进制文件（通常位于 `/usr/bin`、`/usr/sbin`、`/bin` 或 `/sbin`）。
  	2）. 源代码文件（通常位于 `/usr/src` 或 `/usr/local/src`）。
  	3）. 手册页（通常位于 `/usr/share/man`）。
	
	`whereis [选项] [文件名]`  
	[选项]  
		`-b`：只查找二进制文件。
    `-m`：只查找手册页文件。
    `-s`：只查找源代码

24. type  
	type 命令是 Linux 系统中一个强大而常用的工具，用于确定给定命令的类型（内建命令、外部命令、别名等）。
	`type command_name`
	```bash
	type echo 	#查找内建命令
	type ls 		#查找外部命令
	type if 		#检查关键字
	```

25. alias  
	如果你经常使用某些 Linux 命令，你可以使用 alias 命令为这些常用命令创建一个短名，以提升你的工作效率。
	**临时别名仅在当前终端会话有效，关闭后消失**
	例`alias ll='ls -al'`
	```bash
	alias 				# 列出所有别名
	alias ll 			# 查看 ll 对应的具体命令
	unalias 别名		# 别名删除语法
	unalias -a 		# 删除当前Shell环境中所有的别名
	```

	高级技巧  
	 1) 别名持久化
	　　将别名写入 Shell 配置文件，永久生效：
	　　Bash 用户：
	　　```echo ‘alias ll=“ls -alh”’ >> ~/.bashrc
	　　source ~/.bashrc # 立即生效```
	　　Zsh 用户：
	　　```echo ‘alias ll=“ls -alh”’ >> ~/.zshrc
	　　source ~/.zshrc```
	 2) 组合命令
	　　用&&串联多个操作：
	　　`alias update=“sudo apt update && sudo apt upgrade -y”`
	　　输入update即可一键更新系统。
	 3) 绕过别名调用原命令
	　　在别名命令前加\可临时禁用别名：
	　　`\ls # 调用原生 ls 而非别名`           
	 4) 注意事项:  
		 避免覆盖重要命令：
	　　`alias ls=“ls -l” # 可能破坏脚本中对 ls 的依赖`
	　　兼容性问题：不同 Shell（Csh/Bash/Zsh）配置文件路径名字不同。
	　　慎用危险操作：如
	　　`alias sudo=“sudo rm -rf /”（绝对禁止！）`

26. source   
	source 命令在 Linux 和其他 Unix-like 系统中用于在当前 shell 会话中读取并执行指定文件中的命令。这意味着，当你使用 source 命令运行一个脚本时，该脚本中的变量、函数和其他 shell 特性都会在当前 shell 会话中生效，而不仅仅是在子 shell 中。

	`source filename`等同于`. filename`  
	  在这里，filename 是想要在当前 shell 会话中运行的脚本的名称。

	  举例1:
	  如果只是简单地运行这个脚本（例如使用 `./vidisit_variables.sh` 或 `bash vidisit_variables.sh`），那么 MY_VARIABLE 变量只会在子 shell 中存在，并在脚本结束后消失。如果在脚本的同一 shell 会话中使用 source 命令运行它，那么 MY_VARIABLE 变量就会在当前 shell 会话中设置，并且可以在当前 shell 会话中访问它。

	  假设有一个名为 vidisit_variables.sh 的脚本，内容如下：
	  ```bash
	  #!/bin/bash 
		 
		 
		MY_VARIABLE="Hello, vidisit.cn!" 
		echo "This is in the script: $MY_VARIABLE"
		```

	 	```bash
	 	[root@ecs-52a1 home]# sh vidisit_variables.sh 
		This is in the script: Hello, vidisit.cn!
		[root@ecs-52a1 home]#
		[root@ecs-52a1 home]#echo $MY_VARIABLE
		 
		[root@ecs-52a1 home]#
		[root@ecs-52a1 home]#
		[root@ecs-52a1 home]# source vidisit_variables.sh 
		This is in the script: Hello, vidisit.cn!
		[root@ecs-52a1 home]#
		[root@ecs-52a1 home]#
		[root@ecs-52a1 home]# echo $MY_VARIABLE
		Hello, vidisit.cn!
		[root@ecs-52a1 home]#
		[root@ecs-52a1 home]#
		```

		权限小问题:  
			若是直接`./vidisit_variables.sh`,会提示`zsh: permission denied: ./vidisit_variables.sh`
			错误原因:Unix-like 系统（包括 macOS）中，直接运行脚本文件需满足以下条件：
				可执行权限：文件必须被标记为“可执行”（通过权限位 x）。
				正确的解释器路径：脚本首行需指定解释器（如 #!/bin/bash），但这不影响权限错误。
				若未赋予权限，系统会拒绝执行脚本并提示 Permission denied。
			解决方案:  
				使用 chmod 命令为脚本添加执行权限：`chmod +x vidisit_variables.sh`
				验证权限:`ls -l vidisit_variables.sh`


	  举例2:
		 	刷新当前 shell 环境: 当修改了 .bashrc 或 .profile 等配置文件后，可以使用 source命令重新加载它们，使新的配置立即生效，而无需注销并重新登录。
		 	`source ~/.bashrc`

27. sh  
	在 macOS 的终端中输入 sh 后出现 sh-3.2$，表示你已进入 Bourne Shell 兼容模式（sh 的子 Shell 环境）。

	如何退出 sh 环境？直接输入 exit;按下组合键 Ctrl + D

	修改默认 Shell（可选）：
		```bash
		echo $SHELL  # 查看默认 Shell
		echo $0       # 查看当前 Shell 类型
		```

	修改默认 Shell（可选）:
		```bash
		chsh -s /bin/bash  # 修改为 bash
		chsh -s /bin/zsh   # 修改为 zsh
		```

28. du  
	du命令是用于显示目录或文件占用的磁盘空间。
		显示人类可读格式（-h）
		仅显示总计（-s）,而不显示其子目录的大小。
		-c: 除了显示目录大小外，额外一行显示总占用量
		-d: 是 --max-depth=N 选项的简写，表示深入到第几层目录,超过指定层数目录则忽略；比如-d1，-d2

	`du`：统计当前目录（含隐藏项），逐层递归子目录并汇总总大小。du ./* 会单独列出文件大小，而 du 仅统计目录（文件大小被包含在所属目录中，不单独显示）。
	`du ./*`：./* 通配符不匹配隐藏文件（如 .git）,分别处理每个非隐藏条目（文件/目录），显示独立结果，不包含隐藏项和当前目录总汇总。

	显示目录的总和大小
	`[root@jeven ~]# du -sh /etc/sysconfig/`

	查看当前目录下文件所占的空间
	`[root@jeven ~]# du -sh ./*`

	将目录下的文件从大到小进行排序
	`[root@jeven ~]# du -sh /etc/sysconfig/*  |sort -rh`

29. df  
	`df -h`显示磁盘使用空间

30. 权限操作命令:  
	1) chmod：更改文件或目录的权限  
		`chmod 755 /home/user/file.txt`
		表示设置文件的主用户有读，写和执行权限（rwx = 7），同组的用户和其他用户有读和执行权限（rx = 5）

	2) chown：更改文件或目录的所有者和所属的组  
		`chown newuser:newgroup /home/user/file.txt`
		表示将/home/user/file.txt的所有者更改为newuser，所属的组更改为newgroup

	3) chgrp：更改文件或目录的所属组  
		`chgrp newgroup /home/user/file.txt`
		表示将/home/user/file.txt的所属的组更改为newgroup

31. 网络操作命令:  



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
		`echo .DS_Store >> ~/.gitignore_global`
		2. 将这个全局的 .gitignore 文件加入Git的全局config文件中，执行命令：
		`git config --global core.excludesfile ~/.gitignore_global`
