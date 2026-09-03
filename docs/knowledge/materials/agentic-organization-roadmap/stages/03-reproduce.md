---
title: Stage 3 - 再現可能にする
status: active
updated: 2026-09-03
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-small-scope-learning-loop]
stage_ids: [stage-3]
platform_ids: [platform-neutral]
source_ids: [claim-stage3-001, claim-stage3-002]
derived_artifacts: []
review_triggers: [nist-ai-rmf-revision, nist-genai-profile-revision, stage-3-change]
---

# Stage 3 - 再現可能にする

## このStageの目的

一度うまくいった作業を、別の人や別の時点でも同じ条件で試せるようにします。「同じpromptを使う」だけでは、入力データ、権限、接続先、modelやserviceのversionが変わった場合に結果を比較できません。

NIST AI RMFは、評価に使うtest、指標、tool、実行条件、限界を記録し、運用中も継続的に監視することを示しています。[NIST AI RMF Core](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/)

## 記録するもの

- 正本: 現在有効な手順を置く場所
- 目的と期待出力
- 入力データの種類と取得時点
- AIに与えた指示
- 使用したmodel、service、toolとversion
- 許可した権限と承認条件
- 実行日、担当、所要時間、費用
- 合否と確認結果
- 例外、失敗、復旧方法

## 再現確認

1. 別の人が正本だけを読んで実行する。
2. 同じ合否基準で結果を確認する。
3. 差が出た場合、入力、環境、version、権限、手順のどこが変わったかを記録する。
4. 正本を更新し、古い手順を正本として残さない。

## 次へ進む条件

- 正本の場所を一つ説明できる。
- 入力、環境、version、権限、合否を記録している。
- 別の人が同じ合否基準で再試行できる。

## 次に行うこと

直近の成功例について、使用した入力、model/service、権限、合否を一行ずつ記録します。

## Evidence

| claim_id | claim | source_url | source_type | checked_at | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|
| claim-stage3-001 | AIのtest、指標、tool、実行条件、限界を文書化し、運用中も評価・監視する | https://airc.nist.gov/airmf-resources/airmf/5-sec-core/ | public | 2026-09-03 | platform-neutral, stage-3 | NIST Measure の直接的な内容を再現記録へ具体化 | nist-ai-rmf-revision |
| claim-stage3-002 | GenAI riskはlifecycle、system、use case、入力、出力、時間で異なり、contextに応じて評価する | https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf | public | 2026-09-03 | generative-ai, stage-3 | NIST GenAI Profile の直接的な内容。version記録はroadmap recommendation | nist-genai-profile-revision |

## 関連

- [ロードマップ全体](../roadmap.md)
- [環境再現性の技術例](../../../../framework/environment-reproducibility.md)
- [AI間引き継ぎの技術例](../../../../framework/agent-handoff.md)
