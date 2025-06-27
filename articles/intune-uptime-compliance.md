---
title: "Intuneで端末の起動日数を監視する"
emoji: "💻"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["intune","security", "windows", "mdm", "powershell"]
published: true
publication_name: "gmomedia"
---

# 概要
Windowsに限らず、各アプリケーションやOSの更新を適用させるためには、端末の定期的な再起動が重要です。
Intuneで管理しているWindows端末の起動日数を監視することで、端末の更新適用状況を把握し、必要に応じてユーザに再起動を促すことができます。


# 前提条件
- Microsoft Intuneの管理者権限を持っていること

# ①コンプライアンスに使用するPowerShellスクリプトの作成
まず、端末の起動日数を取得するためのPowerShellスクリプトを作成します。
以降、起動日数を`uptimedays`とします。


```powershell:uptimedays.ps1
$bootuptime = (Get-CimInstance -ClassName Win32_OperatingSystem).LastBootUpTime
$currenttime = Get-Date
$uptime = $currenttime - $bootuptime
$uptimedays = $uptime.Days

$hash = @{ "UptimeDays" = $uptimedays }
return $hash | ConvertTo-Json -Compress
```

# ②コンプライアンス用JSONの作成
次に、上記のスクリプトをIntuneのコンプライアンスルールで使用するためのJSON形式に変換します。
以下のJSONは、起動日数が7日超過している場合にコンプライアンス違反とし、ユーザに通知を行う設定です。
参考リンク: 
https://learn.microsoft.com/ja-jp/intune/intune-service/protect/compliance-custom-json 

```json:起動日数監視コンプライアンス.json
{
    "Rules": [
        {
            "SettingName": "UptimeDays",
            "Operator": "LessThan",
            "DataType": "Int64",
            "Operand": "7",
            "MoreInfoUrl": "https://example.com/uptime-policy",
            "RemediationStrings": [
                {
                    "Language": "en_US",
                    "Title": "Device uptime exceeds policy",
                    "Description": "Your device has been running for 7 days or more. Please restart your device to ensure it receives important updates and maintains optimal performance."
                },
                {
                    "Language": "ja_JP",
                    "Title": "デバイスの起動時間がポリシーを超過しています",
                    "Description": "デバイスが7日以上起動し続けています。重要な更新を確実に受け取り、最適なパフォーマンスを維持するため、デバイスを再起動してください。"
                }
            ]
        }
    ],
    "ScheduledActionsForRule": [
        {
            "RunRemediationScript": false,
            "ScriptName": null,
            "Rules": ["UptimeDays"],
            "NotificationMessageCCType": "Warn",
            "NotificationTitle": {
                "TranslatedString": [
                    {
                        "Language": "en_US",
                        "Text": "Device Restart Required"
                    },
                    {
                        "Language": "ja_JP",
                        "Text": "デバイスの再起動が必要です"
                    }
                ]
            },
            "NotificationBody": {
                "TranslatedString": [
                    {
                        "Language": "en_US",
                        "Text": "Your device has been running for 7 days or more. Please restart your device as soon as possible to ensure it receives important updates and maintains optimal performance."
                    },
                    {
                        "Language": "ja_JP",
                        "Text": "デバイスが7日以上起動し続けています。重要な更新を確実に受け取り、最適なパフォーマンスを維持するため、できるだけ早くデバイスを再起動してください。"
                    }
                ]
            },
            "GracePeriodHours": 1,
            "NotificationRepeatIntervalInDays": 1
        }
    ]
}

```


# uptimedays取得用スクリプトの設定手順
1. Micorosoft Intune管理センターにログイン: [Micorosft Intune管理センター](https://intune.microsoft.com/)
1. デバイス→コンプライアンスへ遷移: [コンプライアンス](https://intune.microsoft.com/#view/Microsoft_Intune_DeviceSettings/DevicesMenu/~/compliance)
1. スクリプトタブ→追加→Windows10以降
![](/images/intune-uptime-compliance/1.png)
1. 任意の名前を入力し、次へ
![](/images/intune-uptime-compliance/2.png)
1. ①で作成したPowerShellスクリプトを貼り付けし、オプションは画像のとおりに設定
![](/images/intune-uptime-compliance/3.png)
1. 次へ進み、作成をクリック

# コンプライアンスの設定手順
1. デバイス→コンプライアンス→スクリプトへ遷移: [コンプライアンス](https://intune.microsoft.com/#view/Microsoft_Intune_DeviceSettings/DevicesMenu/~/compliance)
1. ポリシータブ→ポリシーの作成→プラットフォーム Windows 10以降、プロファイルの種類 Windows10/11のコンプライアンスポリシー を選択し作成をクリック
![](/images/intune-uptime-compliance/4.png)
1. 任意の名前を入力し、次へ
![](/images/intune-uptime-compliance/5.png)
1. カスタムコンプライアンスを展開し、カスタムコンプライアンスを必要を選択
1. `検出スクリプトを選択する` で①において作成したスクリプト名で検索し設定
![](/images/intune-uptime-compliance/6.png)
1. `カスタム コンプライアンス設定で JSON ファイルをアップロードし検証する`では
②で作成したJSONファイルをアップロード
![](/images/intune-uptime-compliance/7.png)
1. コンプライアンス非対応に対するアクションはそのまま。任意のグループに割り当て保存※
![](/images/intune-uptime-compliance/8.png)

※割り当ては、コンプライアンスを適用したい端末が所属する**ユーザグループ**を選択してください。
全てのデバイスを割り当てるとコンプライアンスチェックが2回以上走り、集計がおかしくなります。
詳細は以下の記事が参考になります。
https://note.com/nix_ki/n/n7a868ff7f171

# 動作確認
コンプライアンスが各端末に適用されると、端末の起動日数が7日を超えた場合にコンプライアンス違反となることが確認できます。
![](/images/intune-uptime-compliance/9.png)
