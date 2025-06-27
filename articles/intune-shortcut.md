---
title: "IntuneからPowerShellでショートカットを配布する"
emoji: "💻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["intune","autopilot", "windows", "mdm", "powershell"]
published: true
publication_name: "gmomedia"
---

# 概要
Intuneでは、任意のショートカットをWindows端末に配布することができます。
.intunewinファイルを作成し、配布と端末にインストールしたアプリケーションでショートカットが作成されないケースがあったため
Intuneから手動でショートカットを配布する仕組みを活用してみました。


# 前提条件
- Microsoft Intuneの管理者権限を持っていること

# PowerShellスクリプトの作成
まず、ショートカットを作成するためのPowerShellスクリプトを作成します。
以下のスクリプトは、デスクトップにショートカットを作成する例です。
すでに同じショートカットが存在するかのチェックも含まれています。
（hoge部分を適当に置き換えてください）

```powershell:shortcut.ps1
$Shell = New-Object -comObject WScript.Shell
$DesktopPath = [System.Environment]::GetFolderPath([System.Environment+SpecialFolder]::Desktop)
$ShortcutPath = "$DesktopPath\hoge.lnk"

if (-not (Test-Path -Path $ShortcutPath)) {
    $Shortcut = $Shell.CreateShortcut($ShortcutPath)
    $Shortcut.TargetPath = "C:\Program Files (x86)\hogehoge\hoge.exe"
    $Shortcut.Save()
}
```
※パスの中にスペースがあってもエスケープする必要はありません。
※各のパスの指定先に日本語が含まれている場合は文字コードに注意してください。


# 設定手順
1. Micorosoft Intune管理センターにログイン: [Micorosft Intune管理センター](https://intune.microsoft.com/)
1. デバイス→スクリプトと修復→プラットフォームスクリプトへ遷移: [スクリプトと修復](https://intune.microsoft.com/#view/Microsoft_Intune_DeviceSettings/DevicesMenu/~/scripts)
1. 追加→Windows10以降 をクリック
![](/images/intune-shortcut/1.png)
1. 任意の名前を入力し、次へ
![](/images/intune-shortcut/2.png)
1. 先ほど作成したPowerShellスクリプトをアップロードし、画像のとおりに設定
![](/images/intune-shortcut/3.png)
1. 任意のグループに割り当て保存
しばらくすると、対象の端末にショートカットが作成されます。
![](/images/intune-shortcut/4.png)

# Tips
※Intuneで設定したスクリプトは管理センター上で確認することができません。
別途Gitなどでバージョン管理しておくことをおすすめします。
