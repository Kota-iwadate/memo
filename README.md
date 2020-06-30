日々の作業で忘れそうになるあれやこれを自分用にメモしようと思いました。
まめに更新していこうと思います。
<!-- TOC -->

- [Elixir](#elixir)
    - [基本](#基本)
        - [算術](#算術)
            - [割り算](#割り算)
    - [Enum](#enum)
        - [Enumデータで特定の条件が該当する要素を含むか、全ての要素が条件を満たすか判定したい。](#enumデータで特定の条件が該当する要素を含むか全ての要素が条件を満たすか判定したい)
        - [Elixirでの内包表記](#elixirでの内包表記)
    - [String](#string)
        - [一文字ずつsplitしたいとき](#一文字ずつsplitしたいとき)
    - [その他](#その他)
        - [競技プログラミング](#競技プログラミング)
            - [Atcoder](#atcoder)
- [Visual Studio Code](#visual-studio-code)
    - [ショートカットキー](#ショートカットキー)
        - [コマンドパレットを出したいとき](#コマンドパレットを出したいとき)
        - [Markdown Previewを別のタブに表示したいとき](#markdown-previewを別のタブに表示したいとき)
    - [Extension](#extension)
        - [括弧の対応関係を色分けしてくれる](#括弧の対応関係を色分けしてくれる)
        - [Markdownで目次をつくる](#markdownで目次をつくる)
        - [Git History](#git-history)
- [Git](#git)
    - [初期設定まわり](#初期設定まわり)
        - [リモートリポジトリの登録](#リモートリポジトリの登録)
        - [公開鍵の登録](#公開鍵の登録)

<!-- /TOC -->


<a id="markdown-elixir" name="elixir"></a>
# Elixir
<a id="markdown-基本" name="基本"></a>
## 基本
<a id="markdown-算術" name="算術"></a>
### 算術
<a id="markdown-割り算" name="割り算"></a>
#### 割り算
elixirでの割り算は`/`,ただし`/`は常に浮動小数を返す。整数同士で割り算したい場合は`div`を使う。
```bash
iex(4)> 12 / 6   
2.0
iex(5)> div(12, 6)
2
```

<a id="markdown-enum" name="enum"></a>
## Enum
<a id="markdown-enumデータで特定の条件が該当する要素を含むか全ての要素が条件を満たすか判定したい" name="enumデータで特定の条件が該当する要素を含むか全ての要素が条件を満たすか判定したい"></a>
### Enumデータで特定の条件が該当する要素を含むか、全ての要素が条件を満たすか判定したい。
`fn(x)`で書いたものと`&`記法を残す

```bash
iex(9)> Enum.all?([1,2,3], fn(x) -> rem(x,2) == 0 end) 
false
iex(10)> Enum.any?([1,2,3], fn(x) -> rem(x,2) == 0 end) 
true
iex(11)> Enum.all?([1,2,3], &rem(&1,2) == 0)   
false
iex(12)> Enum.any?([1,2,3], &rem(&1,2) == 0)
true
```
<a id="markdown-elixirでの内包表記" name="elixirでの内包表記"></a>
### Elixirでの内包表記
コレクションに対して、繰り返し処理を簡潔に記述できる。ループで書くより宣言的で読みやすく、定着させたい書き方。
`for`とgeneratorで書く。generatorとは`x <- [1,2,3,4,5]`のところ。右辺には列挙可能な値を入れる。
まずは単純な例
```bash
iex(5)> list = [1, 2, 3, 4, 5]
iex(6)> for x <- list, do: x * x
[1, 4, 9, 16, 25]
iex(7)> list = for x <- 1..100, do: x 
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22,
 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42,
 43, 44, 45, 46, 47, 48, 49, 50, ...]
```
generatorのところでパターンマッチを使って書けるようになっている。
```bash
iex(12)> for {:one, val}<- [one: 1, two: 2], do: val
[1]
```
特定の条件に一致したものだけフィルタリングしたい場合は以下のように書く。
```bash
list = for x <- 1..100, rem(x, 2) == 0, do: x 
[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22,
 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42,
 43, 44, 45, 46, 47, 48, 49, 50, ...]
```

generatorが複数の場合は、以下のようになる。
```bash
iex(9)> for x <- [1,2], y <- [3,4,5], do: {x,y}  
[{1, 3}, {1, 4}, {1, 5}, {2, 3}, {2, 4}, {2, 5}]
```
要素数は、各generatorの要素数の積。入れ子になると理解しておく

フィルタの条件も複数記述することができる。この場合記述した条件をすべて満たす要素からリストが生成される。
```bash
iex(11)> for x <- [1,2], y<- [3,4,5], rem(x,2) == 0, rem(y,2) == 0, do:  {x,y} 
[{2, 4}]
```

これまでの例から、内包表記はリスト形式で値を返している。`into:`を指定することでリスト以外も生成することができる。
例えば、以下はMapを返す。

```bash
iex(15)> for x <- [1,2], y<- [3,4,5], rem(x,2)==0, rem(y,2)==0, into: %{}, do:  {x, y} 
%{2 => 4}
```

<a id="markdown-string" name="string"></a>
## String
<a id="markdown-一文字ずつsplitしたいとき" name="一文字ずつsplitしたいとき"></a>
### 一文字ずつsplitしたいとき

`String.codepoints`
```bash
iex(1)> String.codepoints("hogehoge")
["h", "o", "g", "e", "h", "o", "g", "e"]
```
https://hexdocs.pm/elixir/String.html#codepoints/1

<a id="markdown-その他" name="その他"></a>
## その他
<a id="markdown-競技プログラミング" name="競技プログラミング"></a>
### 競技プログラミング
<a id="markdown-atcoder" name="atcoder"></a>
#### Atcoder
提出用のテンプレ。標準入力・標準出力まわりの書き方をよく忘れてしまうのでメモしておく
ソースは下記の問題を解いたときのもの
[ABC081A - Placing Marble](https://atcoder.jp/contests/abs/tasks/abc081_a)
```elixir
defmodule Main do
  def placing_marble(seq) do
    seq |> String.codepoints
        |> Enum.map(&String.to_integer(&1))
        |> Enum.reduce(0, &(&1 + &2))
  end

  def main do
    seq =  IO.read(:line) |> String.trim()
    IO.puts placing_marble(seq)
  end

end
```

<a id="markdown-visual-studio-code" name="visual-studio-code"></a>
# Visual Studio Code
<a id="markdown-ショートカットキー" name="ショートカットキー"></a>
## ショートカットキー
<a id="markdown-コマンドパレットを出したいとき" name="コマンドパレットを出したいとき"></a>
###  コマンドパレットを出したいとき
`[Ctrl］＋［Shift］＋［P]`
<a id="markdown-markdown-previewを別のタブに表示したいとき" name="markdown-previewを別のタブに表示したいとき"></a>
### Markdown Previewを別のタブに表示したいとき
`［Ctrl］＋［Shift］＋［V］`
<a id="markdown-extension" name="extension"></a>
## Extension
<a id="markdown-括弧の対応関係を色分けしてくれる" name="括弧の対応関係を色分けしてくれる"></a>
### 括弧の対応関係を色分けしてくれる
[Bracket Pair Colorizer](https://marketplace.visualstudio.com/items?itemName=CoenraadS.bracket-pair-colorizer)
<a id="markdown-markdownで目次をつくる" name="markdownで目次をつくる"></a>
### Markdownで目次をつくる
[Markdown TOC](https://marketplace.visualstudio.com/items?itemName=AlanWalk.markdown-toc)
<a id="markdown-git-history" name="git-history"></a>
### Git History
[Git history](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory)

<a id="markdown-git" name="git"></a>
# Git
<a id="markdown-初期設定まわり" name="初期設定まわり"></a>
## 初期設定まわり
<a id="markdown-リモートリポジトリの登録" name="リモートリポジトリの登録"></a>
### リモートリポジトリの登録

```bash
git remote set-url origin リポジトリ名
```
<a id="markdown-公開鍵の登録" name="公開鍵の登録"></a>
### 公開鍵の登録

1.key生成とクリップボードに貼り付け

```bash
ssh-keygen -t rsa -b 4096 -C "メールアドレス"

clip < ~/.ssh/id_rsa.pub
```
2. keyの貼り付け。公開鍵名は任意でOK
https://github.com/settings/keys