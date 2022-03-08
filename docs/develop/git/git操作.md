
# Git 操作备忘

### 查看提交历史

```bash
git log --pretty=format:"%h - %an, %ar : %s" -5
```

git 的默认编辑器是 nano，少用不习惯的话可改为 vi

```bash
git config core.editor vim
```

### 修改提交历史用户

```bash
git rebase -i HEAD~9   #最新的 9 个提交
```

将提交前面的  pick 改为 edit，保存退出，会有类似提示，如果是合并的提交就不是，可选择跳过

```bash
停止在 78611caa... fix
您现在可以修补这个提交，使用

  git commit --amend 

当您对变更感到满意，执行

  git rebase --continue
```

使用以下命令来修改提交用户

```bash
git commit --amend --author="zgshen <zguishen@foxmail.com>"
```

continue 继续修改直到结束

