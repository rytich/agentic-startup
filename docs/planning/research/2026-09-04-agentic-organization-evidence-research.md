---
title: エージェント組織資料の外部根拠調査（旧基準）
status: superseded
updated: 2026-09-04
superseded_by: ../../decisions/2026-09-04-external-source-selection-policy.md
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-business-outcome-first, principle-platform-independent, principle-human-accountability]
source_ids: []
derived_artifacts: []
review_triggers: [source-policy-change]
---

# エージェント組織資料の外部根拠調査（旧基準）

## 現在の扱い

この調査は、[外部情報の選定方針](../../decisions/2026-09-04-external-source-selection-policy.md)を決定する前の基準で実施したため、現在の主要主張、推奨、Peitho deckのevidenceとして使用しません。

旧調査には、次の不足がありました。

- 調査時点で公開・更新から6か月以上経過した情報を含んでいた。
- `checked_at`と、source自体の公開・実質更新日を分けていなかった。
- 一次情報の権威性を中心に評価し、独立したWeb・SNS上の実利用評価を必須にしていなかった。
- 肯定、否定、失敗、制約、運用負荷を共通形式で比較していなかった。

旧claim IDとURLは現在の文書から除外しました。次回調査では、同じ結論を引き継がず、新しいEvidence契約に従ってゼロから候補を選びます。

## 再調査の完了条件

1. 調査日時点で公開・更新から6か月未満の一次情報だけを候補にする。
2. sourceの公開日または実質更新日を確認する。
3. 採用・推奨claimごとに、投稿から6か月未満の独立した実利用評価を一件以上確認する。
4. Web・SNS評価を`positive`、`mixed`、`negative`、`insufficient`で整理する。
5. sourceの事実、利用者評価、本資料の推論を分ける。
6. 対象task、対象者、環境、未検証範囲を記録する。

## 関連

- [外部情報の選定方針](../../decisions/2026-09-04-external-source-selection-policy.md)
- [ロードマップのEvidence契約](../../knowledge/materials/agentic-organization-roadmap/evidence/README.md)
