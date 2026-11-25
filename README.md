# overload

Macros overloaded on the number of arguments for LaTeX2e (`overload.sty`).

- Author / Maintainer: Ryoya Ando — <https://ryoya9826.github.io/>
- Version: 2.0 (2026-11-25)
- License: MIT License
- Repository: <https://github.com/ryoya9826/overload>

## Description (English)

`overload.sty` lets you register a separate body for each number of
arguments under one command name:

```latex
\newoverload{\hoge}
\addoverload{\hoge}{0}{(no argument)}
\addoverload{\hoge}{1}{one: #1}
\addoverload{\hoge}{2}{one: #1, two: #2}

\hoge  \hoge{A}  \hoge{A}{B}
```

The command counts the brace groups that follow it by lookahead, so no
terminator is needed. Counting stops at the first token that is not `{`
(a space included) or when the largest registered number is reached.
The arguments are passed to the selected body as token lists, so nested
calls such as `\hoge{\hoge{a}{b}}{c}` work as expected. Calling a command
with a number of arguments that has no body raises an error.

The commands are defined with `\protected`, so they survive in section
titles and captions. They are not expandable and cannot be used where only
expansion takes place (inside `\edef`, as a number or a length, or in PDF
bookmarks).

Up to version 1.0 the package was called `myoverload`, and `\enddelim`
was required at the end of each call. Documents written for the old
version should load `overload` instead. `\enddelim` is still accepted as
an optional terminator.

Requirements: any LaTeX2e format with e-TeX extensions. Tested with pdfLaTeX,
LuaLaTeX, XeLaTeX, pLaTeX and upLaTeX. No other packages are required.
The documentation (`overload-doc.pdf`) is written in Japanese.

### Files

| File | Description |
| --- | --- |
| `overload.sty` | The package |
| `overload-doc.tex`, `overload-doc.pdf` | Documentation (Japanese) and its source |
| `test/overload-test.tex` | Self-checking test document |
| `README.md` | This file |
| `LICENSE` | License text |

To rebuild the documentation:

```
lualatex overload-doc
lualatex overload-doc
lualatex overload-doc
```

To run the tests (the last run prints `ALL TESTS PASSED`):

```
pdflatex test/overload-test
pdflatex test/overload-test
```

### License

This work is released under the MIT License. See the file `LICENSE`.

---

# overload パッケージ（日本語）

Author: Ryoya Ando — [https://ryoya9826.github.io/](https://ryoya9826.github.io/)  
License: [MIT License](LICENSE)  

`overload.sty` は，引数の個数に応じて本体が切り替わるコマンドを定義するためのパッケージです．
1つのコマンド名に，引数が0個のとき，1個のとき，2個のときの本体を別々に登録できます．
呼び出す側は波括弧の引数を並べるだけで，個数に対応する本体が選ばれます．

## 着想

- 記事: [LaTeXでマクロの引数をオーバーロード](https://hak7a3.hatenablog.com/entry/2015/05/24/202509)

2021年8月時点で記事に実装は公開されておらず，本パッケージのコードは作者が独自に記述したものです．
著作権上の問題がありましたらご連絡ください（公開停止など含め対応します）．

## 主な特徴

1. 引数の個数による分岐:
  - 引数が0個から9個までの本体を，個数ごとに登録できます．
2. 終端の印が不要:
  - コマンドの直後に続く波括弧のグループを先読みして数えます．
3. 入れ子の呼び出し:
  - 引数をトークン列のまま本体へ渡すので，引数の中で同じコマンドを呼んでも結果は崩れません．
4. 見出しでの利用:
  - コマンドは `\protected` で定義されるので，見出しやキャプションの中でも使えます．
5. 誤りの検出:
  - 登録していない個数で呼んだときや，同じ個数を2回登録したときにエラーを出します．

## 動作条件

| 項目 | 条件 |
| --- | --- |
| TeX エンジン | pdfLaTeX，LuaLaTeX，XeLaTeX，pLaTeX，upLaTeX（e-TeX 拡張が必要） |
| 依存パッケージ | なし |
| ドキュメントの組版 | LuaLaTeX |

## ライセンス

MIT License の下で配布・改変が可能です．詳細は [LICENSE](LICENSE) を参照してください．

## 使用方法

プリアンブルで `overload` パッケージを読み込み，`\newoverload` でコマンドの名前を宣言してから，`\addoverload` で引数の個数ごとに本体を登録します．
本体では，`\newcommand` と同じく `#1`，`#2` が使えます．

```latex
\documentclass{ltjsarticle} % LuaLaTeX で組版する
\usepackage{overload}

\newoverload{\hoge}
\addoverload{\hoge}{0}{（引数なし）}
\addoverload{\hoge}{1}{一つ目は #1}
\addoverload{\hoge}{2}{一つ目は #1，二つ目は #2}

\begin{document}
\hoge       % （引数なし）
\hoge{A}    % 一つ目は A
\hoge{A}{B} % 一つ目は A，二つ目は B
\end{document}
```

| コマンド | 説明 |
| --- | --- |
| `\newoverload{\名前}` | 未定義のコマンドを，オーバーロードされたコマンドとして宣言する |
| `\renewoverload{\名前}` | 定義済みのコマンドを宣言し直す．登録済みの本体はすべて消える |
| `\addoverload{\名前}{個数}{本体}` | 引数がちょうど〈個数〉個のときの本体を登録する（〈個数〉は0から9） |
| `\enddelim` | 引数の列をここで終えると明示する（省略できる） |

引数は，コマンドの直後に続く波括弧のグループを1つずつ数えます．
次のトークンが `{` でないとき（空白を含む）と，登録済みの最大の個数に達したときに，数えるのを終えます．

```latex
\hoge{A}{B}           % 引数2個
\hoge{A} {B}          % 引数1個．空白と {B} はそのまま残る
\hoge{A}\enddelim{B}  % 引数1個．{B} はそのまま残る
```

> Note: 展開だけが許される場所（`\edef` の中，数値や長さを書く位置，PDF のしおり）では使えません．
> 対処を含む制約の一覧は，`overload-doc.pdf` の「制約と注意」を参照してください．

詳細な仕様や出力例については，`overload-doc.tex`（およびコンパイル後の PDF）をご参照ください．

### 旧版からの移行

Version 1.0 以前はパッケージ名が `myoverload` で，呼び出しの末尾に `\enddelim` が必須でした．
旧版の文書では，`\usepackage{myoverload}` を `\usepackage{overload}` に書き換えてください．
`\enddelim` は付けたままでも動きます．
ただし，波括弧で囲まない引数（`\hoge A B\enddelim`）は数えなくなったので，`\hoge{A}{B}` に書き換えてください．

---

## 変更履歴

- Version 2.0 (2026-11-25): `overload` に改名．先読みによる引数の判定に変更し，`\enddelim` を省略可能にした．内部名の衝突，入れ子の呼び出しでの引数の破損，登録していない個数でエラーが出ない問題を修正．日本語のドキュメント（`overload-doc.pdf`）と試験（`test/overload-test.tex`）を追加．
- Version 0.3 (2026-04-08): 実装の整理（内部共通化・`\ifcase` による分岐）．
- Version 1.0 (2021-11-10): CTAN 掲載向けの準備．
- Version 0.2 (2021-08-02): `\renewoverload` を追加．
- Version 0.1 (2021-08-01): 初版．
