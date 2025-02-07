---
title: "GitHub Copilot公式のAIエージェント（Agent Mode）がいよいよプレビューリリース！"
emoji: "🚀"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["githubcopilot","githubcopilotchat", "AIエージェント", "cline"]
published: true
publication_name: "gmomedia"
---

# 前説
先日、間接的にGitHubCopilotを使用してコーディング支援を行うReclineに関する記事を投稿しました
https://zenn.dev/gmomedia/articles/cbafecc45f9b75
しかし、使用頻度の度が過ぎるとアカウントBANされるケースがあるようでした。

この度いよいよGitHub公式からGitHub Copilot Agent Modeがプレビューリリースされました。
なお、現時点ではVSCode Insidersで利用可能です。

公式リリース記事はこちら
https://github.blog/jp/2025-02-07-github-copilot-the-agent-awakens/

# さっそく使ってみる
使用方法をサクサク紹介していきます。

## 前提条件
- GitHub Copilotが利用できること
- VSCode Insiders Ver1.98が利用できること（緑のアイコンのVSCode）
:::message
なお、2025/02/07時点での記事です。
近日安定版でも利用可能になっているはずですので、
その際はVSCode Insidersではなく安定版のVSCodeでご利用ください。
:::

## VSCode Insidersのインストール
こちらからどうぞ
https://code.visualstudio.com/insiders/
:::message
インサイダー版ということで、安定版より動作が不安定な場合があります。ご注意ください。
:::

## Agent Modeの有効化
VSCode左下の設定の歯車→設定で`github agent`で検索すると有効化オプションが出てくるので有効にする。
![](/images/github-copilot-ai-agent-preview/1.png)

## 使ってみる
1. VSCode上部のGitHubCopilotのマーク（Octocatくん）を押下
![](/images/github-copilot-ai-agent-preview/2.png)

1. Editモードに設定し、テキストボックスのモード設定からエージェントモードを選択
![](/images/github-copilot-ai-agent-preview/3.png)


1. 現在利用可能なモデルはGPT-4oとClaude 3.5 Sonnetだそうです。
![](/images/github-copilot-ai-agent-preview/4.png)


あとは指示をするだけで使用できるようになります。

# 使ってみた感想
めちゃくちゃ爆速です。Reclineは頑張って処理しているなぁという感じがありましたが
GitHub公式のAgent Modeはかなり早いです。ぜひ体感してみてください。

:::details 作成の様子（字が小さいので拡大推奨）
![](/images/github-copilot-ai-agent-preview/5.png)
![](/images/github-copilot-ai-agent-preview/6.png)
:::

## Tips
エージェントということで、clineと同じように基本的に指示をするだけでコーディングをしてくれます。
ただし、プレビュー版なせいか返答テキストが英語になってしまいます。
`日本語で作業して`と文末に加えることで日本語で返答してくれます。

# まとめ
Agent ModeはGitHub公式のため、アカウントBANのリスクはかなり少ないかと思います。
そして、GitHub Copilotの課金の範囲で利用できるのはかなり大きなメリットです。
正式リリースが楽しみですね。