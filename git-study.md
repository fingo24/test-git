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

---

## 几个常用的git语句

1. 设置姓名
	`git config --global user.name ""`

2. 设置邮箱
	`git config --global user.email "< your_email@email.com>"`

3. git fetch
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

4. 修正远程仓库关联
	```bash
	git remote remove origin
	git remote add origin <正确的远程仓库URL>
	```

5. git diff
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

  1. printf命令比echo更加强大，支持格式化输出。
   `printf "这是一些文本内容\n" > filename.txt`
   
  2. 使用rm命令
   
   `rm filename.txt`
   `rm *.txt`
   
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
			解析:递归搜索 patches/ 目录下的所有 .patch 文件；对每个找到的 .patch 文件执行 patch 命令({} 是占位符，会被替换为当前找到的文件路径（例如 patches/fix-bug1.patch）；`\;` 表示命令结束)

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
