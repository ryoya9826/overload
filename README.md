# myoverload

LaTeX2e 用の小さなパッケージです。**引数の個数に応じて**同じ制御綴の振る舞いを切り替えられます（いわゆるマクロの「オーバーロード」風の使い方）。

- **ファイル:** `myoverload.sty`
- **使い方:** `\usepackage{myoverload}`（`TEXINPUTS` にパスを通すか、プロジェクト直下に置いて `\usepackage{./myoverload}` など）

## 背景

着想は次の記事です: [LaTeXでマクロの引数をオーバーロード](https://hak7a3.hatenablog.com/entry/2015/05/24/202509)。2021年8月時点で当該サイトに実装の公開はなく、本リポジトリのコードは作者が独自に記述したものです。著作権上の問題がありましたらご連絡ください（公開停止など含め対応します）。

## クイックスタート

```latex
\documentclass{article}
\usepackage{myoverload}

\newoverload{hoge}
\addoverload{hoge}{0}{（引数なし）}
\addoverload{hoge}{1}{一つ目は #1}
\addoverload{hoge}{2}{一つ目は #1、二つ目は #2}

\begin{document}
\hoge\enddelim
\hoge{A}\enddelim
\hoge{A}{B}\enddelim
\end{document}
```

呼び出しの**末尾**には必ず `\enddelim` を付けます（終端マーカー）。

## 提供されるコマンド

| コマンド | 説明 |
|----------|------|
| `\newoverload{名前}` | 未定義の制御綴にオーバーロード用の入口を定義する |
| `\renewoverload{名前}` | 既存の制御綴を上書きして同様に定義する |
| `\addoverload{名前}{n}{展開}` | 引数がちょうど `n` 個のときの本体を登録する（`n` は 0〜9） |
| `\enddelim` | 引数列の終端。単体ではエラーになる |

`\addoverload` の第3引数は通常のマクロ本体と同様に `#1`…`#n` が使えます。

## 注意事項

- **`\enddelim` を引数の先頭に直接置かないでください。** どうしても必要なら、グループで囲むなどしてトークンを分けます。

  ```latex
  \hoge{{\enddelim} 本文}\enddelim
  ```

- 各引数の中で `\if...` 系を使う場合は、展開順序に注意が必要です（一般的なマクロ実装と同様）。

## 変更履歴

| 版 | 日付 | 内容 |
|----|------|------|
| v0.3 | 2026-04-08 | 実装の整理（内部共通化・`\ifcase` による分岐） |
| v1.0 | 2021-11-10 | CTAN 掲載向けの準備 |
| v0.2 | 2021-08-02 | `\renewoverload` を追加 |
| v0.1 | 2021-08-01 | 初版 |

## ライセンス

[MIT License](LICENSE)

---

**Ryoya Ando** — <https://ryoya9826.github.io/>
