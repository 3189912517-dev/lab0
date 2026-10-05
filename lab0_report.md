# Lab0 GitLab 实验报告

## 一、文档问题回答

### 1. 之前是否有多人协同开发经历？

之前主要以课程作业和小规模项目为主，协作方式通常是先划分模块，再分别完成代码、文档或测试部分。早期如果只靠直接传文件或在聊天工具中同步代码，容易出现“谁的版本是最新的”这类问题；使用 Git 后，可以通过分支、提交记录和合并操作明确每个人做了什么，也更容易定位问题。

### 2. Git 为什么要设计“暂存-提交”两个步骤？

暂存区让提交变成一个可以被整理的过程，而不是把工作区所有变化一次性塞进历史记录。这样可以先用 `git add` 选择本次真正相关的修改，把调试代码、临时笔记或下一步实验内容留在工作区，再用 `git commit` 生成一个主题清晰的版本。暂存区也方便在提交前用 `git diff --cached` 检查将要进入历史的内容，从而让提交粒度更小、记录更可读。

### 3. `git branch` 和 `git branch -a` 的区别

`git branch` 默认只显示本地分支，例如本实验中的 `main` 和 `feature`。`git branch -a` 会显示全部分支，包括本地分支和远程跟踪分支，例如 `remotes/origin/main`。因此，当需要确认远程仓库上有哪些分支、或者本地是否已经跟踪到远程分支时，可以使用 `git branch -a`。

## 二、拓展阅读总结

### 1. Commit Message 规范

阅读链接：[Commit message 和 Change log 编写指南](https://www.ruanyifeng.com/blog/2016/01/commit_message_change_log.html)

这篇文章介绍了规范提交信息的意义：清晰的 commit message 能帮助开发者快速浏览历史、按类型筛选变更，也可以作为自动生成 Change log 的基础。文章重点介绍了类似 Angular 的提交格式，即用 `type(scope): subject` 表达提交类别、影响范围和简短说明，例如 `feat` 表示新增功能、`fix` 表示修复问题、`docs` 表示文档修改。本实验中的提交也尽量采用了类似风格，如 `feat: complete main.c TODO` 和 `merge: resolve feature branch conflict`。

### 2. 语义化版本

阅读链接：[语义化版本 2.0.0](https://semver.org/lang/zh-CN/)

语义化版本强调用 `主版本号.次版本号.修订号` 描述软件变化。修订号通常对应向后兼容的问题修复，次版本号对应向后兼容的新功能，主版本号则表示不兼容的 API 变化。这个规范把版本号变成一种沟通方式，让使用者看到版本变化时就能大致判断升级风险。

### 3. 为什么要学习 Git

Git 不只是“保存代码”的工具，更像是项目历史的数据库。它能记录每次修改的原因和范围，帮助自己在出错时回退，也帮助团队在并行开发时通过分支隔离不同任务。学习 Git 之后，代码修改不再只是文件覆盖，而是可以被解释、审查、合并和追踪的工程过程。

## 三、实验步骤

### 1. 克隆模板仓库

根据实验要求，使用课程模板仓库 `https://github.com/ICS-26Fall-FDU/GitLab` 建立本地仓库。本次在当前目录执行：

```bash
git clone https://github.com/ICS-26Fall-FDU/GitLab.git .
```

初始提交历史为：

```text
f67e080 feat(): add autograding; doc(): add README.md
5b71eff Initial commit
```

### 2. 完成 `main.c` 中的 TODO 并提交

将原来的输出修改为：

```c
printf("Learning Git step by step in ICS Lab0.\n");
```

提交记录：

```text
eb1132d feat: complete main.c TODO
```

### 3. 创建 `feature` 分支并提交修改

创建并切换到 `feature` 分支：

```bash
git checkout -b feature
```

在 `feature` 分支中修改同一行输出为：

```c
printf("Feature branch says: Git branches make experiments safe.\n");
```

提交记录：

```text
8eb4a1d feat: update message on feature branch
```

### 4. 回到 `main` 分支并提交另一处修改

切回 `main` 分支：

```bash
git checkout main
```

在 `main` 分支中修改同一行输出为：

```c
printf("Main branch says: Git history records every careful step.\n");
```

提交记录：

```text
687ed35 feat: update message on main branch
```

### 5. 合并 `feature` 并解决冲突

在 `main` 分支执行：

```bash
git merge feature
```

出现的冲突记录如下：

```text
Auto-merging main.c
CONFLICT (content): Merge conflict in main.c
Automatic merge failed; fix conflicts and then commit the result.
```

冲突发生后，`git status --short --branch` 显示：

```text
## main...origin/main [ahead 2]
UU main.c
```

冲突文件中的标记如下：

```c
<<<<<<< HEAD
    printf("Main branch says: Git history records every careful step.\n");
=======
    printf("Feature branch says: Git branches make experiments safe.\n");
>>>>>>> feature
```

解决冲突时，我保留了两个分支的含义，最终 `main.c` 中的输出为：

```c
printf("Main branch says: Git history records every careful step.\n");
printf("Feature branch says: Git branches make experiments safe.\n");
```

随后执行：

```bash
git add main.c
git commit -m "merge: resolve feature branch conflict"
```

合并提交记录：

```text
8f84362 merge: resolve feature branch conflict
```

## 四、最终提交历史

当前仓库的提交历史如下：

```text
*   8f84362 (HEAD -> main) merge: resolve feature branch conflict
|\
| * 8eb4a1d (feature) feat: update message on feature branch
* | 687ed35 feat: update message on main branch
|/
* eb1132d feat: complete main.c TODO
* f67e080 (origin/main, origin/HEAD) feat(): add autograding; doc(): add README.md
* 5b71eff Initial commit
```

## 五、建议

第一次做 Git 实验时，可以先把每个命令对应的状态变化写下来，例如 `git status`、`git log --oneline --graph --all` 的输出。这样能更直观地理解工作区、暂存区、提交和分支之间的关系，也能在发生冲突时更快判断应该修改哪个文件。
