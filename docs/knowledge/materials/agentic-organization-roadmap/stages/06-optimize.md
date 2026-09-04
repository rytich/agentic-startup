---
title: Stage 6 - 品質と費用を最適化する
status: draft
updated: 2026-09-04
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-business-outcome-first, principle-small-scope-learning-loop]
stage_ids: [stage-6]
platform_ids: [platform-neutral]
source_ids: []
derived_artifacts: [worksheet-evaluation]
review_triggers: [finops-framework-change, ai-cost-guidance-change, stage-6-change, source-policy-change]
---

# Stage 6 - 品質と費用を最適化する

## このStageの目的

AIの利用回数やtoken削減だけでなく、事業成果、品質、人の確認時間、失敗、service費を合わせて評価します。FinOps Frameworkは、technologyの費用を事業価値へ結び付け、engineering、finance、businessが協力してdataに基づく判断と責任を持つ運用を示しています。[FinOps Framework](https://www.finops.org/framework/)

## 記録するもの

- 期待した事業成果と実績
- 出力品質と不合格数
- 人が確認・修正した時間
- 入力・出力token
- model、service、infrastructureの費用
- 実行回数、retry、失敗、停止
- 作業一件あたりの費用と時間

## 次の4分岐

1. 継続: 効果、品質、費用、riskが許容範囲にある。
2. 改善: model、指示、data、tool、頻度の一要素だけを変えて再測定する。
3. Script化: 判断が不要でruleが安定した処理を、定期実行するプログラムへ移す。
4. 停止: 効果不足、検証不能、risk過大、費用超過のいずれかに該当する。

Google CloudのAI/ML cost guidanceも、AI施策を事業目標とKPIへ結び付け、費用と事業価値のownerを置き、反復的に測定・改善することを推奨しています。Google固有の製品は例であり、本Stageの必須条件ではありません。[Google Cloud AI/ML cost optimization](https://docs.cloud.google.com/architecture/framework/perspectives/ai-ml/cost-optimization)

## 次へ進む条件

Stage 6は終点ではありません。[評価ワークシート](../worksheets/evaluation.md)で4分岐のいずれかを選び、必要なStageへ戻ります。

## 次に行うこと

直近一件について、得られた成果、人の確認時間、AI/service費を記録し、4分岐を一つ選びます。

## Evidence（旧形式・再調査中）

| claim_id | claim | source_url | source_type | checked_at | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|
| claim-stage6-001 | Technology費用を事業価値に結び付け、dataに基づく判断と部門横断の財務責任を持つ | https://www.finops.org/framework/ | official | 2026-09-03 | technology-cost, stage-6 | FinOps Frameworkの直接的な内容。4分岐はroadmap recommendation | finops-framework-change |
| claim-stage6-002 | AI/MLの技術判断を事業目標・KPIへ結び付け、費用と価値のownerを置いて継続的に最適化する | https://docs.cloud.google.com/architecture/framework/perspectives/ai-ml/cost-optimization | official | 2026-09-03 | ai-ml, stage-6 | Google公式guideの直接的な原則。provider-neutral層には原則だけを採用 | ai-cost-guidance-change |

## 関連

- [ロードマップ全体](../roadmap.md)
- [評価ワークシート](../worksheets/evaluation.md)
