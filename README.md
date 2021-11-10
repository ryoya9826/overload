 ## overload package

=================

本パッケージはLaTeXパッケージです．
次のウェブサイト[LaTeXでマクロの引数をオーバーロード](https://hak7a3.hatenablog.com/entry/2015/05/24/202509)に触発されて開発をしましたが，2021/08/01において元サイトではプログラムが公開されておらず，本パッケージのコードは私が単独で作成したため著作権上の問題は発生しないと考えていますが，問題があった場合は連絡していただければ公開停止も含め対応いたします．

### 開発履歴

#### v0.1, 2021/08/01

製作開始，公開．

#### v0.2 2021/08/02

`\renewoverload`追加．

#### v1.0 2021/11/10 

Preparation for uploading to CTAN.

### 使用方法
まず
```latex
\newoverload{hoge}
```
のようにしてコントロールシークエンス`\hoge` を登録してください．
すでに定義されているコントロールシークエンスを使いたい場合は`\renewoverload`を使用してください．
その後；
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
  
登録したコントロールシークエンスは引数の末尾に`\delimend` をつけて使用する必要があります．
 この状況下では；
 ```latex
 \hoge\delimend
 \hoge{foo}\delimend
 \hoge{foo}{bar}{\delimend}
  ```
  はそれぞれ；
  ```
  Nothing!
  First argument is foo!
  First argument is foo! Second argument is bar!
  ```
  に展開されます．
  
  `\delimend`は上記のような引数の終端を表す以外の使い方をした場合は無視されるだけですが， 制約として各引数の先頭に`\delimend`が来ることは禁止されています．
  どうしても引数の先頭に`\delimend`を配置したい場合は；
  ```latex
  \hoge{{\delimend} aaa}\delimend
  ```
  のようにしてください．
  
  また，各引数においてif系列のコマンドを使用する際には注意が必要です．
  
### ライセンス

MITライセンスの下で配布されます．

------------------------
Ryoya Ando
https://ryoya9826.github.io/

