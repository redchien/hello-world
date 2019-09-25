# Git
###### 2019/9/23 下午7:30:28
## Link
1. <a href="#userconfig">使用者設定</a>
2. <a href="#repository">新增、初始Repository</a>
3. <a href="#stagingArea">工作區、暫存區與儲存庫</a>
4. <a href="#branch">開始使用分支</a>
5. <a href="#history">修改歷史紀錄</a>
6. <a href="#tag">使用標籤</a>
7. <a href="#other">其它常見狀況題</a>
8. <a href="#github">遠端共同協作 - 使用 GitHub</a>

## <span id="userconfig">使用者設定</span>
```
git config --global user.name "Red Chien"
git config --global user.email "zxccc45697@gmail.com"
git config --list
```

##### [每個專案設定不同作者]
```
git config --local user.name "Red Chien"
git config --local user.email "zxccc45697@gmail.com"
git config --list
```

##### 設定縮寫
```
git config --global alias.co checkout
git config --global alias.br branch
git config --global alias.st status
git config --global alias.l "log --oneline --graph"
git config --global alias.ls 'log --graph --pretty=format:"%h <%an> %ar %s"'
```

## <span id="repository">新增、初始Repository</span>
```
cd /tmp
mkdir git-practice
cd git-practice
git init
```

+ ```git init```建立一個<strong>.git</strong>目錄。
+ ```git status```查詢現在目錄狀態。
+ ```git add```檔案交給<strong>Git</strong>，檔案會從```Untracked```變成```new file```，表示這個檔案被放置在暫存區。
+ ```git add --all```將檔案全部加入暫存區。

##### [git add後又修改檔案內容]
+ 需要再```git add```一次，重新加入暫存區。

##### [all跟.參數比較]
+ Git(1.x)
|使用參數|新增|修改|刪除|
|:--------:|:----:|:----:|:---:|
|```-all```|v|v|v|
|```.```|v|v|x|

+ Git(2.x)
|使用參數|新增|修改|刪除|
|:--------:|:----:|:----:|:---:|
|```-all```|v|v|v|
|```.```|v|v|v|

##### 把暫存區內容提交到倉庫裡存檔
+ ```git commit -m "init commit"```註解Commit。
+ ```git commit --allow-empty -m "empty commit"```允許沒東西Commit。

## <span id="stagingArea">工作區、暫存區與儲存庫</span>
+ 主要分為工作目錄(working directory)、暫存區(staging area)以及儲存庫(repository)，透過不同Git指令，可以把檔案移往不同區域。
+ ```git add```將檔案從工作目錄移至暫存區(索引)。
+ ```git commit```將檔案從暫存區移至儲存庫。

##### [狀況]
1. 某人或某些人的Commit訊息：</br>```git log --oneline --author='Red'```</br>```git log --oneline --author='Red\|Eddie'```
2. 找Commit訊息有某些字：</br>```git log --oneline --grep='WTF'```
3. 找Commit訊息有提到Ruby這個字：</br>```git log -S 'Ruby'```
4. 查詢時間區間Commit訊息：</br>```git log --oneline --since="9am" --until=12am --after="2019-01"```

##### [檔案刪除或變更檔名]
+ 刪除檔案
```
remove welcome.html
git status
git add welcome.html
```
```
git rm welcome.html
git status
```

+ 加上```--cached```參數：檔案不再被Git控管。
```
git rm welcome.html --cached
```

+ 變更檔名
```
mv hello.html world.html
git status
git add -all
```
```
git mv hello.html world.html
git status
```

##### [狀況]修改Commit紀錄
+ 使用```--amend```參數進行Commit，修改最後一次Commit訊息。
```
git log --oneline
git commit --amend -m "hello world"
git log --oneline
```

##### [狀況]追加檔案到最近一次的Commit
1. 使用```git reset```把最後一次的Commit拆掉，加入新檔案後再重新Commit。
2. 使用```amend```參數進行commit。
```
touch test.html
git status
git add test.html
git commit --amend --no-edit //不進行Commit的編輯
```

##### [狀況]新增目錄
+ 空目錄無法提交。
```
git status
mkdir images
git status //根據檔案內容，新增目錄，Git無法處理
```

##### [狀況]有些檔案不想放在Git
+ 設定忽略規則，在專案目錄放一個```.gitignore```檔案。
```
touch .gitignore
vim .gitignore
```

+ 清除忽略的檔案
```
git clean -fX
```

##### [狀況]檢視特定檔案的Commit紀錄
+ 檢視單一檔案在```git log```後面接上檔名。
+ 每次做了什麼修改```git log -p```。

##### [狀況]某行程式誰修改
+ 使用```git blame```。
+ 指定行數```git blame -L 5,10```，將第5~10行資訊顯示。

##### [狀況]不小心把檔案或目錄刪掉
+ 將目錄所以的檔案都刪掉，然後```git checkout```。
```
rm *.html
ls -al
git status
git checkout test.html //救回test.html
git checkout . //救回所有檔案
ls -al
```

##### [狀況]剛才的 Commit 後悔了，想要拆掉重做
+ ```git reset```。
```
git log --oneline
    // 372c1d5 (HEAD -> master) Welcome to test
    // 7f0ea9f mv practice
    // 779e130 create index page
    // ae7677d init commmit
git reset 372c1d5^ //拆掉最後一次Commit，最後一個^表示前一次，表示372c1d5這個Commit的前一次
git reset 779e130  //切換到779e130這個Commit的狀態
```

+ Reset模式
1. mixed模式：</br>```--mixed```是預設參數。這個模式會把暫存區檔案丟掉，但不會動到工作目錄的檔案。
2. soft模式：</br>工作目錄跟暫存區的檔案都不會被丟掉，只有```HEAD```移動而已，Commit拆出來的檔案會直接放入暫存區。
3. hard模式：</br>不管工作目錄跟暫存區的檔案都會丟掉。
4. v:不變、x:丟掉。
|模式|工作目錄|暫存區|
|:---:|:---:|:---:|
|mixed|v|x|
|soft|v|v|
|hard|x|x|
5. Reset觀念比較像是go to或become。

##### [狀況]不小心使用 hard 模式 Reset 了某個 Commit，救得回來嗎？
+ 使用```--hard```參數，hard參數可以強迫放棄Reset之後修改的檔案。
```
git log --oneline
    // 372c1d5 (HEAD -> master) Welcome to test
    // 7f0ea9f mv practice
    // 779e130 create index page
    // ae7677d init commmit
git reset HEAD~2
git log --oneline
    // 779e130 create index page
    // ae7677d init commmit
git reset 372c1d5 --hard
```

+ ```git reflog```。
```
git reset HEAD~2 --hard
git reflog
    //779e130 (HEAD -> master) HEAD@{0}: reset: moving to 779e130
    //372c1d5 HEAD@{1}: reset: moving to 372c1d5
    //779e130 (HEAD -> master) HEAD@{2}: reset: moving to HEAD~2
    //372c1d5 HEAD@{3}: commit (amend): Welcome to test
    //1283a7d HEAD@{4}: commit (amend): Welcome to test
    //49ef1ae HEAD@{5}: commit: WTF
    //7f0ea9f HEAD@{6}: commit: mv practice
    //779e130 (HEAD -> master) HEAD@{7}: commit: create index page
    //ae7677d HEAD@{8}: commit (initial): init commmit
git reset 372c1d5 --hard
git log --oneline
```

##### [冷知識]HEAD是什麼東西？
+ ```HEAD```是一個指標，指向某一個分支，可以把 HEAD 當做「目前所在分支」看待。
```
cat .git/HEAD
    //ref: refs/heads/master
```

##### [狀況]可以只Commit一個檔案的部份的內容嗎？
##### [冷知識]那個長得很像亂碼SHA-1是怎麼算出來的？

## <span id="branch">開始使用分支</span>
+ ```git branch```，印出目前在這個專案有哪些分支，前面有星號```*```表示現在正在這個分支上。

##### 新增分支
+ ```git branch cat```，建立一個分支，名稱為cat。

##### 分支修改名稱
+ ```git branch -m cat tiger```，將cat分支名稱改為tiger。

##### 刪除分支
+ ```git branch -d cat```，將cat分支刪除。
+ ```git branch -D cat```，將cat分支強制刪除。

##### 切換分支
+ ```git checkout cat```，切換到cat分支。
+ ```git checkout -b cat```，如沒有cat會創建分支，否則直接切換。

##### 合併分支
+ ```git merge cat```，合併cat分支Commit的部分。
+ ```git merge cat --no-ff```，不直接使用fast-forward合併，Git額外做出一個Commit物件合併Cat分支。

##### [狀況]不小心把還沒合併的分支砍掉了，救得回來嗎？
+ 分支只是指向某個Commit的指標。
```
git branch -d dog
    //error: The branch 'dog' is not fully merged.
    //If you are sure you want to delete it, run 'git branch -D dog'.
git branch -D dog
    //Deleted branch dog (was 67e0e7f).
git branch new_dog 67e0e7f
git branch
git checkout new_dog
ls -al
```

##### 另一種合併方式(rebase)
+ Rebase合併跟merge第一個明顯的差異在Git不會特別做出一個專門用來合併的Commit。
```
git checkout cat
git rebase dog
    //First, rewinding head to replay your work on top of it...
    //Applying: add cat 1
    //Applying: add cat 2
```
+ 在 Rebase 的過程中你會看到 2 次的 “Applying” 的字樣，就是在做「重新計算」的事情。

##### [狀況]取消rebase
+ 使用```reflog```。
+ 使用```git reset ORIG_HEAD --hard```，ORIG_HEAD紀錄危險操作之前的```HEAD```的位置。

##### [狀況]合併發生衝突
+ Rebase合併發生問題解決後，```git rebase --continue```。
+ 不是文字檔。
```
git checkout --ours cute.jpg
    //如要用對方分支則git checkout --theirs cute.jpg
git add cute.jpg
git commit -m "conflict fixed"
```

##### [冷知識]Git 怎麼知道現在是在哪一個分支？
+ .git目錄裡有個```HEAD```檔案紀錄現在的分支。
+ ```HEAD```的縮寫為```@```。

## <span id="history">修改歷史紀錄</span>
##### [狀況]修改歷史紀錄
+ rebase -i 編輯至ae7677d這個Commit訊息，會產生新的Commit物件。
```
git log --oneline
git rebase -i ae7677d
```

##### [狀況]把多個 Commit 合併成一個 Commit
+ ```squqsh```。
```
git rebase -i ae7677d
    //pick 8c683d7 create index page
    //pick 98cbd3e mv practice
    //pick cac829d Welcome to test
    //pick 14ce9ee add 2 cat
    //squash 955832a commit all
    //squash 5d0af38 commit images and gitignore
    //pick c2359d0 commit all
    //squash 9c4bea8 commit dog3 html
    //pick ef1ef0c d1 html
//9c4bea8 與 c2359d0 merge
//5d0af38 與 955832a 與 14ce9ee merge
```

##### [狀況]把一個 Commit 拆解成多個 Commit
+ ```edit```。
```
git log --oneline
    //146a968 (HEAD -> master) 2 new file
    //47f48c5 2 file
    //84b31d3 squash 2nd commit message
    //0aab536 squash all commit
    //cac829d Welcome to test
    //98cbd3e mv practice
    //8c683d7 create index page
    //ae7677d init commmit
git rebase -i ae7677d
    //pick 8c683d7 create index page
    //pick 98cbd3e mv practice
    //pick cac829d Welcome to test
    //pick 0aab536 squash all commit
    //pick 84b31d3 squash 2nd commit message
    //pick 63b9c0d add file1
    //pick 30792af add file2
    //edit 7393064 2 new file
// 7393064 HEAD停留在這個Commit
git reset HEAD^
// 後續將分別git add及git commit
...
git rebase --continue
```

##### [狀況]想要在某些 Commit 之間再加新的 Commit

##### [狀況]想要刪除某幾個 Commit 或是調整 Commit 的順序
+ 調整Commit順序：```git rebase -i ae7677d```，將順序調整。
+ 刪除Commit：```git rebase -i ae7677d```，pick改成drop或甚至直接把那行刪掉。

##### [狀況]Reset、Revert 跟 Rebase 指令有什麼差別？
+ 使用```git revert HEAD --no-edit```指令，再新做一個Commit來取消你不要的Commit，所以Commit數量才會增加。
+ 取消Revert：</br>1. ```git revert HEAD --no-edit```</br>2. ```git reset HEAD^ --hard```
|指令|改變歷史紀錄|說明|
|:---:|:---:|:---:|
|Reset|v|把目前的狀態設定成某個指定的 Commit 的狀態，通常適用於尚未推出去的 Commit。|
|Rebase|v|不管是新增、修改、刪除 Commit 都相當方便，用來整理、編輯還沒有推出去的 Commit 相當方便，但通常也只適用於尚未推出去的 Commit。|
|Revert|x|新增一個 Commit 來反轉（或說取消）另一個 Commit 的內容，原本的 Commit 依舊還是會保留在歷史紀錄中。雖然會因此而增加 Commit 數，但通常比較適用於已經推出去的 Commit，或是不允許使用 Reset 或 Rebase 之修改歷史紀錄的指令的場合。|

## <span id="tag">使用標籤</span>
+ 標籤```tag```是一個指向某一個Commit的指標，分為輕量標籤(lightweight tag)、附註標籤(annotated tag)。
+ 輕量標籤(lightweight tag)：僅是指向某個Commit的指標。
```
git tag big_cats 30792af
```
+ 附註標籤(annotated tag)：
```
git tag big_cats 30792af -a -m "Big cats are comming"
// -a：Git建立有附註的標籤
// -m：做一般Commit一樣輸入的訊息
```
+ 輕量標籤指向某一個Commit，但附註標籤指向某個Tag物件，而這個Tag物件才再指向那個Commit。
+ 刪除標籤：```git tag -d big_cats```。

## <span id="other">其它常見狀況題</span>
##### [狀況]手邊的工作做到一半，臨時要切換到別的任務
+ 使用 Stash
```
git status
git stash
git status
git stash list
```

+ 把Stash撿回來用
```
git stash list
git stash pop stash@{2}
```

+ Stash確定不要
```
git stash drop stash@{0}
```

+ 另一種方式，撿回來
```
git stash apply stash@{0}
```

##### [狀況]不小心把帳號密碼放在 Git 裡了，想把它刪掉…
+ 使用filter-branch指令，大量修改Commit。
```
git filter-branch --tree-filter "rm -f config/database.yml"
```
+ 回復filter-branch結果。
```
git reset refs/original/refs/heads/master --hard
```

##### [狀況]如果你只想要某個分支的某幾個 Commit？
+ ```cherry-pick```，可以只撿某些Commit來用。
```
git cherry-pick 84b31d3 --no-commit
// --no-commit：撿過來不先合併
```

##### [冷知識]斷頭（detached HEAD）是怎麼一回事？
+ 發生原因：</br>1. 使用```checkout```指令直接跳到某個 Commit，而那個 Commit 剛好目前沒有分支指著它。</br>2. ```rebase```的過程其實也是處於不斷的 detached HEAD 狀態。</br>3. 切換到某個遠端分支的時候。

## <span id="github">遠端共同協作 - 使用 GitHub</span>
+ 使用GitHub：</br>1. ```git remote```：遠端有關操作。</br>2. ```add```：加入一個遠端節點。</br>3. ```origin```：代名詞，指後面GitHub伺服器位置。
+ ```push```指令：</br>1. 把```master```分支內容推向```origin```這個位置。</br>2. 把```origin```那個遠端Server上，如果```master```不在，建立一個叫做```master```的同名分支。</br>3. 如果本來Server上就存在，便會移動Server上的```master```分支的位置，指到目前最新進度上。</br>4. 設定upstream，就是```-u```參數做的好事。
```
echo "# Practicing Git" > README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/redchien/testRepo.git
git push -u origin master
//git push dragonball cat
//遠端節點 dragonball 本地分支 cat
```

##### [冷知識]設定upstream
+ 每個分支可以設定一個upstream，它會指向並追蹤(track)某個分支。
+ ```git push -u origin master```：將```origin/master```設定為本地```master```分支的upstream，當下回執行```git push```而不加任何參數，Git將會推往```origin```這個遠端節點，並將```master```分支推上去。

##### [冷知識]不想同名的分支名稱
+ ```git push origin master:cat```：這樣當本地端的```master```分支推上去之後，就不會在線上建立```master```分支，而是建立一個叫做```cat```的分支。

##### Pull下載更新
+ ```git pull=git fetch + git merge```
+ ```git pull --rebase```

##### [狀況]怎麼有時候推不上去...
+ 解決辦法：</br>1. 先拉再推：```git pull --rebase```。</br>2. 無視規則：```git push -f```。

##### 從伺服器上取得 Repository
+ 整個專案複製一份並存在同名的目錄裡，想要Clone下來之後存成不同的目錄名稱，後面加上目錄名稱
```
git clone https://github.com/redchien/testRepo.git testrepo
```

##### [常見問題]Clone 跟 Pull 指令比較
+ ```git clone```指令只會使用第一次。
+ ```git pull```為更新最新的線上版內容。

##### 與其它開發者的互動 - 使用 Pull Request（PR）
+ GitHub上的機制：</br>1. 先```fork```一份原作專案到你自己GitHub帳號底下。</br>2. 在自己的GitHub帳號下，有完整權限。</br>3. 改完後，先推回自己帳號的專案。</br>4. 發個通知，讓原作者知道(Pull Request)。</br>5. 原作者覺得可以，決定把你做的這些修改合併至他的專案裡。
