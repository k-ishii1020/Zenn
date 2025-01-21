---
title: "GitHub Copilotを活用したAIエージェント Reclineを試してみる"
emoji: "🤖"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["recline","cline", "AIエージェント","GitHubCopilot"]
published: true
publication_name: "gmomedia"
---

ReclineというAIエージェントを使用してみたら、GitHub Copilotを初めて使用したときの感動を思い出しましたのでご紹介します。

# Reclineとは
ClineというVSCode上で動作するAIエージェントがあります。様々なLLM（GPTやClaudeなど）を使用し、コーディング支援をしてくれるものです。
GitHub Copilotとは異なり、指示をするだけでコーディング、各ファイルやディレクトリの作成など行ってくれるため、GitHub Copilotより一段階上を行くAIエージェントと言えます。
Clineは前述の通り、各LLMを使用する必要があるため、別途LLMの料金がかかります。

今回検証するReclineはそのClineをVSCodeのLanguage Model APIを経由したGitHub Copilotと組み合わせて使用できるものです。
つまり、GitHub Copilotを契約しているユーザであれば、別途LLMの利用料金を支払うことなく、Clineに近い機能を使用することができます。
Clineの詳細は以下で紹介されています。
https://www.ai-souken.com/article/what-is-cline


# Reclineの導入方法
検証環境は以下のとおりです。
- MacBook Pro M2
- OS: MacOS Sequoia 15.2
- VSCode: 1.96.3

## 前提条件
- GitHub Copilotが利用できること
- pnpm(JavaScriptパッケージ管理ツール)がインストールされていること
- codeコマンドが利用できること
- fdがインストールされていること ※
- ripgrepがインストールされていること ※

※2025/01/21時点でリリースされているReclineでは、ローカルへのインストールが必要。詳細は[@README](https://github.com/julesmons/recline?tab=readme-ov-file#recline-a-fork-of-cline)を参照
（コメントにて情報提供してくださった [@itmammoth](https://zenn.dev/itmammoth)さん、ありがとうございます）



## pnpmのインストール
1. 以下のコマンドでインストールします。
```sh
brew install pnpm
# 以下のコマンドを実行すると、.zshrcにパスが追加されます。
pnpm setup

exit 
# あるいは
source .zshrc
```

## codeコマンドを利用できるようにする
1. VSCodeを開いて、コマンドパレットを開きます。
1. `Shell Command: Install 'code' command in PATH`を検索して選択します。
![](/images/cbafecc45f9b75/1.png)
1. codeがインストールされれば成功
![](/images/cbafecc45f9b75/2.png)

## fdのインストール
1. 以下のコマンドでインストールします。

```sh
brew install fd
```
https://github.com/sharkdp/fd?tab=readme-ov-file#on-macos

# ripgrepのインストール
1. 以下のコマンドでインストールします。
```sh
brew install ripgrep
```
https://github.com/sharkdp/fd?tab=readme-ov-file#on-macos

## Reclineのインストール
本題のReclineのインストール方法です。

1. 任意のディレクトリでReclineのリポジトリをクローンします。
```sh
git clone https://github.com/julesmons/recline.git
cd ./recline
```
1. pnpmとvsceをインストールします。
```sh
pnpm install -g pnpm
pnpm install -g @vscode/vsce
```
1. 以下のコマンドでReclineをビルドします。
```sh
pnpm install
```

1. VSIX（Visual Studio Code Extension）ファイルを作成します。
```sh
pnpm run package
```

1. 作成されたVSIXファイルをVSCodeにインストールします。
```sh   
code --install-extension recline-x.x.x.vsix
```

VSCodeの拡張機能にRecline（リクライニング式ソファー）が追加されていればインストール成功です。
![](/images/cbafecc45f9b75/3.png)

## Reclineの設定
1. Ready When You Are.と表示されている画面でAPI ProviderをVSCode: Language Modelに設定、お好みのLLMを設定をします。
![](/images/cbafecc45f9b75/4.png)

1. 歯車マークをクリックして、Custom Instructionを設定することをオススメします。
![](/images/cbafecc45f9b75/7.png)


# Reclineを使ってみる
テスト用のディレクトリを作成して、Reclineを使ってみます。
今回はDockerでMySQLを動かすためのコード群一式を作成してもらましょう。お手並み拝見です。
![](/images/cbafecc45f9b75/8.png)

![](/images/cbafecc45f9b75/30.png)
![](/images/cbafecc45f9b75/31.png)


:::details 作成の様子（字が小さいので拡大推奨）
![](/images/cbafecc45f9b75/9.png)
![](/images/cbafecc45f9b75/10.png)
![](/images/cbafecc45f9b75/11.png)
![](/images/cbafecc45f9b75/12.png)
![](/images/cbafecc45f9b75/13.png)
![](/images/cbafecc45f9b75/14.png)
![](/images/cbafecc45f9b75/15.png)
![](/images/cbafecc45f9b75/16.png)
:::


最終的に出来上がったファイル一式です。
![](/images/cbafecc45f9b75/32.png)
指示を出してから、ファイル一式作成されるまで、私はSaveボタンしか押していませんw
Auto approveというオプションを使えば、Saveボタンを押す作業すら行う必要がなさそうですね。

また、秘匿情報について、.envを作成してくれたのに、Readmeに書いてしまったのは残念ですが、プロンプトできちんと指示してあげればよさそうですね。

# まとめ
指示をした後、コーディング、ファイル作成、ディレクトリ作成まで行ってくれるReclineは、
GitHub Copilotを使っているユーザにとっては非常に便利だと感じました。

また、GitHub Copilotを使用するため、GPTなどのLLMの利用料金を気にせず使用できるのは非常に魅力的です。
今回はDockerでMySQLを動かすためのコード群を作成してもらいましたが、業務アプリケーションのコーディングでも検証してみたいと思います。