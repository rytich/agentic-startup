---
title: Worksheet - 一作業の委任定義
status: draft
updated: 2026-09-04
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-small-scope-learning-loop, principle-human-accountability]
stage_ids: [stage-2]
platform_ids: [platform-neutral]
source_ids: []
derived_artifacts: []
review_triggers: [stage-2-change, source-policy-change]
---

# Worksheet - 一作業の委任定義

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

最初の試行では、許可する操作を一つにし、読み取りまたは下書き作成から始めます。

## Evidence（旧形式・再調査中）

| claim_id | claim | source_url | source_type | checked_at | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|
| claim-stage2-001 | 過剰な機能、権限、自律性は損害を招く主因になる | https://genai.owasp.org/llmrisk/llm062025-excessive-agency/ | official | 2026-09-03 | worksheet-delegation | OWASP の直接的な内容 | owasp-llm-top10-change |
| claim-stage2-002 | 最小機能・最小権限と高影響操作への人間承認が主要対策である | https://genai.owasp.org/llmrisk/llm062025-excessive-agency/ | official | 2026-09-03 | worksheet-delegation | OWASP 対策を記入欄へ変換したroadmap recommendation | owasp-llm-top10-change |

## 関連

- [Stage 2](../stages/02-delegate.md)
