
git commit 了一个错误的文件想还原继续修改，这个提交点是0a86ff2b，错误使用了git reset，导致提交的错误文件都没了

```
# git reflog 查看提交历史

238462e9 (HEAD -> hexo-blog, origin/hexo-blog) HEAD@{0}: reset: moving to 238462e957f8136c61648efa564d843f425f4df4
0a86ff2b HEAD@{1}: reset: moving to 0a86ff2bda5f24bde1f0e6cc523ed5832d95bda6
0a86ff2b HEAD@{2}: commit: fix
238462e9 (HEAD -> hexo-blog, origin/hexo-blog) HEAD@{3}: commit: feat: coroutine
206eddf9 HEAD@{4}: commit: chore
88187047 HEAD@{5}: commit: chore: new friend link
```


`git reset --hard  0a86ff2b` 回到提交点

`git reset --soft HEAD^` 软reset，HEAD^ 表示上一个版本，即上一次的commit，也可以写成HEAD~1。如果进行两次的commit，想要都撤回，可以使用HEAD~2


	rgb(84 86 88 / 10%)
