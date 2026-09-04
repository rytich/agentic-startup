# エージェント組織の将来構想ケーススタディ設計

## 実施日

2026-09-03

## 実施内容

- 製品一覧でなく成果・統制マトリクスを正本にする方針を決定した。
- 将来構想を中心に、現在、計画、候補、block、非該当を分離する状態モデルを定義した。
- macOSとWindowsの同等性を、製品一致でなく成果と統制の達成で判定する方針を定義した。
- 初期12成果と3scenarioを定義した。
- 名刺交換からSFA、情報収集、提案、日程調整、mail下書き、承認、送信、SFA反映までのmulti-agent workflowをScenario Bとした。
- 実SFA更新と外部mail送信は実施せず、人間承認を初期要件とした。

## 未実施

- macOS実機のread-only確認
- Windows公式手段の調査
- Outcome Matrixとscenario本文の作成
- Peitho deckの作成

## 2026-09-04 レビュー差分

- 非公開の共有資料は、論点と表現を補強する内部参考に限定した。
- 内部参考の資料名、ページ、URL、引用箇所、source IDを、出典・参考文献・evidence tableへ記録しない契約を追加した。
- 英語圏を中心に、原著論文、標準、providerの公式技術資料を調査した。
- task単位の適用判断、Script・workflow・agent・人の選択、multi-agentの失敗分類、costと成果の同時評価を設計へ追加した。
- 主対象、重要成果、役割方針、platform、費用、source方針の変更影響マップを追加した。
- 変更後の設計は人間の再レビュー待ちとして`draft`へ戻した。

## 2026-09-04 外部source方針の再変更

- 共有PDF以外の外部情報は、引用・出典表示を許可した。
- 外部情報は、調査時点で公開または実質更新から6か月未満のものだけを対象とした。
- 主要事実は可能な限り一次情報で確認し、採用・推奨claimには独立したWeb・SNS上の実利用評価を必須にした。
- Web・SNS評価では、肯定だけでなく否定、失敗、制約、運用負荷を確認し、単純な反応数だけで判断しない契約を追加した。
- 旧基準の外部調査は`superseded`とし、旧claim IDとURLを現在の調査文書から除外した。
- 旧形式の20claimを有効なevidenceから外し、7 Stage、1 audience profile、4 worksheetの計12ファイルを`active`から`draft`へ戻した。
- ロードマップ入口と全体roadmapの`source_ids`も空にし、新基準での再調査待ちを明示した。
- ロードマップ設計仕様も`approved`から`draft`へ戻し、旧Source優先順位とEvidence形式を新方針への参照に置き換えた。

## 成果物

- [エージェント組織の将来構想ケーススタディ 設計](../superpowers/specs/2026-09-03-agentic-organization-case-study-design.md)
- [エージェント組織資料の外部根拠調査](../planning/research/2026-09-04-agentic-organization-evidence-research.md)
- [外部情報の選定方針](../decisions/2026-09-04-external-source-selection-policy.md)

## 2026-09-04 料金比較・実行経路・小規模統制の追加設計

- AI serviceのsubscription料金とAPI・token料金を別表で管理する方針を決定した。
- 初期subscriptionはBusiness Insider Japanの2026年9月版が扱う8 service、APIはOpenAI、Anthropic、Google、xAI、Perplexityとした。
- 新規候補は自動探索せず、人間から共有された場合だけ監視対象へ追加する。
- 公式sourceを3日ごとにread-only確認し、変更なしではmonitor stateと鮮度表示だけをbot更新し、料金・条件の変更はdraft PRへ出して人間承認前に公開値を変更しない設計とした。
- 継続更新される公式料金pageは、`observed_at`、market、通貨・単位、取得値、fingerprintを伴う`official-live`として扱うようSource方針とEvidence契約を補強した。
- API、CLI、MCPを構造化経路、Computer UseをGUI操作経路として比較し、安定処理は構造化経路、未接続部分と例外だけComputer Useを使う方針を追加した。
- 「小規模なら安全」とせず、対象業務、data、identity、接続先、権限、承認、log、停止手段を限定した「小さく統制されたAI組織」の利点として説明する方針を追加した。
- HackerOneとAkamaiの2026年調査は規模拡大、test範囲、Shadow AI可視性の根拠に使い、小規模統制の利点そのものは本設計の推論として分離した。
- Reddit上の2026年8月の実務者議論を独立したWeb評価として追加し、匿名投稿のため一般化せず`mixed`として扱った。
- 影響範囲として、ロードマップ設計、case study設計、Source方針、Evidence契約、将来のPeitho deckを更新対象にした。

### 追加成果物

- [AIサービス料金比較・鮮度監視 設計](../superpowers/specs/2026-09-04-ai-service-pricing-monitor-design.md)
