---
title: Stage 5 - 組織として運用する
status: active
updated: 2026-09-03
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-human-accountability, principle-platform-independent]
stage_ids: [stage-5]
platform_ids: [platform-neutral]
source_ids: [claim-stage5-001, claim-stage5-002, claim-stage5-003]
derived_artifacts: []
review_triggers: [nist-zero-trust-revision, nist-ai-rmf-revision, organization-boundary-change]
---

# Stage 5 - 組織として運用する

## このStageの目的

複数の課題やagentを、情報混在、越権、責任の空白を起こさず運用します。集中管理は全データを一か所へ無制限に共有することではありません。どのprojectが、どの目的、data、権限、担当、予算、成果を持つかを一覧できる状態です。

## Projectごとに分けるもの

- 目的と期待成果
- 使用するdataと正本
- 担当者、agent、最終責任者
- 許可する接続と権限
- 予算、実行頻度、期限
- 成果、検証結果、操作履歴
- incidentと停止状態

## 集中管理で集めるもの

- projectのindexと状態
- 人間承認を待っている操作
- 費用、上限、異常
- 失敗、incident、停止
- 次のreview日

Projectのdata本体は、目的と権限がないprojectへ自動共有しません。NIST Zero Trustは、端末やnetworkの場所だけで信頼せず、利用者、device、resourceへの認証・認可を個別に行い、resourceを保護する考え方を示しています。[NIST SP 800-207](https://csrc.nist.gov/pubs/sp/800/207/final)

## 次へ進む条件

- projectの目的、data、権限、責任者を分離して説明できる。
- 集中管理画面や台帳が、必要以上のdataを共有しない。
- 承認待ち、費用超過、incident、停止を一覧できる。

## 次に行うこと

現在のAI活用をproject単位で一行ずつ一覧にし、目的と最終責任者が空欄のものを一つ確認します。

## Evidence

| claim_id | claim | source_url | source_type | checked_at | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|
| claim-stage5-001 | AI systemのinventory、役割、責任、継続的reviewを組織として管理する | https://airc.nist.gov/airmf-resources/airmf/5-sec-core/ | public | 2026-09-03 | platform-neutral, stage-5 | NIST Governの直接的な内容をproject台帳へ具体化 | nist-ai-rmf-revision |
| claim-stage5-002 | Networkや端末の場所だけで暗黙に信頼せず、利用者・device・resourceへの認証と認可を行う | https://csrc.nist.gov/pubs/sp/800/207/final | public | 2026-09-03 | remote-access, cloud, stage-5 | NIST Zero Trustの直接的な内容 | nist-zero-trust-revision |
| claim-stage5-003 | 集中管理はindex、承認待ち、費用、incidentを集め、data本体はproject境界を維持する | https://csrc.nist.gov/pubs/sp/800/207/final | public | 2026-09-03 | platform-neutral, stage-5 | Zero Trustのresource中心設計から導いたroadmap recommendation | organization-boundary-change |

## 関連

- [ロードマップ全体](../roadmap.md)
