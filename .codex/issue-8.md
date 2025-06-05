#8 Notion:ロール管理

---

ChatGPTリンク

[ChatGPT - Notion 課金通知設定](https://chatgpt.com/share/683eda6f-d188-8008-97ad-b2118251bd5e)

## 有料会員の「決済前日

リマインド」を **最少ステップで作る方法**

> **ゴール**<br>毎朝 9:00 に「明日決済がくるお客様」を自動で知らせて、付け忘れやロール剥がし忘れをゼロにする。

---

### 1 Notion に“会員台帳”を作る

| プロパティ | 種類 | やること & ねらい |
| -- | -- | -- |
| **Name** | タイトル | 会員名・IDを書く |
| **Plan** | Select |  /  を選ぶ |
| **Last Payment** | Date | 実際にお金が入った日 |
| **Next Payment** | Formula |  |
| **Reminder Date** | Formula | → **前日**になる |
| **Notified?** | Checkbox | 送信済みフラグ |

> **ここまででできること**
>
> * 「次はいつ課金か」を自動計算
> * 「その前日」を自動で出す
> * チェックボックスで“もう通知したか”を記録

---

### 2 Notion Automations で毎日チェック → 通知 → フラグON

1. **Trigger**
   * Every day — 09:00 — Asia/Tokyo
2. **Filter**
   *  **is exactly** 
   *  **is** unchecked
3. **Action**（好きな通知チャネルを 1 つ選ぶ）
   1. **Slack 通知**

      
   2. **メール送信**（Gmail）
   3. **Webhook 送信**（→ Discord／LINE／Zapier など）
4. **Action ②**
   *  を **Checked** にする（２重送信防止）

> **ワンクリックで完了**<br>Automations は GUI だけで組めるので、コード不要。通知文もその場でテンプレ化できます。

---

### 3 通知チャネル一覧（シンプル版）

| チャネル | 設定のしかた | 使いどころ |
| -- | -- | -- |
| **Slack** | 「Send Slack notification」を選ぶだけ | 社内でいちばん見る場所 |
| **メール** | 「Send mail」→ Gmail 宛先入力 | 経理や外注さんに確実送付 |
| **Discord** | Discord で Webhook URL を発行 → Automation で「Send Webhook」 | コミュニティ運営・スマホ通知に便利 |
| **LINE Notify** ほか | 同上（Webhook） | 個人 LINE で即プッシュ |
| **Zapier / Make** | Automation → Webhook → Zapier で分岐 | 「Slack＋メール両方」など多重送信 |

---

### 4 Zapier / Make で代替する場合（Notion Automations が使えない組織向け）

1. **毎日 09:00** トリガ
2. Notion **Search**:  & 
3. **Slack / Gmail** へ送信（各レコードをループ）
4. **Notion Update**: 

---

### 5 よくあるつまずき

| 症状 | 原因 → 解決 |
| -- | -- |
| 通知が来ない |  に時刻がないと 0:00 判定→ Automations の実行時刻を 0:00 以降にする／日時入りに直す |
| 年払いの日付がズレる |  は「12か月後」固定で閏年をまたぐと 2/28 → 3/28 になる→ 必要なら  を手動で上書き可に |
| 複数チャネルで送りたい | Automation → Webhook → Zapier / Make で好きなだけ分岐 |

---

## まとめ（30 秒で把握）

1. **台帳を Notion に作る**
   * 前回課金日 →  →  を式で自動化
2. **Automations**で
   * 毎朝 9 時に \*\*「前日が Reminder」\*\*の行だけ拾う
   * Slack / メール / Webhook で通知
   *  にチェックを付ける
3. **通知チャネル**は Slack・メール・Discord・LINE など自由。
4. **Zapier/Make** を使えば、同じロジックを外部サービスでも簡単に再現。

これで「決済前日に自動で教えてくれる仕組み」が、ノーコード＆30 分ほどで完成します🎉
