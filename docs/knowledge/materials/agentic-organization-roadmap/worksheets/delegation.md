---
title: Worksheet - 一作業の委任定義
status: draft
updated: 2026-09-05
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-small-scope-learning-loop, principle-human-accountability]
stage_ids: [stage-2]
platform_ids: [platform-neutral]
source_ids: [source-google-delegation-2026-08-22, source-simonwillison-claude-code-2026-07-21]
derived_artifacts: []
review_triggers: [management-loop-change, source-age-6-months, stage-2-change, source-policy-change]
---

# Worksheet - 一作業の委任定義

## 前回の結果から依頼を作る

- 前回確認した質問:
- 確認できた事実:
- 採用した推論・判断:
- 残った不足情報:

| 要素 | 今回の依頼 |
|---|---|
| `What` |  |
| `Why` |  |
| `How` |  |
| `Done` |  |

## 作業

- 目的:
- 期待出力:
- 入力:
- 渡してはいけないデータ:
- 許可する機能・操作:
- 禁止する機能・操作:
- 参照できる対象:
- 変更できる対象:

## 人間の承認

| 操作 | AIは下書きまで | 実行前に承認 | 承認者 |
|---|---|---|---|
| 外部への投稿・送信 |  |  |  |
| データの変更・削除 |  |  |  |
| 購入・契約・予約 |  |  |  |
| 権限・認証の変更 |  |  |  |
| その他の高影響操作 |  |  |  |

## 検証と停止

- 合格条件:
- 不合格条件:
- 最大時間:
- 最大実行回数:
- 費用上限:
- 即時停止する条件:
- 失敗時に戻す方法:
- 操作履歴の保存先:

## 次に行うこと

最初の試行では、許可する操作を一つにし、読み取りまたは下書き作成から始めます。完了後は[評価ワークシート](evaluation.md)で結果を判断し、次の依頼へ戻します。

## Evidence

| claim_id | claim | primary_source_url | source_type | source_published_or_updated_at | observed_at | source_fingerprint | checked_at | reception_urls | reception_published_at | reception_signal | reception_summary | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| claim-management-loop-002 | 委任を検証可能に分け、関連情報、境界、人間判断を明示する | https://cloud.google.com/blog/products/ai-machine-learning/how-agents-can-delegate-better | official | 2026-08-22 | — | — | 2026-09-05 | https://simonwillison.net/2026/Jul/21/cat-and-thariq/ | 2026-07-21 | mixed | contextは有効だが、例外を無視した硬い指示や情報過多は誤解を増やし得る | worksheet-delegation, audience-business-leader-ai-user | `What / Why / How / Done`の記入欄は本資料の推論 | management-loop-change, source-age-6-months |

## 関連

- [Stage 2](../stages/02-delegate.md)
- [効果・品質・費用の評価](evaluation.md)
- [マネジメントループ調査](../../../../planning/research/2026-09-05-agent-management-loop.md)
