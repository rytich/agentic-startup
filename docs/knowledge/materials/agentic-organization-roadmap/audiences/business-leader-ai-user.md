---
title: 対象者プロファイル - AI利用経験のある事業責任者
status: active
updated: 2026-09-03
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-human-accountability]
stage_ids: [stage-0, stage-1, stage-2, stage-3, stage-4, stage-5, stage-6]
platform_ids: [platform-neutral]
source_ids: [claim-audience-001, claim-audience-002]
derived_artifacts: []
review_triggers: [audience-profile-change, oecd-ai-principles-change, nist-ai-rmf-revision]
---

# 対象者プロファイル - AI利用経験のある事業責任者

ID: `audience-business-leader-ai-user`

## 前提知識

- ChatGPT などの AI サービスを仕事で試した経験がある。
- 解決したい事業上の課題がある。
- CLI（文字でコンピューターへ指示する操作）、Git（変更履歴を管理する仕組み）、サーバー運用は未経験でもよい。

## 提供する支援

- AI の能力と限界、入力データ、判断理由を確認する観点。
- 大きな課題を、一度に結果を確認できる範囲へ絞る質問。
- 人、AI、決められた処理を繰り返す Script の役割分担。
- データ、権限、費用、失敗時の影響を見落とさない設計図。

## 期待する到達点

自分でシステムを実装することは必須ではありません。次を自分で行えることを到達点とします。

- 事業課題への AI 適用の採用、保留、却下を理由付きで判断する。
- AI または専門家へ、構築・改善する範囲と完了条件を指示する。
- 結果を確認し、継続、改善、停止、対象拡大を選ぶ。

## 対象外

- 全 AI モデルやサービスの機能を網羅すること。
- エンジニアと同じ実装能力を短期間で身につけること。
- AI の回答を根拠なく正しいものとして採用すること。

## 対象者を変更した場合の影響

前提知識、提供する支援、到達点のいずれかを変えた場合、`audience-business-leader-ai-user` を持つ Stage、ワークシート、ケーススタディ、Peitho deck を再確認します。技術者向けや現場担当者向けへ対象を変える場合は、既存 ID の意味を上書きせず新しい対象者 ID を作ります。

## Evidence

| claim_id | claim | source_url | source_type | checked_at | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|
| claim-audience-001 | 利用者が AI の能力・限界、入力や判断に関わる情報を平易に理解し、結果へ異議を示せる透明性が重要である | https://oecd.ai/en/dashboards/ai-principles/P7 | public | 2026-09-03 | audience-business-leader-ai-user | OECD が直接示す原則。この資料では判断補助の必須要素として採用 | oecd-ai-principles-change |
| claim-audience-002 | AI risk の役割と責任を明確にし、経営層が導入判断の責任を持つことが重要である | https://airc.nist.gov/airmf-resources/airmf/5-sec-core/ | public | 2026-09-03 | stage-0 to stage-6 | NIST Govern 2 の直接的な内容。非エンジニア向け到達点への具体化は本ロードマップの推奨 | nist-ai-rmf-revision |

## 関連

- [ロードマップの入口](../README.md)
