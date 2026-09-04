---
title: Stage 1 - 判断を補助させる
status: draft
updated: 2026-09-04
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-business-outcome-first, principle-human-accountability, principle-small-scope-learning-loop]
stage_ids: [stage-1]
platform_ids: [platform-neutral]
source_ids: []
derived_artifacts: [worksheet-problem-framing]
review_triggers: [audience-profile-change, oecd-ai-principles-change, nist-ai-rmf-revision, source-policy-change]
---

# Stage 1 - 判断を補助させる

## このStageの目的

AI に判断を丸ごと任せるのではなく、課題、選択肢、根拠、能力と限界を整理させ、人間が採用、保留、却下を説明できる状態にします。OECD は、AI の能力・限界や判断に関係する情報を平易に示し、影響を受ける人が結果へ異議を示せることを透明性の原則に含めています。[OECD AI Principle](https://oecd.ai/en/dashboards/ai-principles/P7)

## 判断の3分岐

- 採用: 期待成果、確認方法、費用、risk、責任者が明確で、小さく試せる。
- 保留: 重要な情報や判断基準が不足している。追加調査の問いを一つ決める。
- 却下: 事業価値がない、結果を確認できない、risk が許容範囲を超える、または責任者がいない。

## AIへ依頼すること

1. 課題を一文に言い換える。
2. 前提、分かっていること、不明点を分ける。
3. AI を使わない案を含む選択肢を出す。
4. 各案の期待効果、費用、risk、確認方法を比較する。
5. 根拠となる情報と推論を分ける。

AI の出力は判断材料です。最終責任者が、[課題整理ワークシート](../worksheets/problem-framing.md)を使って採否を決めます。NIST AI RMF は、AI の知識限界と人間による監督方法を文書化し、利用者が次の行動を判断できる情報を持つことを求めています。[NIST AI RMF Core](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/)

## 次へ進む条件

- 期待成果と対象外を一つずつ言える。
- 採用、保留、却下の理由を説明できる。
- 最終責任者と、次に確認する一項目が明確である。

## 次に行うこと

課題整理ワークシートの「期待成果」を一つだけ書き、採否を選びます。

## Evidence（旧形式・再調査中）

| claim_id | claim | source_url | source_type | checked_at | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|
| claim-stage1-001 | AI の能力・限界、入力、判断に関係する情報を平易に示し、結果を理解・異議申立て可能にすることが重要 | https://oecd.ai/en/dashboards/ai-principles/P7 | public | 2026-09-03 | audience-business-leader-ai-user, stage-1 | OECD の直接的な原則。採用・保留・却下の3分岐は roadmap recommendation | oecd-ai-principles-change |
| claim-stage1-002 | AI の知識限界、出力の利用方法、人間の監督を文書化し、関係者の判断を支える | https://airc.nist.gov/airmf-resources/airmf/5-sec-core/ | public | 2026-09-03 | platform-neutral, stage-1 | NIST Map 2.2 と Map 3.5 の直接的な内容を平易に要約 | nist-ai-rmf-revision |

## 関連

- [ロードマップ全体](../roadmap.md)
- [課題整理ワークシート](../worksheets/problem-framing.md)
