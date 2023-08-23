---
title: git-stash
date: 2023-08-23 23:02:43
tags: [Git]
---

### Have this ever happen to you?

- 當你在某分支大展身手時，同事走過來問你問題，但他的問題只出現在另一支分支裡，這時候該怎麼辦呢?
- 你研究一個新功能到一半，需要臨時中斷處理別的事情。你想保留寫到一半的 code 但又不想要影響到既有的 code

### 你該試試 Git Stash!

git stash 是一個小暫存庫，能幫你把 code 收好，等你需要時再叫出來，不會影響目前分支的 commit

## 指令介紹

**`git stash save -u 註解`** - 將 untrack 和 unstage 檔案都存進 stash， -u 表示 untrack 也要加入。

**`git stash list`** - 查看目前 stash 有幾筆。stash 的資料結構為 **Stack**，所以會先進後出。

**`git stash pop 資料index`** - 將第幾筆 stash 取出，如果不指定 index，則 pop 最上面一筆。
EX: git stash pop 1。pop 後資料會從 stash stack 中剔除。

**`git stash apply 資料index`** - 也是取出 stash，但該筆資料仍會保留在 stash stack 中。

**`git stash drop 資料index`** - 刪除某筆 stash

**`git stash clear`** - 清除 stash
