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
review_triggers: [evidence-contract-change, source-freshness-window-change, web-reception-change]
---

# ロードマップのEvidence契約

## Sourceの採用条件

次の条件をすべて満たす情報だけを有効なevidenceとして扱います。

1. 調査時点で、公開または実質更新から6か月未満である。
2. 主要な事実は、可能な限り一次情報で確認している。
3. 採用、推奨、実用性に関する主要claimは、独立したWeb・SNS上の実利用評価を一件以上伴う。
4. 一次情報の事実と、実利用者の評価と、本資料の推論を分けている。

技術仕様は公式specification、release note、repository、provider documentation、研究結果は原著論文や著者の正式公開物、事例は利用者・実装者・運用者の原文を優先します。二次記事は探索の入口に限り、元情報へ遡ります。

`checked_at`や検索engineのcrawl日はsourceの鮮度ではありません。公開日・実質更新日を確認できないsource、検索結果、無関係なtop page、AIの回答、開けないURLは採用しません。

Web・SNS評価は、GitHub Issues / Discussions、技術forum、実装記事、X、LinkedIn、Reddit、Hacker Newsなどの原投稿を対象にします。肯定だけでなく否定、失敗、制約、運用負荷を探し、like数やview数だけで判断しません。provider自身の宣伝投稿は独立評価に数えません。

詳細な判定規則は[外部情報の選定方針](../../../../decisions/2026-09-04-external-source-selection-policy.md)を正本とします。

## 非公開の内部参考

人間から共有された非公開資料は、論点の発見と説明表現の改善に限って内部参考として扱います。資料名、ページ、URL、引用箇所、source IDを、出典・参考文献・evidence tableへ記録しません。

内部参考から着想した内容は、外部の一次資料で独立して確認できる場合だけ、その外部sourceを根拠として採用します。独立した根拠がない内容は、事実ではなく本資料の推奨または設計方針と明示します。固有の文章、見出し順、段階名、図表構成、画面例は転用しません。

## 必須形式

各Stageは本文の主張に近い場所へリンクを置き、末尾に次の項目を持つtableを置きます。

| field | 内容 |
|---|---|
| `claim_id` | 一意な主張ID |
| `claim` | 資料で述べる事実または推奨 |
| `primary_source_url` | 主張を直接確認できる一次情報のURL |
| `source_type` | `official`、`standard`、`paper`、`case` |
| `source_published_or_updated_at` | sourceに表示された公開日または実質更新日 |
| `checked_at` | 調査日 |
| `reception_urls` | 投稿から6か月未満の独立したWeb・SNS上の実利用評価URL |
| `reception_published_at` | 実利用評価の公開日 |
| `reception_signal` | `positive`、`mixed`、`negative`、`insufficient`、`not-applicable` |
| `reception_summary` | 利用環境、結果、失敗、制約を含む短い要約 |
| `applies_to` | 対象Stage、対象者、platform、version |
| `interpretation` | 一次情報の事実、実利用評価、本資料の推論の区別 |
| `review_trigger` | 再調査する条件 |

純粋な技術仕様のclaimでは`reception_signal: not-applicable`を許容します。ただし、その仕様を根拠に採用・推奨するclaimではありません。実利用評価が不足する採用候補は`reception_signal: insufficient`とし、`candidate`から進めません。

## 完了条件

公開・実質更新から6か月未満という鮮度を満たさない、sourceの日付が不明、主要事実の一次情報がない、採用・推奨claimに独立した実利用評価がない、URLと主張の対応が不明、推論を事実として書いている、内部参考だけで一般化している場合は`draft`のままとします。Peitho deckも正本の`claim_id`、一次情報URL、実利用評価URL、鮮度情報を引き継ぎます。

2026-09-04より前の形式で記録したclaimは、新しい鮮度・実利用評価項目を満たすまで有効なevidenceとして扱いません。

## 関連

- [ロードマップの入口](../README.md)
- [外部情報の選定方針](../../../../decisions/2026-09-04-external-source-selection-policy.md)
