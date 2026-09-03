---
title: Stage 0 - 全体を描く
status: active
updated: 2026-09-03
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-business-outcome-first, principle-small-scope-learning-loop]
stage_ids: [stage-0]
platform_ids: [platform-neutral]
source_ids: [claim-stage0-001, claim-stage0-002, claim-stage0-003]
derived_artifacts: [worksheet-system-map]
review_triggers: [audience-profile-change, nist-ai-rmf-revision]
---

# Stage 0 - 全体を描く

## このStageの目的

エージェント組織の完成図を作るのではなく、7領域を一度見渡し、分かっていることと分かっていないことを分けます。NIST AI RMF も、利用目的や事業価値、利用状況、利益、費用、risk tolerance を整理した上で初期の go/no-go 判断へ進む考え方を示しています。[NIST AI RMF Core](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/)

## 進め方

1. [7領域の初期設計図](../worksheets/system-map.md)を粗く埋める。
2. 各項目を `分かっている`、`未決定`、`現在は決めない` に分ける。
3. 今回検証する項目を一つだけ選ぶ。
4. 期待する成果を一文にする。

空欄が残っていても問題ありません。AI risk 管理は一度で完成する checklist ではなく、状況に合わせて反復するものです。[NIST AI RMF Core](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/)

## 次へ進む条件

- 今回扱う事業課題を一つ選べる。
- 期待する成果を一文で説明できる。
- 今回は扱わない範囲を一つ以上書ける。

## うまく進まない場合

- 課題が複数ある: 最も困っている人と、一番早く結果を確認できる課題を一つ選ぶ。
- 技術を決められない: Stage 0 では決めず、必要な成果と制約だけを書く。
- AI を使うべきか不明: `今回検証` とせず、AI を使わない方法との比較を次の調査項目にする。

## 次に行うこと

設計図の `事業目的` から、今回扱う課題を一つ選んで丸を付けます。

## Evidence

| claim_id | claim | source_url | source_type | checked_at | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|
| claim-stage0-001 | AI risk 管理は Govern、Map、Measure、Manage を反復し、組織の必要性と能力に合わせる | https://airc.nist.gov/airmf-resources/airmf/5-sec-core/ | public | 2026-09-03 | platform-neutral, stage-0 to stage-6 | NIST の直接的な内容 | nist-ai-rmf-revision |
| claim-stage0-002 | 目的、事業価値、利用範囲、期待効果・費用、risk tolerance の把握が初期判断を支える | https://airc.nist.gov/airmf-resources/airmf/5-sec-core/ | public | 2026-09-03 | platform-neutral, stage-0 | NIST Map 1〜3 の直接的な内容を平易に要約 | nist-ai-rmf-revision |
| claim-stage0-003 | GenAI risk 管理は組織の目標、法的要件、best practice、risk priority に合わせる | https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf | public | 2026-09-03 | generative-ai, stage-0 | NIST GenAI Profile の直接的な内容。7領域への対応は本資料の推論 | nist-genai-profile-revision |

## 関連

- [ロードマップ全体](../roadmap.md)
- [対象者プロファイル](../audiences/business-leader-ai-user.md)
