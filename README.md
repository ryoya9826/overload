 myoverload package

=================

本パッケージはLaTeXパッケージです．
次のウェブサイト[LaTeXでマクロの引数をオーバーロード](https://hak7a3.hatenablog.com/entry/2015/05/24/202509)に触発されて開発をしましたが，2021/08/01において元サイトではプログラムが公開されておらず，本パッケージのコードは私が単独で作成したため著作権上の問題は発生しないと考えていますが，問題があった場合は連絡していただければ公開停止も含め対応いたします．

### 開発履歴

#### v0.1, 2021/08/01

製作開始，公開．

### 使用方法
まず
```latex
\newoverload{hoge}
```
のようにしてコントロールシークエンス`\hoge` を登録してください．その後；
```latex
\addoverload{hoge}{0}{Nothing!}
\addoverload{hoge}{1}{First argument is #1!}
\addoverload{hoge}{2}{First argument is #1! Second argument is #2!}
```
のようにして`\hoge` の引数の個数ごとの挙動を定義します．構文は；
```latex
 \addoverload{<csname>}{<num>}{<tokens>}
```
で，\<num\>は0から9までの整数であることが想定されています．
  
登録したコントロールシークエンスは引数の末尾に`\enddelim` をつけて使用する必要があります．
 この状況下では；
 ```latex
 \hoge\enddelim
 \hoge{foo}\enddelim
 \hoge{foo}{bar}{\enddelim}
  ```
  はそれぞれ；
  ```
  Nothing!
  First argument is foo!
  First argument is foo! Second argument is bar!
  ```
  に展開されます．
  
### ライセンス

MITライセンスの下で配布されます．

------------------------
Ryoya Ando
https://ryoya9826.github.io/

