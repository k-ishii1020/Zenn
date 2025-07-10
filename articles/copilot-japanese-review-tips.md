---
title: "【日本語対応】GitHub Copilot コードレビュー機能のTips"
emoji: "🌸"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["GitHub", "GitHubCopilot", "AI", "コードレビュー", "Tips"]
published: true
publication_name: "gmomedia"
---
早くも新年度になり、桜が咲き始める季節になりましたね🌸
エイプリルフールネタを考えてみましたが、特に思い浮かびませんでした🤨

今回はGitHub CopilotのAIコードレビュー機能を使ったTipsを紹介します。

# GitHub Copilotのコードレビューとは
GitHub Copilotのコードレビュー機能を使うと、プルリクエストのコードレビューをCopilotにお願いすることができます。
https://docs.github.com/ja/copilot/using-github-copilot/code-review/using-copilot-code-review

ちなみに、GitHub Copilot Enterprise のサブスクリプションに加入している組織ではコーディングガイドラインを[別途設定](https://docs.github.com/ja/copilot/using-github-copilot/code-review/configuring-coding-guidelines)できるようですが、以下の記事は**Enterprise のサブスクリプションに加入していない組織のリポジトリでも使えるTipsです。**


## 日本語でレビューさせる方法
デフォルトだと英語でレビューされてしまいますが、PULL_REQUEST_TEMPLATE.mdに以下のように設定することで日本語でレビューしてもらえます。

``` markdown:.github/PULL_REQUEST_TEMPLATE.md
<!-- I want to review in Japanese. -->
## 要件

## テスト項目
- [x] xxxxxx
以下略
<!-- I want to review in Japanese. -->
``` 
日本語レビューして欲しい旨を先頭行と最終行で挟んであげると、Copilotが日本語でレビューしてくれます。
2025/3/7からこの設定を有効にしていますが、現在でも有用な設定です。
https://x.com/k_ishii1020/status/1897860133352620069
:::message
ちなみに、copilot-instructions.mdに記載しても効果はありませんでした。
~~現時点ではプルリクエストのCopilot AIレビューでは参照されないのかもしれません。~~
:::

:::message alert
2025/07/10 追記: 2025/06/13からパブリックプレビューではありますが、有料版Copilotを契約しているアカウントではcopilot-instructions.mdを参照したレビューが可能になりました。
本記事の内容は引き続き有用ですが、参考までにリンクを掲載しておきます。

https://github.blog/changelog/2025-06-13-copilot-code-review-customization-for-all/
:::



## レビューについて指示を出す
同じく、PULL_REQUEST_TEMPLATE.mdでレビューについてのプロンプトを埋め込んでおくと
その指示通りにレビューしてくれます。

``` markdown:.github/PULL_REQUEST_TEMPLATE.md
## レビューに関して
レビューする際には、以下のprefix(接頭辞)を付けましょう。
<!-- for GitHub Copilot review rule -->
[must] → かならず変更してね  
[imo] → 自分の意見だとこうだけど修正必須ではないよ(in my opinion)  
[nits] → ささいな指摘(nitpick) 
[ask] → 質問  
[fyi] → 参考情報
<!-- for GitHub Copilot review rule-->
## PRのルール
- まずはDraftでPRを作成する。
- レビューに出せる状態になったらOpenにする。（レビュワーがランダムにアサインされます）
- レビューなしでのmainブランチへのマージは原則禁止
```

------
![](/images/copilot-japanese-review-tips/2.png)

------

以下、あえて誤った実装をしたらどうなるか実験してみました。
### Copilotによるレビュー結果
![](/images/copilot-japanese-review-tips/1.png)

きちんとプレフィックス`[must]`を書いてくれました。

ポイントは…
- プロンプトを`<!-- for GitHub Copilot review rule -->`で挟む
- 英語で指示をする(日本語で指示するよりも指示が通りやすい傾向にある)
- AI向けの指示はコメントアウトしておくことで、人間がコメントを見た時にノイズにならない。

チームごとに定められたコーディング規約をここに書いておくと良さそうですね。
別のドキュメントにコーディング規約が書いてある場合は、コメントアウトしておくことで
ノイズにならずに済みます（ただし規約が二重管理になるデメリットもある）


## 所感
今までCodeRabbitやPR-Agentなど、AIコードレビューツールの検証をしてきましたが、
後発のGitHub CopilotによるAIコードレビュー機能もなかなか優秀です。
追加料金無しで利用できるのは大変ありがたいですね。
コードレビューの規約をPULL_REQUEST_TEMPLATE.mdに埋め込んでおくことで
Copilotによるレビュー品質の向上とレビュワーの負担軽減ができると思います。

PULL_REQUEST_TEMPLATE.mdのサンプルを置いておくので参考にしてみてください✨️
https://github.com/k-ishii1020/github-copilot-review-sample/blob/main/.github/PULL_REQUEST_TEMPLATE.md