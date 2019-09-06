<h1>Markdown Note</h1>
#### date:2019/9/6 上午9:09:46

+ Part0 : [前言](#first)
+ Part1 : [區塊元素](#second)
 1. [標題](#second_5)
 1. [區塊引言](#second_1)
 2. [清單](#second_2)
 3. [程式碼區塊](#second_3)
 4. [分隔線](#second_4)
+ Part2 : [區段元素](#third)
 1. [連結](#third_1)
 2. [強調](#third_2)
 3. [程式碼](#third_3)
 4. [跳脫字元](#third_4)
 5. [圖片](#third_5)
    - [行內](#third_5_1)
    - [參考](#third_5_2)

- - -

<h4 id="first">Part0. 前言</h4>

 + Markdown的語法有個主要的目的：用來作為一種網路內容的寫作用語言。
 + 在HTML文件中，有兩個字元需要特殊處理：<和&。
 + <符號用於起始標籤，&符號則用於標記HTML實體，如果你只是想要使用這些符號，你必須要使用實體的形式，像是`&lt;`和 `&amp;`。
 + 要在文件中插入一個著作權的符號，你可以這樣寫：`&copy;`。

<h4 id="second">Part1. 區塊元素</h4>

<h5 id="second_5">1. 標題</h5>

+ Markdown支援兩種標題的語法，[Setext](http://docutils.sourceforge.net/mirror/setext/)和[atx](http://www.aaronsw.com/2002/atx/)形式。
+ Setext形式是用底線的形式，利用=（最高階標題）和-（第二階標題）:

```
This is an H1
=====
This is an H2
-----
```

<h5 id="second_1">2. 區塊引言</h5>

+ Markdown使用email形式的區塊引言，每行的最前面加上>
+ 階層式 > 、 >> 、 >>>

<h5 id="second_2">3. 清單</h5>

+ Markdown支援有序清單和無序清單
- 有序

```
1. a
2. b
3. c
```

- 無序

```
    + a
    * b
    - c
```

- HTML :

```
    <ol>
      <li>a</li>
      <li>b</li>
      <li>c</li>
    </ol>
```

<h5 id="second_3">4. 程式碼區塊</h5>

+ 要在Markdown中建立程式碼區塊很簡單，只要簡單地縮排4個空白或是1個tab就可以

```
<pre><code>code block</code></pre>
```

+ 一個程式碼區塊會一直持續到沒有縮排的那一行（或是文件結尾）

<h5 id="second_4">5. 分隔線</h5>

```
* * *
***
*****
- - -
---------------------------------------
```

<h4 id="third">Part2. 區段元素</h4>
<h5 id="third_1">1. 連結</h5>

+ Markdown支援兩種形式的連結語法：行內和參考兩種形式。
  連結的文字都是用 [方括號] 來標記。

+ 要建立一個行內形式的連結，
  只要在方塊括號後面馬上接著括號並插入網址連結即可，如果你還想要加上連結的title文字，只要在網址後面，用雙引號把title文字包起來即可。
+ 如果你是要連結到同樣主機的資源，你可以使用相對路徑。
+ 參考形式的連結使用另外一個方括號接在連結文字的括號後面，而在第二個方括號裡面要填入用以辨識連結。
+ linkid不區分大小寫:

```
See me [link] [linkid] reference link.
[linkid]: <https://www.w3schools.com/js/> "Title"
```

<h5 id="third_2">2. 強調</h5>

+ Markdown使用星號( \* )和底線( \_ )作為標記強調字詞的符號，被 \* 或 \_ 包圍的字詞會被轉成用```<em>```標籤包圍，用兩個 \* 或 \_ 包起來的話，則會被轉成```<strong>```。

<h5 id="third_3">3. 程式碼</h5>

+ 如果要標記一小段行內程式碼，你可以用反引號把它包起來（`）

<h5 id="third_4">4. 圖片</h5>

+ Markdown使用一種和連結很相似的語法來標記圖片，同樣也允許兩種樣式：行內和參考。
- <span id="third_5_1">行內</span>

```
![Alt text](/path/to/img.jpg)
![Alt text](/path/to/img.jpg "Optional title")
```

- <span id="third_5_2">參考</span>

```
![Alt text][id]
[id]: url/to/image  "Optional title attribute"
```

<h5 id="third_5">5. 跳脫字元</h5>

+ Markdown可以利用反斜線來插入一些在語法中有其他意義的符號。

```
\   反斜線
`   反引號
*   星號
_   底線
{}  大括號
[]  方括號
()  括號
#   井字號
+   加號
-   減號
.   英文句點
!   驚嘆號
```
