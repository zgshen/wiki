
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

### git commit -a

加了-a，在 commit 的时候，能帮你省一步 git add，但也只是对修改和删除文件有效，新文件还是要 git add，不然就是 untracked 状态。


### 查看修改和放弃修改

#### 查看修改
```bash
git status
```


#### 取消 仓库所有 修改、删除
```bash
git checkout -f
```
此时你修改的文件和删除的文件都会被恢复，但是你**_新添加的文件不会被删除_**

#### 放弃 指定文件 修改、删除
```bash
git checkout filename
```
#### 放弃 指定文件夹 修改、删除
```bash
git checkout directory
```
此时指定目录下修改的文件和删除的文件都会被恢复，但是你**_新添加的文件不会被删除_**

#### 放弃 仓库所有 添加
```bash
git clean –df
```
此时该仓库下所有新添加文件将被清除, 不会对_**修改**_和_**删除**_做任何处理

#### 放弃 指定文件 添加
```bash
git clean filename –df
```
此时该新添加文件将被清除, 不会对_**修改**_和_**删除**_做任何处理

#### 放弃 指定文件夹 添加
```bash
git clean directory –df
```
此时该目录新添加文件将被清除, 不会对_**修改**_和_**删除**_做任何处理

#### git clean参数
首先我们需要认清 忽略的文件 和 未被跟踪的文件。

-   忽略的文件：.gitignore 中忽略的文件；
-   未被跟踪的文件：没有被忽略，但是还没 git add 的文件

```bash
git clean  -f             # 删除：未被跟踪的文件
git clean –fd             # 删除：未被跟踪的文件和文件夹
git clean –xfd            # 删除：忽略的文件、未被跟踪的文件和文件夹
git clean [-xfd] -n-n     # 会先打印一些将要删除的文件，并不执行删除动作，主要是查看是否有自己需要的不想被删除
```
