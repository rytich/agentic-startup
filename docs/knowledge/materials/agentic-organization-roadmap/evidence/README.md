---
title: ロードマップのEvidence契約
status: active
updated: 2026-09-04
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-human-accountability]
stage_ids: [stage-0, stage-1, stage-2, stage-3, stage-4, stage-5, stage-6]
platform_ids: [platform-neutral]
source_ids: []
derived_artifacts: []
review_triggers: [evidence-contract-change]
---

# ロードマップのEvidence契約

## Sourceの優先順位

1. 法令、規制当局、公的機関、標準化団体の一次情報
2. 製品、provider、OSS の公式文書と repository
3. 原著論文、公式事例、当事者の技術資料
4. 信頼できる解説、比較、報道

検索結果、無関係なトップページ、AI の回答、開けない URL は evidence として採用しません。変更されやすい料金、製品仕様、モデル、API、CLI は資料作成時に再確認します。

同等に適用可能なsourceがある場合は、英語圏を含む原著論文、公的機関、標準、providerの公式技術資料を優先します。特定taskの効果量を別の業務へ一般化せず、対象者、task、環境、研究上の制約を`interpretation`へ残します。

## 非公開の内部参考

人間から共有された非公開資料は、論点の発見と説明表現の改善に限って内部参考として扱います。資料名、ページ、URL、引用箇所、source IDを、出典・参考文献・evidence tableへ記録しません。

内部参考から着想した内容は、外部の一次資料で独立して確認できる場合だけ、その外部sourceを根拠として採用します。独立した根拠がない内容は、事実ではなく本資料の推奨または設計方針と明示します。固有の文章、見出し順、段階名、図表構成、画面例は転用しません。

## 必須形式

各 Stage は本文の主張に近い場所へリンクを置き、末尾に次の表を持ちます。

| claim_id | claim | source_url | source_type | checked_at | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|

- `claim_id`: 一意な主張 ID
- `claim`: 資料で述べる事実または推奨
- `source_url`: 主張を直接確認できる URL
- `source_type`: `official`、`public`、`standard`、`paper`、`case`、`secondary`
- `checked_at`: `YYYY-MM-DD`
- `applies_to`: 対象 Stage、対象者、platform、version
- `interpretation`: source が直接示す事実か、本資料の推論か
- `review_trigger`: 再調査する条件

## 完了条件

主要主張に URL がない、URL と主張の対応が不明、確認日がない、推論を事実として書いている、内部参考だけで一般化している場合は `draft` のままとします。Peitho deck も正本の `claim_id` と URL を引き継ぎます。

## 関連

- [ロードマップの入口](../README.md)
- [英語圏を中心とした外部根拠調査](../../../../planning/research/2026-09-04-agentic-organization-evidence-research.md)
