---
title: "Will patch rename the filename?"
date: 2020-01-08T10:00:00+08:00
draft: false
---
幫朋友測試 patch 在使用 git diff 產出的 patch 檔案時，看會不會改名。
我測試的環境是 Ubuntu 18.04
先說結果，patch 是會為檔案改名的喔。

首先建立 bare repository
```
mkdir /tmp/foo.git
cd /tmp/foo.git && git init --bare
```

到另外一個目錄，簽出 repository，然後加一個檔案
```
cd $HOME
git clone /tmp/foo.git foo
cd foo
touch readme.md
git add readme.md
git commit -m "first initial"
```


再回到 HOME，簽出 repository，等等會回到這個目錄使用 patch 指令
```
cd $HOME
git clone /tmp/foo.git foo-new
```


回到 foo ，先建立 branch，更名，然後製作 patch 檔
```
cd $HOME/foo
git checkout -b file_is_renamed
git mv readme.md README.md
git commit -a -m "rename"
git diff master > changes.patch
cp changes.patch $HOME/foo-new
```

接著回到 foo-new，上 patch
```
cd $HOME/foo-new
patch -p1 < changes.patch
```

此時可以看到檔案已經由 readme.md 更名為 README.md 
