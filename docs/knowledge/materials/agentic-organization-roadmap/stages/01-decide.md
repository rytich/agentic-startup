---
title: Stage 1 - 判断を補助させる
status: draft
updated: 2026-09-05
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-business-outcome-first, principle-human-accountability, principle-small-scope-learning-loop]
stage_ids: [stage-1]
platform_ids: [platform-neutral]
source_ids: [source-microsoft-wti-2026-05-05, source-microsoft-goals-2026-04, source-span-agent-effectiveness-2026-07]
derived_artifacts: [worksheet-problem-framing]
review_triggers: [audience-profile-change, management-loop-change, source-age-6-months, source-policy-change]
---

# Stage 1 - 判断を補助させる

## このStageの目的

AI に判断を丸ごと任せるのではなく、課題、選択肢、根拠、能力と限界を整理させ、人間が採用、保留、却下を説明できる状態にします。Microsoftの2026年調査でも、高度な利用者はAIの出力を最終回答ではなく出発点として扱い、方向設定、品質管理、結果の利用責任を人間に残しています。[Microsoft 2026 Work Trend Index](https://www.microsoft.com/en-us/worklab/work-trend-index/agents-human-agency-and-the-opportunity-for-every-organization)

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

AI の出力は判断材料です。最終責任者が、[課題整理ワークシート](../worksheets/problem-framing.md)を使って採否を決めます。

## 質問は目的から作る

「何を知りたいか」だけでなく、「その答えで何を判断するか」を先に決めます。AIへは、目的、判断する人、既知の事実、対象外、必要な根拠、回答後に選ぶ行動を渡します。情報量を増やすこと自体が目的ではありません。判断に必要なcontextだけを、不足なく渡します。

回答は次の三つに分けます。

- 事実: sourceや観測で確認できたこと
- 推論: 事実からAIまたは人間が導いた解釈
- 不足情報: 次の判断に必要だが、まだ確認できていないこと

Microsoft Researchは、人とAIが共有できる形で目的を明確化し、複数のtoolやcontextをまたいで目的を引き継ぎ、出力ではなく成果へ向かう評価を提案しています。[Goals as First-Class Abstractions in Human-AI Collaboration](https://www.microsoft.com/en-us/research/wp-content/uploads/2026/04/2026-goals-as-first-class-abstractions-in-human-ai-collaboration-AutomationXP26_paper_0982.pdf) 明確な依頼と検証可能な環境がagentの手戻り減少に関連するという実務観測もありますが、これは因果関係の証明ではありません。[Span Agent Effectiveness, July 2026](https://www.span.app/research/agent-effectiveness-july2026)

## 次へ進む条件

- 期待成果と対象外を一つずつ言える。
- 採用、保留、却下の理由を説明できる。
- 最終責任者と、次に確認する一項目が明確である。
- AIへの質問と、その回答で行う判断が一対一で対応している。

## 次に行うこと

課題整理ワークシートの「期待成果」を一つだけ書き、採否を選びます。

## Evidence

| claim_id | claim | primary_source_url | source_type | source_published_or_updated_at | observed_at | source_fingerprint | checked_at | reception_urls | reception_published_at | reception_signal | reception_summary | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| claim-management-loop-001 | AI活用では、明確な意図と成果を設定し、出力を判断・改善する人間の能力と管理環境が重要 | https://www.microsoft.com/en-us/worklab/work-trend-index/agents-human-agency-and-the-opportunity-for-every-organization | official | 2026-05-05 | — | — | 2026-09-05 | https://www.span.app/research/agent-effectiveness-july2026 | 2026-07 | positive | 103 engineering teamの観測研究で、明確な依頼、検証可能な環境、feedback loopが手戻り減少と関連。ただし因果関係は未証明 | audience-business-leader-ai-user, stage-1, platform-neutral | 質問を事実・推論・不足情報へ分ける構造は本資料の推論 | management-loop-change, source-age-6-months |

## 関連

- [ロードマップ全体](../roadmap.md)
- [課題整理ワークシート](../worksheets/problem-framing.md)
- [マネジメントループ調査](../../../../planning/research/2026-09-05-agent-management-loop.md)
