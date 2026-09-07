---
title: エージェント組織導入ロードマップ全体
status: draft
updated: 2026-09-05
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-business-outcome-first, principle-platform-independent, principle-small-scope-learning-loop, principle-human-accountability]
stage_ids: [stage-0, stage-1, stage-2, stage-3, stage-4, stage-5, stage-6]
platform_ids: [platform-neutral]
source_ids: [source-microsoft-wti-2026-05-05, source-microsoft-goals-2026-04, source-google-delegation-2026-08-22, source-span-agent-effectiveness-2026-07, source-simonwillison-claude-code-2026-07-21]
derived_artifacts: [presentation-yukawa-closing-management-loop]
review_triggers: [roadmap-stage-change, management-loop-change, source-policy-change]
---

# エージェント組織導入ロードマップ全体

## まず全体を描き、小さく繰り返す

最初から完成したエージェント組織を作るのではありません。粗い全体設計図を持ち、その中から一つの課題を選び、結果を確認できる最小範囲で試します。

## 7つの領域

| 領域 | 決めること | 人間が判断すること |
|---|---|---|
| 事業目的 | 解決する課題と期待成果 | 取り組む価値があるか |
| 役割 | 人・AI・Script の担当 | 誰が最終責任を持つか |
| データ | 保存場所、入力元、正本 | AI に見せてよいか |
| 実行環境 | クラウド、端末、遠隔操作 | 継続運用できるか |
| 接続 | CLI、MCP、API、サービス連携 | 製品依存を許容できるか |
| 境界と統制 | プロジェクト、担当、権限、承認 | AI が自律実行してよいか |
| 効果と費用 | 成果、品質、token、定期処理 | 継続、改善、停止のどれか |

`token` は AI が文章を読み書きする際の処理量の単位です。ここでは費用だけでなく、結果の品質や人の確認時間と合わせて評価します。

## 成熟段階

| Stage | 到達状態 | 次へ進む条件 |
|---|---|---|
| [0. 全体を描く](stages/00-map.md) | 7領域を粗く可視化 | 今回扱う課題を一つ選べる |
| [1. 判断を補助させる](stages/01-decide.md) | AI と課題・選択肢を整理 | 採否の理由を説明できる |
| [2. 一作業を委任する](stages/02-delegate.md) | 限定した作業を依頼 | 結果の合否を判断できる |
| [3. 再現可能にする](stages/03-reproduce.md) | 別の人でも同じ流れを実行 | 同条件で結果を再現できる |
| [4. 役割を分ける](stages/04-separate-roles.md) | 人・AI・Script を分担 | 責任者と権限が明確 |
| [5. 組織として運用する](stages/05-operate.md) | 複数プロジェクトを集中管理 | 衝突・越権・情報混在を防ぐ |
| [6. 最適化する](stages/06-optimize.md) | 品質と費用を継続改善 | 継続・変更・停止を定期判断 |

各 Stage は調査と evidence review を経て作成しています。

## 結論: AI活用の本質はマネジメントループ

エージェント組織づくりの本質は、最新modelの知識を増やすことではなく、仕事を任せられる形にすることです。必要十分な背景と目的を伝え、何を確かめるための質問かを決め、結果を判断し、その判断から次の依頼を具体化します。

| 段階 | 人間が行うこと | 次へ渡すもの |
|---|---|---|
| 1. 背景と目的 | なぜ必要か、何を成果とするか、対象外は何かを伝える | 判断に必要なcontext |
| 2. 問い | 何を確かめれば次の判断ができるかを一つ決める | 調査・分析の質問 |
| 3. 判断 | 結果を事実、推論、不足情報に分け、採用・保留・却下を選ぶ | 判断理由と未解決事項 |
| 4. 次の依頼 | 判断に基づき、`What / Why / How / Done`を明示する | 検証可能な一作業 |

この循環は、良い人間のマネジメントと同じ構造です。ただしAIは組織の暗黙知や状況を自動では共有しません。権限、禁止事項、人間承認、費用・回数上限、完了条件は、人への依頼より明示します。

Microsoftの2026年調査は、高度なAI利用能力を、AIへ指示すること、出力を判断すること、そこから学ぶことの組み合わせとして扱い、明確な意図と仕事の設計を重視しています。[Microsoft 2026 Work Trend Index](https://www.microsoft.com/en-us/worklab/work-trend-index/agents-human-agency-and-the-opportunity-for-every-organization) Google Cloudは、委任を検証可能な作業へ分解し、渡す情報を正確・関連的・統制可能に保ち、主観判断が必要な箇所へ人間を配置する考え方を示しています。[How agents can delegate better](https://cloud.google.com/blog/products/ai-machine-learning/how-agents-can-delegate-better)

この4段階と「人間のマネジメントと同じ構造」という表現は、各sourceの直接的な文言ではなく、本ロードマップの対象者向けに再構成した推奨です。根拠の採否と制約は[マネジメントループ調査](../../../planning/research/2026-09-05-agent-management-loop.md)に記録しています。

## 現在地の表し方

各領域を `未着手`、`仮説あり`、`小規模に検証済み`、`再現可能`、`分担・権限設定済み`、`効果測定・改善中` のいずれかで表します。総合点や技術力の順位にはしません。

## この資料で使う技術用語

- AI provider: AI modelや関連serviceを提供する会社・組織。
- model: 入力を受け取り、文章や判断材料などを生成するAIの中核。
- CLI: 画面のボタンではなく、文字のcommandでcomputerへ指示する操作方法。
- Git: fileの変更履歴を保存し、以前の状態や変更理由を追跡する仕組み。
- API: service同士が決められた形式でdataや操作を受け渡す接続口。
- MCP: AIが外部のtoolやdata sourceを共通方式で利用するための接続規格。
- connector: 特定serviceと接続するための機能。UIやprovider固有の場合がある。
- orchestration: 複数のagent、作業、承認、dataの流れを全体として調整すること。
- Script: 判断済みのruleを、同じ手順で繰り返す小さなprogram。
- token: AIが文章を読み書きする際の処理量の単位。

## 次に行うこと

[7領域の初期設計図](worksheets/system-map.md)を一度だけ埋め、`今回検証` とする項目を一つ選びます。

最初の試行後は、[効果・品質・費用の評価](worksheets/evaluation.md)で結果を判断し、その判断から次の依頼を一つ作ります。

## Evidence（2026-09-05追加）

| claim_id | claim | primary_source_url | source_type | source_published_or_updated_at | observed_at | source_fingerprint | checked_at | reception_urls | reception_published_at | reception_signal | reception_summary | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| claim-management-loop-001 | AI活用では、明確な意図と成果を設定し、出力を判断・改善する人間の能力と管理環境が重要 | https://www.microsoft.com/en-us/worklab/work-trend-index/agents-human-agency-and-the-opportunity-for-every-organization | official | 2026-05-05 | — | — | 2026-09-05 | https://www.span.app/research/agent-effectiveness-july2026 | 2026-07 | positive | 103 engineering teamの観測研究で、明確な依頼、検証可能な環境、feedback loopが手戻り減少と関連。ただし因果関係は未証明 | audience-business-leader-ai-user, stage-1 to stage-6, platform-neutral | Microsoftの調査結果とSpanの実務観測。4段階のloopは本資料の推論 | management-loop-change, source-age-6-months |
| claim-management-loop-002 | 有効な委任では、作業を検証可能に分け、正確で関連する情報を渡し、必要箇所に人間判断を置く | https://cloud.google.com/blog/products/ai-machine-learning/how-agents-can-delegate-better | official | 2026-08-22 | — | — | 2026-09-05 | https://simonwillison.net/2026/Jul/21/cat-and-thariq/ | 2026-07-21 | mixed | Anthropic実務者への第三者インタビューでは、contextを増やす一方で硬い指示を減らし、誤解され得る表現を見直す必要性を指摘 | audience-business-leader-ai-user, stage-1, stage-2, platform-neutral | Google Cloudの原則と実務者評価。人間のマネジメントとの共通構造は本資料の推論で、AI固有の明示境界を併記 | management-loop-change, source-age-6-months |

## Evidence（旧形式・再調査中）

| claim_id | claim | source_url | source_type | checked_at | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|
| claim-stage0-001 | AI risk 管理は Govern、Map、Measure、Manage を継続的・反復的に適用し、組織の状況に合わせる | https://airc.nist.gov/airmf-resources/airmf/5-sec-core/ | public | 2026-09-03 | platform-neutral, stage-0 to stage-6 | NIST の直接的な内容。順序付き成熟段階は本ロードマップの推奨 | nist-ai-rmf-revision |
| claim-stage0-002 | AI の目的、事業価値、利用範囲、risk tolerance、期待効果と費用を文書化することが初期判断を支える | https://airc.nist.gov/airmf-resources/airmf/5-sec-core/ | public | 2026-09-03 | platform-neutral, stage-0 | NIST Map の直接的な内容を7領域へ再構成 | nist-ai-rmf-revision |

## 関連

- [ロードマップの入口](README.md)
- [対象者プロファイル](audiences/business-leader-ai-user.md)
- [湯川塾プレゼンの結論スライド](presentations/yukawa-juku-closing.md)
- [マネジメントループ調査](../../../planning/research/2026-09-05-agent-management-loop.md)
