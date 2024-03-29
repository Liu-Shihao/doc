# Git
设置远程分支地址
```shell
git remote set-url origin https://github.com/your-username/your-repository.git
git remote -v
```



从远程仓库获取最新的提交和对象
```shell
git fetch <远程仓库名称>
git fetch origin
git fetch -v origin master
git fetch -v --all
```


```shell

git pull <远程仓库名称> <远程分支名称>
git pull origin master

git push <远程仓库名称> <本地分支名称>:<远程分支名称>
git push origin feature-branch:feature-branch


git push -u origin main


```
在使用 git push 时，如果您是第一次推送本地分支到远程仓库，可以使用 -u 选项设置跟踪关系，使得将来的 git pull 和 git push 可以自动关联本地分支与远程分支。



将暂存区的更改提交到本地仓库
```shell
git commit -m "提交消息"
git commit -m "Add new feature"

```

修改commit msg

如果您需要修改上一次提交的提交消息，可以使用 `git commit --amend` 命令。以下是修改上一次提交消息的步骤：

1. **打开文本编辑器：**
   ```bash
   git commit --amend
   ```

这会打开默认文本编辑器，显示上一次提交的提交消息。您可以在编辑器中修改提交消息。

2. **修改提交消息：**
   在文本编辑器中，修改您希望更改的提交消息。保存并关闭编辑器。

   例如，在 Vim 编辑器中，按 `Esc` 键，输入 `:wq`，然后按 `Enter` 保存并退出。

   如果只是要修改提交消息，不修改任何文件，可以省略 `-m` 参数。

3. **保存修改：**
   保存并关闭编辑器后，Git 将更新上一次提交的提交消息。

如果您需要修改的不仅是提交消息，还包括已暂存的文件或提交的更改，可以在执行 `git commit --amend` 之前先进行 `git add`，确保暂存了所有要包含在修改中的更改。

请注意，如果上一次的提交已经推送到远程仓库，修改提交消息后需要使用 `git push --force` 推送更改。谨慎使用 `--force`，因为它会覆盖远程仓库上的历史记录。




要查看 Git 仓库中的提交消息（commit messages），您可以使用 `git log` 命令。以下是一些 `git log` 命令的常见用法：

1. **查看所有提交消息：**
   ```bash
   git log
   ```

   这将显示所有提交消息，从最新的到最旧的顺序。

2. **查看最近的 N 条提交消息：**
   ```bash
   git log -n N
   ```

   其中，N 是您希望查看的提交消息数量。

3. **查看某个分支的提交消息：**
   ```bash
   git log <branch_name>
   ```

   替换 `<branch_name>` 为您要查看的分支的名称。

4. **查看某个文件的提交历史：**
   ```bash
   git log -- <file_path>
   ```

   替换 `<file_path>` 为您要查看的文件的路径。

5. **以一行的形式查看提交消息：**
   ```bash
   git log --oneline
   ```

   这将以一行的形式显示每个提交的简短哈希和提交消息。

6. **以图形化形式查看提交历史：**
   ```bash
   git log --graph --oneline --all
   ```

   这将以图形化的形式显示提交历史，包括分支和合并情况。

7. **查看某个作者的提交历史：**
   ```bash
   git log --author=<author_name>
   ```

   替换 `<author_name>` 为您要查看的作者的名称。

上述命令执行后，您将看到提交历史的详细信息，包括提交的哈希值、作者、提交日期和提交消息。通过查看提交历史，您可以了解项目的演变过程以及每个提交的详细信息。

```shell

git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:Liu-Shihao/springboot-all.git
git push -u origin main


git remote add origin git@github.com:Liu-Shihao/springboot-all.git
git branch -M main
git push -u origin main
```
# refusing to merge unrelated histories
"refusing to merge unrelated histories" 是 Git 报出的错误信息，通常在尝试合并两个看起来没有共同历史的分支时出现。这通常发生在以下情况下：

1. 你可能有两个不相关的仓库（repository）或分支，它们之间没有共同的提交历史。
2. 你可能尝试将一个新的仓库合并到已有的仓库中，但它们之间没有任何共同的提交历史。

要解决这个问题，你可以使用 `--allow-unrelated-histories` 选项来强制合并不相关的历史。例如：

```
git pull origin master --allow-unrelated-histories
git pull origin main --allow-unrelated-histories
```

这将允许合并两个没有共同历史的分支或仓库。请注意，这可能会导致一些冲突，因为 Git 不知道如何将它们合并在一起，所以你需要手动解决任何冲突。

如果你确定你要合并的两个历史确实没有共同点，那么使用 `--allow-unrelated-histories` 是一个有效的解决方法。但在合并之前，请确保你理解潜在的冲突和后果。如果可能的话，最好与其他团队成员或贡献者讨论，以确保合并的合理性。

# fatal: couldn't find remote ref master
"fatal: couldn't find remote ref master" 错误通常表示在你的 Git 仓库中没有与名为 "master" 的远程分支相关联。这可能是因为你的远程仓库没有名为 "master" 的分支，或者你的本地仓库没有正确设置远程仓库的 URL。

要解决这个问题，你可以执行以下步骤：

1. 检查远程分支的存在：确保远程仓库中存在名为 "master" 的分支。你可以使用以下命令查看远程分支：

   ```
   git ls-remote <remote_name>
   ```

   替换 `<remote_name>` 为你的远程仓库的名称，通常是 "origin"。如果 "master" 分支不存在，你可能需要查看其他可用的分支并使用相应的分支名称。

2. 更新远程分支信息：有时，远程仓库的分支信息可能不会被及时同步到本地。你可以使用以下命令来获取远程仓库的最新信息：

   ```
   git remote update
   ```

3. 检查远程仓库 URL：确保你的本地仓库正确配置了远程仓库的 URL。你可以使用以下命令查看远程仓库的 URL：

   ```
   git remote -v
   ```

   如果远程仓库的 URL 不正确，可以使用 `git remote set-url` 命令来更新它：

   ```
   git remote set-url <remote_name> <new_url>
   ```

   替换 `<remote_name>` 为远程仓库的名称，`<new_url>` 为正确的仓库 URL。

确保你的本地仓库和远程仓库配置正确，远程分支存在，并且你使用正确的分支名称，这样你应该能够避免 "fatal: couldn't find remote ref master" 错误。

# fatal: unable to access 'https://github.com/Liu-Shihao/doc.git/': Failure when receiving data from the peer

"fatal: unable to access 'https://github.com/Liu-Shihao/doc.git/': Failure when receiving data from the peer" 错误通常是由于与远程仓库通信时出现问题而引起的。这个问题可能由多种原因引起，包括网络问题、认证问题、或者 GitHub 服务器问题。下面是一些可能的解决方法：

1. **检查网络连接**：首先确保你的网络连接正常。有时候网络问题可能导致无法与远程仓库通信。尝试访问其他网站，确认你的网络连接没有问题。

2. **验证 GitHub 访问权限**：如果你正在访问一个私有仓库，确保你在本地已经正确配置了 GitHub 的访问权限。你可以使用 SSH 密钥或者用户名密码进行认证。检查你的认证信息是否正确，并且你有权限访问该仓库。

3. **尝试使用 SSH**：如果你之前使用 HTTPS URL 访问远程仓库，并遇到问题，可以尝试切换到 SSH URL。首先，添加你的 SSH 公钥到你的 GitHub 帐户，然后将仓库 URL 更改为 SSH 格式。使用 SSH URL 时，不需要输入用户名和密码。

4. **清除 Git 缓存**：有时 Git 缓存可能引起问题。你可以尝试清除 Git 的凭据缓存，然后再次尝试操作。运行以下命令：

   ```
   git credential reject
   git credential reject https://github.com
   ```

   这会清除 Git 缓存中的凭据信息，然后 Git 会提示你输入用户名和密码或 SSH 密钥。

5. **尝试不同时间或地点**：有时 GitHub 服务器可能出现暂时性问题，尝试在不同的时间或地点进行操作可能有帮助。

6. **联系 GitHub 支持**：如果你尝试了以上方法仍然无法解决问题，可以联系 GitHub 支持寻求进一步的帮助。

如果你的问题仍然存在，请提供更多细节，例如你的操作步骤和 Git 配置，以便提供更具体的建议。


# 合并有冲突的没有共同历史记录的分支
```shell
❯ git pull origin main --allow-unrelated-histories

From https://github.com/Liu-Shihao/doc
 * branch            main       -> FETCH_HEAD
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint: 
hint:   git config pull.rebase false  # merge
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint: 
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
(base) 

```



这个错误信息表明你的分支之间存在分歧（divergent branches），需要明确指定如何协调它们。你可以按照错误提示中提到的方法之一来解决这个问题。

1. **使用 `--rebase` 进行变基合并**：要使用变基合并（rebase），可以运行以下命令：

   ```bash
   git pull origin main --rebase
   ```

   这将将你的本地分支上的提交放在远程分支的顶部，看起来就像它们一直在同一条历史线上。

2. **使用 `--no-rebase` 进行传统合并**：要执行传统合并（merge），可以运行以下命令：

   ```bash
   git pull origin main --no-rebase
   ```

   这将创建一个新的合并提交来整合分歧的历史。

3. **使用 `--ff-only` 进行快进合并**：如果你确定可以进行快进合并，可以运行以下命令：

   ```bash
   git pull origin main --ff-only
   ```

   这将只执行快进合并，不会创建新的合并提交。

选择其中一个选项，根据你的需求来合并分支。如果你想设置一个默认选项，你可以使用以下命令：

- 设置默认为变基合并：

  ```bash
  git config pull.rebase true
  ```

- 设置默认为传统合并：

  ```bash
  git config pull.rebase false
  ```

- 设置默认为快进合并：

  ```bash
  git config pull.ff only
  ```

这样你在未来的 pull 操作中将会使用默认的合并方式。如果你希望设置全局默认值，可以在上述命令中加上 `--global` 选项。

在选择合并方式时，要根据你的项目需求和工作流程来决定哪种方式最适合。