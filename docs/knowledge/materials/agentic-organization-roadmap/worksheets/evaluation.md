---
title: Worksheet - 効果・品質・費用の評価
status: draft
updated: 2026-09-05
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-business-outcome-first, principle-small-scope-learning-loop]
stage_ids: [stage-6]
platform_ids: [platform-neutral]
source_ids: [source-microsoft-wti-2026-05-05, source-google-delegation-2026-08-22, source-span-agent-effectiveness-2026-07]
derived_artifacts: []
review_triggers: [management-loop-change, source-age-6-months, stage-6-change, source-policy-change]
---

# Worksheet - 効果・品質・費用の評価

## 記録

| 項目 | 期待 | 実績 | 確認方法 |
|---|---|---|---|
| 事業成果 |  |  |  |
| 出力品質 |  |  |  |
| 人の確認・修正時間 |  |  |  |
| token |  |  |  |
| service・実行費 |  |  |  |
| retry・失敗 |  |  |  |
| risk・incident |  |  |  |

## 判断

- [ ] 継続: 現在の条件で続ける。
- [ ] 改善: 変更する一要素は `model / 指示 / data / tool / 頻度` のうち（ ）。
- [ ] Script化: 判断不要でruleが安定した処理は（ ）。
- [ ] 停止: 理由は `効果不足 / 検証不能 / risk過大 / 費用超過 / その他` のうち（ ）。

- 判断日:
- 最終責任者:
- 次回review日:
- 次に戻るStage:

## 判断から次の依頼へ

- 次に解く一つの課題:
- `What`（一作業と成果物）:
- `Why`（今回の結果とのつながり）:
- `How`（data・tool・権限・禁止事項・承認）:
- `Done`（観測可能な完了条件）:
- 次の依頼をする人:
- 次の結果を判断する人:

次の依頼が作れない場合は、modelを変える前に、背景、質問、判断理由、不足情報のどれが欠けているかを確認します。

## Evidence

| claim_id | claim | primary_source_url | source_type | source_published_or_updated_at | observed_at | source_fingerprint | checked_at | reception_urls | reception_published_at | reception_signal | reception_summary | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| claim-management-loop-001 | 人間がAIの出力を判断し、学びを次のworkflowへ反映する | https://www.microsoft.com/en-us/worklab/work-trend-index/agents-human-agency-and-the-opportunity-for-every-organization | official | 2026-05-05 | — | — | 2026-09-05 | https://www.span.app/research/agent-effectiveness-july2026 | 2026-07 | positive | feedback loopとquality stewardshipが少ないreview手戻りと関連。ただし因果関係は未証明 | worksheet-evaluation, audience-business-leader-ai-user | 判断から次の依頼へ戻す欄は本資料の推論 | management-loop-change, source-age-6-months |

## 関連

- [Stage 6](../stages/06-optimize.md)
- [一作業の委任定義](delegation.md)
- [マネジメントループ調査](../../../../planning/research/2026-09-05-agent-management-loop.md)
