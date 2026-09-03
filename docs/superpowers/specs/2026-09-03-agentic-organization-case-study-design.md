---
title: エージェント組織の将来構想ケーススタディ 設計
status: approved
updated: 2026-09-03
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-business-outcome-first, principle-platform-independent, principle-human-accountability]
source_ids: []
derived_artifacts: []
review_triggers: [roadmap-outcome-change, platform-capability-change, case-study-status-change]
---

# エージェント組織の将来構想ケーススタディ 設計

## 1. 目的

市江氏が目指すエージェント組織の将来構想を、製品一覧ではなく「事業上の成果と統制」の設計図として示す。macOS は現在の実現例、Windows は同じ成果を実現する代替候補として扱う。

同じ製品、画面、操作方法を揃えることを再現とは呼ばない。クラウド同期、遠隔操作、agent連携、権限分割、費用最適化など、定義した成果と統制を同等に達成できることを再現とする。

## 2. 対象者と到達点

主対象は [`audience-business-leader-ai-user`](../../knowledge/materials/agentic-organization-roadmap/audiences/business-leader-ai-user.md) とする。読者はケーススタディを通じて次を理解できる状態を目指す。

- エージェント組織がどの事業成果を支えるか。
- 人、AI、Scriptがどこで役割を分けるか。
- data、権限、承認、費用をどこで統制するか。
- macOS固有手段とplatform-neutralな成果の違い。
- 自分の環境で次に検証する一項目。

## 3. 設計原則

1. **成果を正本にする。** OS、model、provider、connectorは成果を実現する手段である。
2. **将来構想と現在を混ぜない。** 採用方針、候補、検証済みを状態で区別する。
3. **同等性を成果で判定する。** Windowsで製品が異なっても、達成条件と統制を満たせば同等とする。
4. **業務の終点まで追跡する。** agentが個別taskを終えたことではなく、人間承認を含む事業成果まで状態を追う。
5. **外部情報に根拠を付ける。** 出典、確認日、不確実性を引き継ぐ。
6. **高影響操作は人が判断する。** 外部送信、SFA確定更新、削除、購入、権限変更を初期版では自動実行しない。
7. **機密情報を資料へ含めない。** 実顧客、名刺、credential、個人情報、非公開dataを使わない。

## 4. 成果物構成

```text
docs/knowledge/materials/agentic-organization-case-study/
├── README.md
├── future-architecture.md
├── outcome-matrix.md
├── scenarios/
│   ├── remote-delegation.md
│   ├── multi-agent-sales-workflow.md
│   └── failure-and-recovery.md
├── macos/
│   └── current-and-planned.md
├── windows/
│   └── parity-candidates.md
└── evidence/
    └── README.md
```

### 4.1 README

読む順序、状態定義、正本ロードマップとの境界、更新方法を示す。

### 4.2 Future Architecture

将来構想を成果ID、project、agent、人間承認、data flowで表現する。macOSのapplication構成を全体architectureと同一視しない。

### 4.3 Outcome Matrix

各成果について、事業目的、達成条件、人間の判断、data・権限、将来構想、macOS実現例、Windows代替候補、状態、evidence、未達時の影響、次の検証を管理する。

### 4.4 Scenarios

複数成果が一つの業務でどう連携するかを示す。架空または匿名化したdataだけを使用する。

### 4.5 Platform Examples

macOSとWindowsの実現方法を、同じ成果IDと達成条件へ対応付ける。製品比較を正本にしない。

## 5. 状態モデル

状態は成果全体ではなく、構成要素または検証項目ごとに付ける。

| 状態 | 定義 | 必須evidence |
|---|---|---|
| `verified-current` | 現在の実機・serviceで観測可能な終点まで確認済み | 確認日、環境、実行内容、観測結果 |
| `planned` | 採用方針は決定済みだが未実装 | 決定理由、依存、次の検証 |
| `candidate` | 調査・比較対象で採用未決定 | 公式source、適用条件、不確実性 |
| `blocked` | 権限、環境、人間判断がなく検証不能 | blocker、解除条件、owner |
| `not-applicable` | そのOS・構成では使用しない | 理由、同じ成果を担う代替手段 |

設定fileの存在、commandのexit 0、applicationの起動だけでは、業務scenarioを`verified-current`にしない。

## 6. 重要成果

| ID | 重要成果 | 達成条件 |
|---|---|---|
| `OUT-01` | dataのcloud同期 | 必要な端末・担当者から同じ正本へ安全にaccessできる |
| `OUT-02` | 遠隔からの常時操作 | 場所を問わずagentへ指示し、結果と承認待ちを確認できる |
| `OUT-03` | model・agentの交換可能性 | 新model導入を全体再構築でなく選択設定の変更として扱える |
| `OUT-04` | CLI/MCP/API中心の接続 | 特定UIやconnectorだけに業務を閉じず接続方法を交換できる |
| `OUT-05` | project集中管理 | 目的、状態、担当、承認待ち、成果を横断把握できる |
| `OUT-06` | data・権限・担当の分離 | project間の情報混在と越権を防げる |
| `OUT-07` | 人・AI・Scriptの役割最適化 | 判断、曖昧な処理、定型処理を適切に割り当てられる |
| `OUT-08` | 品質・費用・tokenの最適化 | 事業成果と費用を比較し継続・改善・停止を選べる |
| `OUT-09` | 障害時の継続・復旧 | 端末、provider、接続先の障害時に停止・復旧・代替を選べる |
| `OUT-10` | 複数agent間の引き継ぎ | 入力、出力、責任、状態を明示して連携できる |
| `OUT-11` | 人間承認を含む業務完結 | 自動処理と承認待ちを分断せず最終成果まで追跡できる |
| `OUT-12` | 外部情報の根拠管理 | 収集情報に出典、確認日、不確実性を付けられる |

## 7. 成果の追加条件

新しい成果は次を満たす場合に追加する。

- 事業成果または継続運用へ直接影響する。
- macOSとWindowsで達成方法を比較できる。
- 人間の判断、data、権限、費用のいずれかに新しい設計判断が生じる。
- 既存成果の下位手段でなく、独立して達成状態を評価できる。

追加時は既存成果との重複、影響するscenario、正本、Peitho deckを記録する。

## 8. Scenario A: 遠隔から一業務を委任する

```text
事業責任者が遠隔から指示
  -> 対象projectと権限を限定
  -> agentが正本dataを参照
  -> 作業を実行
  -> 結果・費用・操作履歴をcloudへ同期
  -> 高影響操作は人間の承認待ち
  -> 人間が継続・修正・停止を判断
```

主に`OUT-01`、`OUT-02`、`OUT-05`、`OUT-06`、`OUT-08`、`OUT-11`を検証する。

## 9. Scenario B: 名刺交換から商談打診まで連携する

```text
名刺交換・名刺data受領
  -> 顧客情報の抽出・重複確認
  -> 人間が登録対象と内容を確認
  -> SFAへ登録
  -> 企業・担当者・業界情報を収集
  -> 出典と確認日を付けて整理
  -> 課題仮説と提案候補を作成
  -> 社内予定と相手条件から候補日時を作成
  -> 打診mailの下書きを作成
  -> 人間が宛先・内容・根拠・候補日時を承認
  -> mail送信
  -> 送信結果と次回行動をSFAへ反映
```

### 9.1 役割

| 役割 | 担当 |
|---|---|
| Intake Agent | 名刺情報の抽出、形式整理 |
| CRM Agent | 重複確認、SFA登録候補の作成 |
| Research Agent | 企業・人物・業界情報の収集と出典管理 |
| Proposal Agent | 課題仮説、提案候補の作成 |
| Scheduling Agent | 候補日時と調整条件の整理 |
| Outreach Agent | 打診mailの下書き、送信後の記録 |
| Human Approver | 登録、提案、宛先、送信、例外の最終判断 |

主に`OUT-01`、`OUT-04`〜`OUT-08`、`OUT-10`〜`OUT-12`を検証する。

初期版ではSFA確定更新と外部送信に人間承認を必須とする。このcase study作成中は実SFA更新とmail送信を行わない。

## 10. Scenario C: 障害時に停止・復旧する

```text
異常を検知
  -> 自動処理とagent操作を安全に停止
  -> 影響するprojectとdataを確認
  -> 代替端末・model・接続方法を選択
  -> 正本から再開
  -> 未完了操作と重複実行を確認
  -> 原因と再発防止を記録
```

主に`OUT-01`、`OUT-03`、`OUT-04`、`OUT-06`、`OUT-09`〜`OUT-11`を検証する。

## 11. Model切替の扱い

Model切替は独立scenarioにしない。`OUT-03`と`OUT-08`の検証手順として、同じ入力、完了条件、data、権限でquality、費用、所要時間、制約を比較し、採用、限定採用、却下を判断する。

## 12. 調査とdata flow

1. `OUT-01`〜`OUT-12`の達成条件を調査・確定する。
2. 将来構想を成果単位で配置する。
3. macOSの現在状態をread-onlyで確認する。
4. `verified-current`と`planned`を分離する。
5. Windowsの公式手段を調査する。
6. 成果単位でmacOSとWindowsを比較する。
7. 3scenarioへ成果IDを対応付ける。
8. 未達、`blocked`、次の検証を記録する。
9. 正本ロードマップと双方向linkする。
10. Peitho deckがclaim IDと成果IDを参照できるようにする。

## 13. Evidence契約

各主張は[ロードマップのEvidence契約](../../knowledge/materials/agentic-organization-roadmap/evidence/README.md)に従う。外部仕様は公式文書を優先し、直接URL、確認日、適用範囲、事実と推論の区別、review triggerを記録する。

MacOSの状態は、実機で確認した事実と将来構想を同じclaimで表現しない。Windows代替は公式に対応が確認できても、業務scenarioの終点まで実機確認していなければ`candidate`とする。

## 14. Error handling

- 検証対象が曖昧: 成果IDと達成条件を先に確定する。
- 認証・権限が不足: `blocked`とし、回避して検証済みとしない。
- 公式sourceがない: 二次情報と推論を分け、`candidate`のままにする。
- macOSだけで成立: 共通成果でなくmacOS実現例へ限定する。
- Windows候補が同じ統制を満たさない: 同等とせず差分とriskを記録する。
- scenarioの途中だけ成功: 確認範囲を記録し、scenario全体を`verified-current`にしない。

## 15. Quality gates

1. 全12成果に事業目的、達成条件、人間判断、data・権限、状態、evidence、次の検証がある。
2. macOSとWindowsの比較が製品名だけでなく成果・統制で行われている。
3. `verified-current`に確認日、環境、実行内容、観測結果がある。
4. `planned`、`candidate`、`blocked`を実装済みと表現していない。
5. 3scenarioが成果IDへ対応し、人間承認を含む終点を持つ。
6. 実顧客data、名刺、mail address、credential、secretを含まない。
7. 正本ロードマップと双方向linkされる。
8. 全主要主張にevidence URLと確認日がある。

## 16. 非目標

- 実顧客dataを使ったSFA登録やmail送信。
- Windows環境を未検証のまま再現済みとすること。
- macOS application一覧を標準architectureとして配布すること。
- 全provider、model、connectorを網羅すること。
- case study作成と同時に本番権限や認証設定を変更すること。

## 17. 完了判定

初版は、読者が12成果、将来構想、macOS例、Windows候補、3scenario、人間承認、未検証範囲を一つのmapから辿れ、次に検証する成果を一つ選べる状態を満たす。

## 18. 関連

- [エージェント組織導入ロードマップ](../../knowledge/materials/agentic-organization-roadmap/README.md)
- [ロードマップ設計仕様](2026-09-03-agentic-organization-roadmap-design.md)
