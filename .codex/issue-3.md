#3 Discord:講座アーカイブ動画・まとめ資料投稿

### “Notion → Zapier → Discord” で **動画リンクとまとめテキスト** を

### (仮)毎回 18 : 00 に自動投稿する構成

ChatGPTリンク

[ChatGPT - NotionとDiscord自動投稿](https://chatgpt.com/share/683eb344-abd0-8008-919f-3c9c7a8c2bbd)

---

## 1\. Notion 側 ― データを Zapier が拾いやすい形に整える

| 手順 | 操作 | ポイント |
| -- | -- | -- |
| ① 講座一覧を **データベース化** | 画像のリスト部分を選択 → **Turn into database → Table – Full Page** | データベースに変えると Zapier が 1 行＝1 講座 として扱える |
| ② 列（プロパティ）を追加 | *Lecture Date* (日付)*Title* (テキスト)*Video URL* (URL)*Summary URL* または *Summary Text* | **URL 列**ならリンクを貼るだけで OK／長文ならテキスト列に貼り付け |
| ③ 投稿時刻を持たせる方法（どちらも可） | A. 別列 *Post At* に「2025-05-22 18:00」など日時を入れるB. or 講座日に固定開催なら *Lecture Date* に「18:00」を含める | Zapier で「今日」「今からの１時間以内」などフィルターが組める |

> **サイズが大きい動画は “添付” ではなくリンクで渡す**<br>Discord Webhook/Bot の添付上限は通常 8 MB（Nitro でも 50 MB）なので、Google Drive 共有リンクなどが現実的です。 ([support.discord.com](https://support.discord.com/hc/en-us/community/posts/360031093812-Message-Max-Lenght?page=2&utm_source=chatgpt.com))

---

## 2\. Zapier 側 ― 毎日 18 : 00 に走る Zap を組む

1. **Trigger**
   * **Schedule by Zapier** → Every Day → 18:00 (Asia/Tokyo)<br>→ ザックリ「毎日 18:00 に発火」させる。 ([help.zapier.com](https://help.zapier.com/hc/en-us/articles/8496288648461-Schedule-Zaps-to-run-at-specific-intervals?utm_source=chatgpt.com))
2. **Action 1 : Notion – Find Database Item(s)**
   * Database: ①で作った講座 DB
   * Filter:
     *  が  **かつ**
     * （必要なら） ≤ 
   * その日分だけを取得。
3. **Action 2 : Discord – Send Channel Message**
   * Channel: 告知用チャンネル
   * Message Text 例（テンプレに保存しておく）

     
   * 必要なら *Attach File* に  を指定（8 MB 以下の場合）。
4. （複数講座を同日に流す場合）
   * **Looping by Zapier** で Action 1 の配列を回すだけ。コード不要。

---

## 3\. メッセージ例（Discord 出力イメージ）



> Discord 文字数は 1 投稿 2 000 字まで。長い要約は PDF／別ページにし、リンクを貼るのが安全です。 ([support.discord.com](https://support.discord.com/hc/en-us/community/posts/360031093812-Message-Max-Lenght?page=2&utm_source=chatgpt.com))

---

## 4\. “完全放置” をさらにラクにする小ワザ

| シナリオ | 追加設定 |
| -- | -- |
| Notion 側で **「公開 OK」トグル** を付けたい | 列  を作成 → Zapier のフィルターで  の行だけ送信 |
| 曜日固定で毎週開催 | Trigger を “Every Week → 曜日指定” に変更するだけ |
| まとめテキストを **本文にそのまま貼りたい** |  列に入れる → Discord の  にマッピング |

---

## 手順まとめ

1. **Notion を DB 化し “日付＋リンク” を行プロパティにする**
2. **Schedule → Notion 検索 → Discord 送信** の３ステップ Zap を作る
3. これで **毎回 18 : 00、自動でアーカイブ動画＋まとめを Discord に投稿** 完了

> Notion 連携は Zapier 標準アプリでサポートされており、Find / Retrieve / Create など主要アクションが使えます。 ([zapier.com](https://zapier.com/blog/automate-notion/?utm_source=chatgpt.com))

もし **子ページのブロック内容を丸ごと抜きたい**、**複数ファイルを添付したい** など更に踏み込む場合は、Notion API を「Custom Request」ステップで呼び出して JSON をパース → Discord へ送信する方法もあります。
