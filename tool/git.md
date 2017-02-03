# Git 指令

記錄一些很常用但卻一直忘記的指令：

#### checkout

checkout remote branch

```
git checkout -b BRANCH origin/BRANCH
```

#### diff

```
git diff BRANCH > file.txt
```

#### commit message 

```
Fix #issueNo
Related to GROUP/PROJECT#issueNo
```

#### 修改 git config 

```
git config -e
```

#### 新增 tag, push tag

```
git tag -a v1.1.0 -m 'MESSAGE'
git push origin v1.1.0
```

#### cherry pick

先到目的地 BRANCH 在 pick 別的 BRANCH 的 HASH

```
git cherry-pick HASH
git cherry-pick HASH1..HASH2
```

