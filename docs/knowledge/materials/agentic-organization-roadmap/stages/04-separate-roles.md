---
title: Stage 4 - 人・AI・Scriptの役割を分ける
status: draft
updated: 2026-09-04
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-human-accountability, principle-small-scope-learning-loop]
stage_ids: [stage-4]
platform_ids: [platform-neutral]
source_ids: []
derived_artifacts: []
review_triggers: [nist-ai-rmf-revision, owasp-llm-top10-change, role-policy-change, source-policy-change]
---

# Stage 4 - 人・AI・Scriptの役割を分ける

## このStageの目的

再現できるようになった作業を、判断、AIが得意な曖昧な処理、決められた反復処理に分けます。

| 担当 | 向いている仕事 | 必ず明確にすること |
|---|---|---|
| 人 | 目的、採否、例外、高影響操作の承認 | 最終責任者と判断期限 |
| AI | 調査、比較、要約、下書き、曖昧な入力の整理 | 入力、権限、検証、停止条件 |
| Script | 条件と処理が決まった定期実行、集計、転記 | rule、実行頻度、失敗通知、保守担当 |

この割り当ては本ロードマップの推奨です。NIST AI RMFは、人とAIの役割・監督を区別して文書化し、責任を明確にすることを示しています。[NIST AI RMF Core](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/)

## 役割表

| 作業 | 実行担当 | 最終責任者 | 許可 | 禁止 | 人間承認 | 代替担当 |
|---|---|---|---|---|---|---|
|  |  |  |  |  |  |  |

権限は役割を実行するために必要な最小限にします。NISTは最小権限を、利用者やprocessに割り当てるaccessを担当作業に必要な範囲へ制限する原則と定義しています。[NIST least privilege](https://csrc.nist.gov/glossary/term/least_privilege)

## 次へ進む条件

- 各作業の実行担当と最終責任者が異なる場合も説明できる。
- AIとScriptの許可、禁止、承認条件が明確である。
- 担当不在や失敗時の代替経路がある。

## 次に行うこと

Stage 3で再現した作業を、判断、AI処理、定型処理の3つに色分けします。

## Evidence（旧形式・再調査中）

| claim_id | claim | source_url | source_type | checked_at | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|
| claim-stage4-001 | 人とAIの役割、責任、監督を区別して文書化する | https://airc.nist.gov/airmf-resources/airmf/5-sec-core/ | public | 2026-09-03 | platform-neutral, stage-4 | NIST Govern 2、3の直接的な内容 | nist-ai-rmf-revision |
| claim-stage4-002 | 各entityには担当機能に必要な最小限のsystem resourceとauthorizationだけを与える | https://csrc.nist.gov/glossary/term/least_privilege | public | 2026-09-03 | platform-neutral, stage-4 | NIST定義の直接的な内容 | nist-least-privilege-source-change |
| claim-stage4-003 | Agentの機能、権限、自律性を最小化し、高影響操作に人の承認を求める | https://genai.owasp.org/llmrisk/llm062025-excessive-agency/ | official | 2026-09-03 | agentic-ai, stage-4 | OWASP対策の直接的な内容 | owasp-llm-top10-change |

## 関連

- [ロードマップ全体](../roadmap.md)
