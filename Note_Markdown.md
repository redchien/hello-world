<h1>:tw-1f525:Markdown Note:tw-1f525:</h1>
#### date:2019/09/05
+ **Part0 : [�e��](#first)**
+ **Part1 : [�϶�����](#second)**
 1. [���D](#second_5)
 1. [�϶��ި�](#second_1)
 2. [�M��](#second_2)
 3. [�{���X�϶�](#second_3)
 4. [���j�u](#second_4)
+ **Part2 : [�Ϭq����](#third)**
 1. [�s��](#third_1)
 2. [�j��](#third_2)
 3. [�{���X](#third_3)
 4. [����r��](#third_4)
 5. [�Ϥ�](#third_5)
   1.1. [�椺](#third_5_1)
   1.2. [�Ѧ�](#third_5_2)
- - -
<h4 id="first">Part0. �e��</h4>

 + Markdown���y�k���ӥD�n���ت��G�Ψӧ@���@�غ������e���g�@�λy���C
 + �bHTML��󤤡A����Ӧr���ݭn�S��B�z�G<�M&�C
 + <�Ÿ��Ω�_�l���ҡA&�Ÿ��h�Ω�аOHTML����A�p�G�A�u�O�Q�n�ϥγo�ǲŸ��A�A�����n�ϥι��骺�Φ��A���O`&lt;`�M `&amp;`�C
 + �n�b��󤤴��J�@�ӵۧ@�v���Ÿ��A�A�i�H�o�˼g�G`&copy;`�C

<h4 id="second">Part1. �϶�����</h4>

<h5 id="second_5">1. ���D</h5>

+ Markdown�䴩��ؼ��D���y�k�A[Setext](http://docutils.sourceforge.net/mirror/setext/)�M[atx](http://www.aaronsw.com/2002/atx/)�Φ��C
+ Setext�Φ��O�Ω��u���Φ��A�Q��=�]�̰������D�^�M-�]�ĤG�����D�^:

```
This is an H1
=====
This is an H2
-----
```

<h5 id="second_1">2. �϶��ި�</h5>

+ Markdown�ϥ�email�Φ����϶��ި��A�C�檺�̫e���[�W>
+ ���h�� > �B >> �B >>>

<h5 id="second_2">3. �M��</h5>

+ Markdown�䴩���ǲM��M�L�ǲM��
- ����

```
1. a
2. b
3. c
```

- �L��

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

<h5 id="second_3">4. �{���X�϶�</h5>

+ �n�bMarkdown���إߵ{���X�϶���²��A�u�n²��a�Y��4�ӪťթάO1��tab�N�i�H

```
<pre><code>code block</code></pre>
```

+ �@�ӵ{���X�϶��|�@�������S���Y�ƪ����@��]�άO��󵲧��^

<h5 id="second_4">5. ���j�u</h5>

```
* * *
***
*****
- - -
---------------------------------------
```

<h4 id="third">Part2. �Ϭq����</h4>
<h5 id="third_1">1. �s��</h5>

+ Markdown�䴩��اΦ����s���y�k�G�椺�M�ѦҨ�اΦ��C
  �s������r���O�� [��A��] �ӼаO�C

+ �n�إߤ@�Ӧ椺�Φ����s���A
  �u�n�b����A���᭱���W���۬A���ô��J���}�s���Y�i�A�p�G�A�ٷQ�n�[�W�s����title��r�A�u�n�b���}�᭱�A�����޸���title��r�]�_�ӧY�i�C
+ �p�G�A�O�n�s����P�˥D�����귽�A�A�i�H�ϥά۹���|�C
+ �ѦҧΦ����s���ϥΥt�~�@�Ӥ�A�����b�s����r���A���᭱�A�Ӧb�ĤG�Ӥ�A���̭��n��J�ΥH���ѳs���C
+ linkid���Ϥ��j�p�g:

```
See me [link] [linkid] reference link.
[linkid]: <https://www.w3schools.com/js/> "Title"
```

<h5 id="third_2">2. �j��</h5>
+ Markdown�ϥάP��( \* )�M���u( \_ )�@���аO�j�զr�����Ÿ��A�Q \* �� \_ �]�򪺦r���|�Q�ন��```<em>```���ҥ]��A�Ψ�� \* �� \_ �]�_�Ӫ��ܡA�h�|�Q�ন```<strong>```�C

<h5 id="third_3">3. �{���X</h5>
+ �p�G�n�аO�@�p�q�椺�{���X�A�A�i�H�ΤϤ޸��⥦�]�_�ӡ]`�^

<h5 id="third_4">4. �Ϥ�</h5>
+ Markdown�ϥΤ@�ةM�s���ܬۦ����y�k�ӼаO�Ϥ��A�P�ˤ]���\��ؼ˦��G�椺�M�ѦҡC
- �椺

```
![Alt text](/path/to/img.jpg)
![Alt text](/path/to/img.jpg "Optional title")
```

- �Ѧ�

```
![Alt text][id]
[id]: url/to/image  "Optional title attribute"
```

<h5 id="third_5">5. ����r��</h5>
+ Markdown�i�H�Q�Τϱ׽u�Ӵ��J�@�Ǧb�y�k������L�N�q���Ÿ��C

```
\   �ϱ׽u
`   �Ϥ޸�
*   �P��
_   ���u
{}  �j�A��
[]  ��A��
()  �A��
#   ���r��
+   �[��
-   �
.   �^��y�I
!   ��ĸ�
```