---
title: "Slack勤怠投稿アプリを個人開発し社内で活用してみた✨️"
emoji: "✨️"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["slack","slackapp", "slackbolt", "個人開発"]
published: true
publication_name: "gmomedia"
---

# 前説
弊社では、Slackにて勤怠投稿を行っております。
- 出勤時に所属するチーム内に「業務開始します（出社）」というような文言を投稿
- 社内独自のSlack絵文字を押し、Reacji Channelerを発火させて全体チャンネルにも投稿
- ↑休憩時や退勤時も同様

という形で運用されていますが、毎回文字入力するのは地味に面倒です。
私はClippyというMacのアプリに定型文として上記の文言を登録していましたが、手入力は結構手間です。
ということで、勤怠投稿できるSlackアプリを個人開発として開発し、社内の皆さんに使ってもらいました。たくさんの従業員に使用して頂いています☺️

# アプリ作成に至った思い
世の中で、Slackで勤怠連絡している会社は多いはず。ゆくゆくは弊社だけではなくOSSとして活用して頂きたい、という思いがあり、夜な夜な熱中しながら開発を進めました。
ぜひ他社の方も導入して頂き、フィードバック頂けると幸いです。

# 目指す姿
せっかく作るのであれば、ただの勤怠投稿アプリではなく、勤怠操作をトリガーとして様々な機能を追加していきたいと考えていました。

- ワンクリックで複数チャンネルに勤怠投稿できる。
- チャンネルに直接勤怠投稿するパターンと、とあるメッセージのスレッド内に勤怠投稿するパターンのどちらもサポートしたい。
- 勤怠時に投稿するメッセージは個人ごとに変更できるようにしたい（個性を尊重したい）
- この人は出社しているのか、リモートなのか、既に退勤しているのかをプロフィールのステータスでわかるようにしたい。（🏢🏠💤）

# 完成したもの
こちらになります！
![](/images/slack-attendance-app/1.png)
*Slackアプリホームのキャプチャ*

「出勤お知らせちゃん」という名称になっていますがこの名称はconfig.yamlで変更可能です。

導入方法であったり、詳細については下記リポジトリのREADME.mdをご覧ください。
https://github.com/k-ishii1020/slack-attendance-app

勤怠投稿先や投稿メッセージはこのように設定できます。
![](/images/slack-attendance-app/2.png =350x)
![](/images/slack-attendance-app/3.png =350x)


# 開発後記
業務でSlackアプリを開発している中で、個人開発としても何か便利なものを作りたい！という気持ちが強くなり、このアプリを作成しました。
今回、OSSとしては3作目なのですが、実装していてとても楽しかったですし、社内の皆さんが実際に使ってくれているのを見ると大変嬉しいですね。

また、社内のとあるデザイナーさんがこのように画像を作成して宣伝してくださったおかげもあり、爆発的に使用人数が増えました！
このリアクションの賑やかさ、見ていてとても楽しいです！
![](/images/slack-attendance-app/4.png)

もし導入してみたいという方がいらっしゃいましたら、GitHubのリポジトリをご覧ください！
