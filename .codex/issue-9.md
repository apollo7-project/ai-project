#9 Test Codex after AGENTS.md

## 要件定義書

**案件名**:「Notion → Zapier → Discord 自動告知システム」<br>**版数**: v1.0 **作成日**: 2025-06-04 **作成者**: ChatGPT (o3)

---

### 1\. 背景と目的

講座アーカイブ動画とまとめテキストを、毎回 **18:00（JST）** に Discord へ自動投稿して告知作業を完全自動化する。手動ミス削減と運営負荷の最小化が目的。

### 2\. スコープ

| 項目 | 対象 | 備考 |
| -- | -- | -- |
| データ格納 | Notion データベース | 既存ページを DB 化 |
| 自動処理 | Zapier 1 Zap | Trigger ×1 / Action ×2 (+Loop) |
| 送信先 | Discord チャンネル 1 つ | Webhook/Bot 利用 |
| 時刻管理 | 毎日 18:00（Asia/Tokyo） | 週次などへの変更は拡張要件 |

### 3\. 用語

* **Lecture DB**: 講座情報を行単位で保持する Notion データベース
* **Post At**: 投稿日時を保持する Date/Time 型列
* **ReadyToPost**: Boolean トグル列。「Yes」の行のみ送信
* **Zap**: Zapier で構成する自動処理ユニット

### 4\. システム構成図（論理）



### 5\. データ要件（Lecture DB 設計）

| 列名 | データ型 | 必須 | 用途 |
| -- | -- | -- | -- |
| Lecture Date | Date (含む時刻可) | ◯ | 開催日・基準日 |
| Title | Text | ◯ | 講座タイトル |
| Video URL | URL | ◯ | Google Drive 等の共有リンク |
| Summary URL | URL or Text | △ | PDF / Notion ページ / 長文テキスト |
| Post At | DateTime | △ | 送信日時を直接指定する場合 |
| ReadyToPost | Checkbox | △ | 送信許可フラグ |

> **制約**: 動画ファイルを直接添付しない（Discord 制限 8 MB / 50 MB）。

### 6\. 機能要件

| ID | 機能 | 詳細 |
| -- | -- | -- |
| F-1 | スケジュールトリガ | 「Schedule by Zapier」で毎日 18:00 (Asia/Tokyo) に Zap を起動 |
| F-2 | 講座検索 | Notion → Find Database Item(s) で以下条件を満たす行を取得 ・ もしくは  ・（列がある場合） |
| F-3 | ループ処理 | 取得結果が複数行の場合、Looping by Zapier で 1 行ずつ処理 |
| F-4 | Discord 投稿 | Discord – Send Channel Message で以下本文テンプレを送信 ```\n**📚 {{Title}}**　({{Lecture Date |
| F-5 | 失敗通知 | Zap Run が失敗した場合、Zapier 内通知 + 管理者メール送信 |
| F-6 | 手動再送 | Zap History から “Replay” で再送可能にする |

### 7\. 非機能要件

| 区分 | 要件 |
| -- | -- |
| 信頼性 | 送信遅延許容 ±5 分以内 |
| 可用性 | Zapier 稼働率 99% 以上（Zapier SLA 準拠） |
| 保守性 | Notion 列追加のみで拡張可能／Zap 内のマッピング変更不要 |
| セキュリティ | Discord Webhook URL を環境変数に格納し、共有禁止 |

### 8\. 拡張要件（Nice to Have）

| ID | 内容 | 実装方針 |
| -- | -- | -- |
| X-1 | 曜日固定開催 | Schedule Trigger を Every Week に変更 |
| X-2 | PDF 添付 | Summary PDF ≤ 8 MB を Attach File に指定 |
| X-3 | 詳細抜粋 | Notion API Custom Request で子ブロック取得 → Discord へ整形送信 |

### 9\. テスト計画

| フェーズ | テスト項目 | 期待結果 |
| -- | -- | -- |
| 単体 | F-2 条件分岐 | フィルター一致行のみ取得 |
| 結合 | F-1→F-4 流れ | 18:00 に Discord へメッセージ投稿 |
| 障害 | Video URL 欠落 | Zap Error 発生 & 管理者通知 |

### 10\. 運用 & 保守フロー

1. 講座追加時に Lecture DB 行を作成し **ReadyToPost=Yes** にする
2. Zapier が毎日 18:00 に自動投稿
3. 異常時は Zapier ダッシュボードで Run を確認し再送 or 修正
4. 年度切替や列追加時は Zap マッピングを確認

### 11\. 制約・前提条件

* Zapier 有料プラン（Notion + Schedule + Loop 利用）
* Discord 側に Bot 権限 / Webhook URL 発行済み
* Google Drive 等で動画を公開リンク化（権限「リンクを知っている全員」）

### 12\. 受け入れ基準

* テスト日に Lecture Date を当日設定 → 18:00 ±5 分内に Discord 投稿される
* 投稿内容がテンプレートと一致 (改行・絵文字含む)
* Notion に行を追加すれば次サイクルで自動反映

---

**以上で要件定義完了。**<br>ご確認のうえ、追加要望・修正点があればお知らせください。
