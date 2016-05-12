## Merge, Rebase

##### 问题

`feature/stats1.2.2` 和 `master` 相隔太远。
今天将 `feature/stats1.2.2` rebase `master`。过程中当然冲突不断。
后将调整好的 `feature/stats1.2.2` 合并到本地的 `staging`，`rebase` 远端分支，解决冲突。
再将 `staging` 推向远端，PR 无法直接合并。

##### 核心

`rebase` 操作会改变分支的提交结构。在这里 `feature/stats1.2.2` 的提交结构就基于 `master` 改变了。
但公共分支的提交结构就一定不能改。
这里虽然没有直接对 `staging` 进行 `rebase` 操作，但其之前就有 `feature/stats1.2.2` 的提交，如果将提交结构改变后的 `feature/stats1.2.2` 再合并，自身结构一定也会改变。（具体什么变化？）
所以如果这是将 `staging` 推向远端，整个结构都会变化，会影响所有开发者。

##### 处理

从远端 `staging` checkout 出新的分支 `merge/staging`，本地合并 `feature`，再推向远端。

这里好处在，不对 `staging` 进行 `rebase`，`merge` 时影响的 `commit` 都视作新添加的。（新 `commit` 很多，但文件改动少。）

同时，本地解决合并冲突，不留到线上。

##### 后续

C = A
A rebase B
C merge A

提交结构怎样？