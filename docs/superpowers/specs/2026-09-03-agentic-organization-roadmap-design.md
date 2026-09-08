---
title: 非エンジニア向けエージェント組織導入ロードマップ 設計
status: draft
updated: 2026-09-05
audience_ids:
  - audience-business-leader-ai-user
principle_ids:
  - principle-business-outcome-first
  - principle-platform-independent
  - principle-small-scope-learning-loop
  - principle-human-accountability
source_ids:
  - source-openai-models-live
  - source-hackerone-ai-security-gap-2026
  - source-akamai-enterprise-ai-risk-2026
  - source-reddit-shadow-ai-2026-08
  - source-microsoft-wti-2026-05-05
  - source-microsoft-goals-2026-04
  - source-google-delegation-2026-08-22
  - source-span-agent-effectiveness-2026-07
  - source-simonwillison-claude-code-2026-07-21
derived_artifacts: []
review_triggers: [target-audience-change, roadmap-stage-change, source-policy-change, source-freshness-window-change, web-reception-change, pricing-source-change, execution-path-change, shadow-ai-governance-change]
---

# 非エンジニア向けエージェント組織導入ロードマップ 設計

## 1. 目的

AI provider、model、service の変化を追うこと自体を目的にせず、事業課題へ継続的に活用するための全体設計図と、次の一歩を選ぶロードマップを提供する。

主対象は、ChatGPT などの AI service は使っているが、CLI、Git、server 運用の経験がない事業責任者とする。第一到達点は、対象者が自力で実装できることではない。解像度が低くてもエージェント組織の全体像を持ち、事業課題への適用、AI や専門家への構築・改善指示、結果の採否を自分で判断できる状態とする。

## 2. 背景

エンジニアや経営者の経験がある利用者は、AI の提案を暗黙に評価し、大きな課題を検証可能な範囲へ絞りながら改善を反復できる。一方、この判断力と課題分解力を前提にすると、経験のない利用者は全体像を失うか、個別 tool の操作だけを学んで止まりやすい。

本ロードマップは、暗黙の判断基準とスコープの絞り方を、設計図、質問票、checklist、段階別の移行条件として外部化する。全体設計図を先に持ちながら、実践では小さな成功ループを繰り返す。

## 3. 設計原則

1. **事業成果から始める。** model や tool の導入ではなく、解決したい事業課題と観測可能な成果を起点にする。
2. **全体像と次の一歩を両立する。** 先に粗い設計図を描き、その中から一度に検証する最小範囲を選ぶ。
3. **人間が責任を持つ。** AI は提案、実行、検証補助を担えるが、目的、権限、採否、継続、停止の判断は人間が持つ。
4. **人・AI・Script を使い分ける。** 判断を伴わない routine は可能な限り Script へ移し、AI token は不確実性の高い仕事へ使う。
5. **provider と connector への依存を抑える。** model 切り替えと CLI、MCP、API を優先し、特定 service の UI だけに運用を閉じない。APIがない画面や例外処理ではComputer Useを限定利用し、構造化経路との違いを明示する。
6. **platform 非依存を原則にする。** macOS は実例であり必須条件にしない。Windows などで同じ目的と統制を実現できれば同等と扱う。
7. **未検証を明示する。** 代替環境や効果を確認していない場合は候補として扱い、再現済みとは表現しない。
8. **小さく統制してから広げる。** 対象業務、data、identity、接続先、権限、承認、log、停止手段を限定した単位で成果と安全性を観測し、確認できた範囲だけ拡大する。

## 4. 資料体系

正本と派生成果物を次のように分ける。

```text
導入ロードマップ（正本）
├── なぜエージェント組織なのか
├── 全体設計図
├── 現在地診断
├── 成熟度別ロードマップ
├── 判断の型
├── 課題を狭める型
└── 次の一歩の選び方
    ├── ワークシート
    ├── 実現パターン集
    ├── ケーススタディ
    └── Peitho プレゼン
```

### 4.1 正本

導入ロードマップは、対象者、設計原則、全体設計図、成熟度、判断方法、課題分解、次の行動を定義する。個別 tool の操作方法や OS 固有手順を正本へ混ぜない。

### 4.2 ワークシート

- 事業課題整理
- 人・AI・Script の役割分担
- data、権限、承認境界
- 効果、費用、risk の評価

### 4.3 実現パターン集

- cloud での data 集中管理
- 遠隔からの agent 操作
- CLI、MCP、APIを優先し、必要箇所だけComputer Useを使う接続
- project、担当、権限の分割
- 小さく統制されたAI組織から段階的に拡大する方法
- Script による定期処理
- 3日ごとに公式sourceを確認するAI service料金比較
- agentic-framework を使った orchestration と集中管理

### 4.4 ケーススタディ

市江氏の macOS、Codex、Hermes、agentic-framework 環境を、原則を実現した一例として扱う。Windows の代替は、目的、必要な統制、検証状態を併記する。macOS 固有の実装を共通要件へ昇格させない。

### 4.5 Peitho プレゼン

Peitho は講義用の派生成果物に使う。Markdown と design の分離、section と発表時間、speaker notes、footnotes、include による共通 slide の再利用を活用する。プレゼン独自の設計判断を持たせず、各 deck は参照した正本 ID と更新時点を記録する。

## 5. 全体設計図

対象者は次の 7 領域を粗く描く。空欄をなくすことではなく、未決定、検証対象、現在は決めない項目を区別することを目的とする。

| 領域 | 決めること | 人間が判断すること |
|---|---|---|
| 事業目的 | 解決する課題と期待成果 | 取り組む価値があるか |
| 役割 | 人・AI・Script の担当 | 誰が最終責任を持つか |
| data | 保存場所、入力元、正本 | AI に見せてよい情報か |
| 実行環境 | cloud、端末、遠隔操作 | 継続運用できるか |
| 接続 | CLI、MCP、API、Computer Use、service 連携 | 構造化経路とUI操作をどこで使い分けるか |
| 境界と統制 | project、担当、権限、承認 | AI が自律実行してよい範囲か |
| 効果と費用 | 成果、品質、token、定期処理 | 継続、改善、停止のどれを選ぶか |

設計図は次の loop で更新する。

1. 7 領域を粗く埋める。
2. 不明点を未決定として可視化する。
3. 今回検証する領域を一つに絞る。
4. 小さな実行単位と観測可能な完了条件を決める。
5. 実行結果を観測する。
6. 設計図を更新する。
7. 継続、改善、停止、対象拡大から次の行動を選ぶ。

## 6. 判断と課題分解の補助線

### 6.1 判断の補助線

AI の提案は、少なくとも次の観点で採用、保留、却下を判断する。

- 期待する事業効果
- 結果を確認できる方法
- 導入と継続の費用
- 他者が再現できる程度
- 必要な data と権限
- 失敗時の影響と復旧方法
- 最終責任を持つ人間

### 6.2 課題分解の補助線

```text
事業上の困りごと
  -> 期待する成果を一つに絞る
  -> 人・AI・Script の担当を分ける
  -> data と権限を必要最小限にする
  -> 小さく実行し、観測可能な結果を得る
  -> 効果、費用、risk を人間が判断する
  -> 継続、改善、停止、対象拡大を選ぶ
```

対象者が自力で絞り込めない場合に備え、質問票と具体例で各段階を支援する。一度に複数の成果、部門、data source、権限変更を含む課題は分割候補とする。

## 7. 成熟度ロードマップ

成熟度は tool の導入数ではなく、委任範囲と検証可能性で判定する。

| 段階 | 到達状態 | 主な成果物 | 次へ進む条件 |
|---|---|---|---|
| 0. 全体を描く | 事業課題と 7 領域を粗く可視化 | 初期設計図 | 今回扱う課題を一つ選べる |
| 1. 判断を補助させる | AI と課題、選択肢を整理できる | 課題整理票 | 採否の理由を説明できる |
| 2. 一作業を委任する | 限定した作業を AI へ依頼できる | 委任定義、検証条件 | 結果を観測し合否判断できる |
| 3. 再現可能にする | 別の人でも同じ流れを実行できる | 手順、template、正本 | 同条件で結果を再現できる |
| 4. 役割を分ける | 人・AI・Script を分担できる | 役割表、承認境界 | 責任者と権限が明確になる |
| 5. 組織として運用する | 複数 project を集中管理できる | project 構成、運用規則 | 衝突、越権、情報混在を防げる |
| 6. 最適化する | 品質と費用を継続改善できる | 指標、実行履歴、改善判断 | 継続、変更、停止を定期判断できる |

診断結果は、各領域について `未着手`、`仮説あり`、`小規模に検証済み`、`再現可能`、`分担・権限設定済み`、`効果測定・改善中` の状態で表示する。単純な総合点や技術力の順位にはしない。

## 8. 失敗時の戻り先

| 観測した問題 | 戻る場所 | 見直すこと |
|---|---|---|
| AI の回答を判断できない | 段階 1 | 判断基準、根拠、専門家確認 |
| 課題が大きすぎる | 段階 0 | 期待成果と対象範囲 |
| 結果を検証できない | 段階 2 | 観測可能な完了条件 |
| 人によって再現しない | 段階 3 | 入力、手順、例外 |
| AI が越権する | 段階 4 | 権限、承認、停止条件 |
| 費用が見合わない | 段階 6 | model、Script 化、頻度、停止 |

失敗を成熟度の低下とは扱わない。設計図を更新するための観測結果として記録する。

## 9. 依存関係と変更影響の追跡

対象者や platform の方針変更時に影響範囲を検索できるよう、正本、実現パターン、case study、worksheet、deck に次の metadata を持たせる。

| Field | 用途 |
|---|---|
| `audience_ids` | 対象者 profile への依存 |
| `principle_ids` | 設計原則への依存 |
| `stage_ids` | 成熟段階との対応 |
| `platform_ids` | OS、実行環境への依存 |
| `source_ids` | 根拠となる正本 |
| `derived_artifacts` | slide、worksheet などの派生先 |
| `review_triggers` | 再確認を必要とする変更条件 |

初期 ID は次を使う。

- `audience-business-leader-ai-user`: AI service 利用経験はあるが CLI、Git、server 運用は未経験の事業責任者
- `platform-neutral`: 共通の原則、到達状態、評価方法
- `platform-macos-case-study`: 市江氏の現行実例
- `platform-windows-candidate`: Windows 代替候補。検証状態を別途明記

対象者 profile は、`前提知識`、`提供する支援`、`期待する到達点` を分離して定義する。profile の変更時は `audience_ids` を参照する全 artifact を再確認する。

## 10. 更新 data flow

```text
対象者・設計原則
  -> 全体設計図
  -> 成熟度ロードマップ
  -> worksheet / 実現パターン / case study
  -> Peitho deck
```

更新は上流から下流へ伝播させる。

- 対象者を変更したら `audience_ids` の参照先を再確認する。
- 設計原則を変更したら roadmap、pattern、deck の該当箇所を再確認する。
- macOS 環境の変更は case study だけに閉じ、原則と roadmap を暗黙に変更しない。
- Codex、Hermes、model、MCP の変更は実装例を更新する。共通原則を変える場合は別の意思決定として扱う。
- Computer Useの対象画面または権限を変更したら、誤操作、prompt injection、外部送信、停止条件を再確認する。
- AI serviceの料金・提供条件は[料金比較・鮮度監視設計](2026-09-04-ai-service-pricing-monitor-design.md)に従って確認し、未承認の観測値を正本へ反映しない。
- Peitho deck は正本 ID と更新時点を記録し、正本との不一致を検査対象にする。
- 質問・判断・次の依頼の構造を変えたら、`management-loop-change`を持つroadmap、Stage 1/2、worksheet、Peitho deckを再確認する。

## 11. 品質確認

1. **構造確認:** 必須章、metadata、ID、参照、link の欠落がない。
2. **対象者確認:** CLI や Git を知らない読者が、未説明の用語によって行動不能にならない。
3. **行動確認:** 各章を読んだ後、次に行う一項目を選べる。
4. **境界確認:** 未検証の代替手段や効果を、検証済みと表現していない。
5. **実地確認:** 湯川塾での質問、つまずき、判断不能箇所を記録し、正本へ反映する。

## 12. 調査と evidence の要件

各成熟段階、実現 pattern、case study、worksheet、Peitho deck は、作成前に対象 topic の調査を行う。経験則だけで一般化せず、読者が根拠を確認できる URL を掲載する。

### 12.1 調査の単位

各 step は次の順で作成する。

1. その step で対象者が判断する内容を列挙する。
2. 判断に必要な事実、推奨、事例、未確定事項を分ける。
3. 調査日から6暦月前の日付を算出し、公開・実質更新から6か月未満の一次情報を探す。
4. 採用・推奨候補ごとに、投稿から6か月未満の独立したWeb・SNS上の実利用評価を探す。
5. 肯定、否定、失敗、制約、運用負荷を整理する。
6. 一次情報の事実、実利用評価、本roadmapの推論を分ける。
7. 出典付きの本文、worksheet、slideを作成する。
8. sourceの日付、URL、確認日、対象version、実利用評価、未検証範囲をreviewする。

### 12.2 Sourceの選定

[外部情報の選定方針](../../decisions/2026-09-04-external-source-selection-policy.md)を正本とする。社会的な権威性だけで採用せず、現在の実装者・利用者による独立した評価を重視する。二次記事は探索の入口に限り、主要事実は可能な限り元のspecification、release note、repository、原著論文、原投稿へ遡る。公開日を示さず継続更新される公式料金pageは、`official-live`の例外と用途別鮮度期限を適用する。

共有PDFは内部参考に限り、資料名、ページ、URL、引用箇所、source IDを出典・参考文献・evidence tableへ記録しない。

### 12.3 Artifact に残す evidence

各artifactは、本文の該当箇所に近い位置で一次情報と実利用評価のURLを示し、[ロードマップのEvidence契約](../../knowledge/materials/agentic-organization-roadmap/evidence/README.md)が定めるfieldを持つevidence tableを置く。

### 12.4 完了 gate

sourceが公開・実質更新から6か月未満でない、公開日が確認できない、主要事実の一次情報がない、採用・推奨claimに独立した実利用評価がない、未検証の推論が事実として書かれている場合、そのstepは`draft`のままとする。Peitho deckも正本のclaim ID、一次情報URL、実利用評価URL、鮮度情報を引き継ぐ。

## 13. 初期提供順

1. 対象者 profile と ID 定義
2. 正本となる導入ロードマップ
3. 全体設計図と現在地診断 worksheet
4. 段階 0 から 2 の判断・課題分解 worksheet
5. 市江氏の macOS case study
6. Windows 代替候補の調査と再現性表示
7. AI service料金比較と3日ごとの鮮度監視
8. 湯川塾向け Peitho deck
9. 実地 feedback を反映する更新手順

## 14. 非目標

- 全 model、provider、AI service の網羅
- 非エンジニアだけで全環境を構築できることの保証
- macOS 固有構成の標準化
- 未検証の Windows 構成を再現可能と主張すること
- agentic-framework の全機能説明
- agent を増やすこと自体を成熟とみなすこと

## 15. 完了判定

初版は、主対象者が次を実行できる状態を満たす。

- 7 領域の全体設計図を粗く作成できる。
- 未決定事項と今回の検証範囲を区別できる。
- 一つの事業課題を、観測可能な一作業へ絞れる。
- 人、AI、Script の役割と人間の承認境界を説明できる。
- 実行結果から継続、改善、停止、対象拡大のいずれかを選べる。
- 自分で実装できなくても、AI または専門家へ次の構築・改善を指示できる。
- 必要十分な背景と目的を渡し、質問の結果を事実・推論・不足情報に分け、その判断から次の依頼を`What / Why / How / Done`で作れる。
- 各段階の主要主張について、対応する evidence URL と確認日を追跡できる。
- API、CLI、MCP、Computer Useの違いと選択理由を説明できる。
- 小さく統制されたAI組織と、無管理なShadow AIの違いを説明できる。
- subscriptionとAPI料金を分け、鮮度とsourceを確認して比較できる。

## 16. 参照

- [実装計画](../plans/2026-09-03-agentic-organization-roadmap.md)
- [Peitho Guide](https://peitho.gosu.ke/guide/)
- [Peitho Writing Decks](https://peitho.gosu.ke/guide/writing-decks/)
- [agentic-framework 概要](../../knowledge/materials/agentic-framework-overview.md)
- [AIサービス料金比較・鮮度監視 設計](2026-09-04-ai-service-pricing-monitor-design.md)
- [知識ベース構造](../../framework/knowledge-base.md)
