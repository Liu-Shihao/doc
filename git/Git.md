# Git



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