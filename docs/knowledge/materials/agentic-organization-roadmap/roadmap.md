---
title: エージェント組織導入ロードマップ全体
status: draft
updated: 2026-09-03
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-business-outcome-first, principle-platform-independent, principle-small-scope-learning-loop, principle-human-accountability]
stage_ids: [stage-0, stage-1, stage-2, stage-3, stage-4, stage-5, stage-6]
platform_ids: [platform-neutral]
source_ids: [claim-stage0-001, claim-stage0-002]
derived_artifacts: []
review_triggers: [roadmap-stage-change, nist-ai-rmf-revision]
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

## Evidence

| claim_id | claim | source_url | source_type | checked_at | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|
| claim-stage0-001 | AI risk 管理は Govern、Map、Measure、Manage を継続的・反復的に適用し、組織の状況に合わせる | https://airc.nist.gov/airmf-resources/airmf/5-sec-core/ | public | 2026-09-03 | platform-neutral, stage-0 to stage-6 | NIST の直接的な内容。順序付き成熟段階は本ロードマップの推奨 | nist-ai-rmf-revision |
| claim-stage0-002 | AI の目的、事業価値、利用範囲、risk tolerance、期待効果と費用を文書化することが初期判断を支える | https://airc.nist.gov/airmf-resources/airmf/5-sec-core/ | public | 2026-09-03 | platform-neutral, stage-0 | NIST Map の直接的な内容を7領域へ再構成 | nist-ai-rmf-revision |

## 関連

- [ロードマップの入口](README.md)
- [対象者プロファイル](audiences/business-leader-ai-user.md)
