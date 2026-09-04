---
title: AIサービス料金比較・鮮度監視 設計
status: draft
updated: 2026-09-04
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-business-outcome-first, principle-platform-independent, principle-human-accountability]
stage_ids: [stage-3, stage-4, stage-6]
platform_ids: [platform-neutral]
source_ids:
  - source-bi-ai-pricing-2026-09
  - source-openai-pricing-live
  - source-claude-pricing-live
  - source-google-ai-pricing-live
  - source-openai-models-live
  - source-hackerone-ai-security-gap-2026
  - source-akamai-enterprise-ai-risk-2026
  - source-reddit-shadow-ai-2026-08
derived_artifacts:
  - ../../knowledge/materials/agentic-organization-roadmap/comparisons/subscriptions.md
  - ../../knowledge/materials/agentic-organization-roadmap/comparisons/api-pricing.md
review_triggers: [pricing-source-change, comparison-axis-change, freshness-policy-change, target-provider-change, execution-path-change]
---

# AIサービス料金比較・鮮度監視 設計

## 1. 目的

主要AIサービスの料金、利用条件、実行経路を、非エンジニアでも比較できる二つの表として提供する。値を記事へ直接転記して固定せず、公式sourceを3日に一度確認し、変更を人間が承認してから公開する。

比較表は最安値を選ぶためだけに使わない。実際の業務workflowに対する品質、所要時間、token量、権限、運用負荷と合わせて、採用、限定採用、継続、停止を判断する材料とする。

## 2. 確定した要件

1. 個人・法人向けsubscriptionと、API・token料金を別表にする。
2. 初期subscription対象は、Business Insider Japanの2026年9月版が扱うChatGPT、Gemini、Claude、Microsoft Copilot、Perplexity、Felo、Genspark、Grokとする。
3. 初期API対象はOpenAI、Anthropic、Google、xAI、Perplexityとする。
4. 対象登録済みの公式sourceを3日に一度確認する。
5. 変更はdraft PRにし、人間の承認前に公開値を変更しない。
6. 新規serviceを自動探索しない。人間から候補が共有された場合だけ監視対象へ追加する。
   登録済みproviderの公式料金pageに現れた新plan・新modelは変更候補としてdraftへ出すが、自動承認しない。
7. 取得不能、source間の矛盾、不自然な変更は推測せず、手動確認へ送る。
8. Computer Useを、API、CLI、MCPと並ぶ実行経路の比較軸にする。
9. 小規模であるだけで安全とは表現しない。対象、data、identity、権限、接続先、送信先、承認、log、停止手段が限定された「小さく統制されたAI組織」の利点を説明する。

## 3. 非目標

- 全AI service、model、providerの網羅。
- 新規serviceの自動発見、自動採用、自動公開。
- ログイン、決済、契約、無料trial開始による料金確認。
- providerの利用規約に反するscrapingやbot回避。
- 料金だけによるmodelまたはserviceの順位付け。
- 未公開の法人見積価格の推測。
- 為替換算値を実際の請求額として保証すること。

## 4. Architecture

構造化dataを正本にし、Markdown表とPeitho向け表を派生成果物にする。

```text
allowlist済み公式料金page / release note
  -> provider adapterで取得・正規化
  -> schema・単位・地域・金額を検証
  -> 前回の承認値と比較
      -> 変更なし: monitor stateの確認日時だけを更新
      -> 変更あり: observationとdraft PRを作成
      -> 取得不能・矛盾: manual-reviewを通知
  -> Human Approverがsourceと差分を確認
  -> approved dataを更新
  -> subscription表 / API表 / Peitho断片を再生成
```

### 4.1 Component

| component | 責務 | 依存 |
|---|---|---|
| provider registry | 対象service、market、allowlist URL、取得方式を定義 | 人間が追加した候補 |
| provider adapter | 公式sourceから必要項目だけを取得・正規化 | provider registry |
| schema validator | 通貨、単位、型、必須field、重複を検査 | 正規化data |
| diff engine | 承認値と観測値の意味差分を生成 | approved、observation |
| monitor state | sourceごとの最終成功日時、fingerprint、失敗状態を保持 | checker結果 |
| reviewer output | old/new、source、確認日時、警告を提示 | diff engine |
| renderer | 同一の承認値からMarkdownとPeitho用dataを生成 | approved data |
| scheduler | 3日ごとにread-only確認を起動 | GitHub Actions |

providerごとの差はadapterへ閉じ込める。共通schema、diff、rendererはprovider固有のHTML構造を知らない。

## 5. Repository構成

実装時は次の配置を正本とする。

```text
data/ai-service-pricing/
├── providers.yaml
├── approved.yaml
├── monitor-state.yaml
└── observations/
    └── 2026-09-04-provider-id.yaml

scripts/ai-service-pricing/
├── check.mjs
├── validate.mjs
├── diff.mjs
├── render.mjs
└── adapters/
    └── provider-id.mjs

docs/knowledge/materials/agentic-organization-roadmap/comparisons/
├── README.md
├── subscriptions.md
└── api-pricing.md
```

`observations/`は変更または異常を検知した場合だけ追加する。変更のない定期確認では、`monitor-state.yaml`の`last_successful_observed_at`、fingerprint、workflow run URLだけをbot commitで更新する。`approved.yaml`の料金・条件は変更しない。この小さな更新により、各行の鮮度をrepository上で再現できるようにする。

## 6. Source戦略

### 6.1 優先順位

1. providerの公式料金page。
2. providerの公式release note、料金変更告知、model lifecycle文書。
3. 地域別の公式購入画面。認証や購入操作が不要な範囲だけを確認する。
4. Business Insider Japanの比較記事は、比較軸、対象漏れ、公式sourceとの不一致を見つける二次情報として使う。料金値の正本にはしない。

公開日を表示しない公式料金pageは、継続更新される`official-live` sourceとして扱う。`observed_at`、`market`、取得値、`source_fingerprint`を記録すれば、「その日時に表示されていた現在値」の根拠にできる。ただし、変更日や過去の価格を証明する根拠には使わない。

### 6.2 初期監視対象

| 種別 | provider / service | 初期source | 初期取得方式 |
|---|---|---|---|
| subscription | ChatGPT | https://chatgpt.com/ja-JP/pricing/ | HTTP、必要時に手動確認 |
| subscription | Gemini | https://one.google.com/intl/ja_jp/about/google-ai-plans/ | HTTP、地域差を記録 |
| subscription | Claude | https://claude.com/ja/pricing | HTTP |
| subscription | Microsoft Copilot | https://www.microsoft.com/ja-jp/microsoft-365-copilot/pricing/individuals | HTTP |
| subscription | Perplexity | https://www.perplexity.ai/help-center/ | 公式記事を特定してHTTP |
| subscription | Felo | https://felo.ai/search | 料金表示を確認できなければ手動確認 |
| subscription | Genspark | https://genspark.ai/pricing | 認証へredirectされた場合は手動確認 |
| subscription | Grok | https://grok.com/plans | page本文を取得できなければ手動確認 |
| API | OpenAI | https://developers.openai.com/api/docs/models/compare | HTTP |
| API | Anthropic | https://platform.claude.com/docs/en/about-claude/pricing | HTTP |
| API | Google | https://ai.google.dev/gemini-api/docs/pricing | HTTP |
| API | xAI | https://docs.x.ai/developers/models | HTTP |
| API | Perplexity | https://docs.perplexity.ai/docs/getting-started/pricing | HTTP |

初期source URLは実装時にlive確認し、料金を直接示さないtop pageは、より直接的な公式sourceへ置き換える。対象serviceまたはproviderの追加は`providers.yaml`への人間承認済み変更だけで行う。登録済みprovider内の新plan・新modelは、既存sourceの意味差分として検知し、`candidate`のdraftへ出す。

## 7. Data model

### 7.1 共通field

| field | 内容 |
|---|---|
| `provider_id` | providerの安定ID |
| `product_id` | subscriptionまたはAPI productの安定ID |
| `market` | JP、globalなどの適用地域 |
| `currency` | 原価格の通貨 |
| `source_url` | 値を直接示す公式URL |
| `source_type` | `official-dated`または`official-live` |
| `source_published_or_updated_at` | sourceが明示する公開・実質更新日。live pageで不明ならnull |
| `observed_at` | 実際に値を確認した日時 |
| `effective_at` | 適用開始日。公式に示されない場合はnull |
| `source_fingerprint` | 価格・条件に関係する正規化断片のhash |
| `verification_status` | `verified`、`changed`、`unavailable`、`manual-review` |
| `approved_at` | 公開承認日時 |
| `approved_by` | 個人名でなく承認主体role |

### 7.2 Subscription field

- plan名、individual / business、月額、年額、月額換算。
- 税込・税別・不明、最低seat数、trial、一時campaign。
- 利用可能model、agent機能、data学習、admin、SSO、利用上限。
- Web / app UI、API、CLI、MCP、Computer Use、agent間連携の提供状態。

### 7.3 API field

- model ID、提供状態、廃止予定日、context上限。
- 100万token当たりのinput、output、cache write、cache read。
- batch、長文input、priority処理などの単価差。
- Web search、file search、Computer Useなどtool固有料金。
- rate limit、利用tier、region、data retentionなど比較に必要な条件。

提供状態は`available`、`limited`、`unavailable`、`unknown`で記録する。値を確認できない場合に`unavailable`と推測せず、`unknown`または`manual-review`にする。

## 8. 更新周期とreview flow

GitHub Actionsをremote schedulerとし、毎月1、4、7、10、13、16、19、22、25、28、31日の09:00 JSTに、対象登録済みsourceだけを確認する。月境界では間隔が3日未満になる場合があるが、正常稼働時に3日を超えないscheduleとする。scheduler遅延を考慮し、表示上の鮮度は次で判定する。

| state | 最終成功確認からの経過 | 表示 |
|---|---:|---|
| `current` | 4日以内 | 通常表示 |
| `delayed` | 5〜7日 | 警告を表示 |
| `stale` | 8日以上 | 値は履歴として残すが比較・推奨から除外 |
| `unavailable` | 確認不能 | 理由と最終成功日時を表示 |

更新処理は次の状態遷移だけを許可する。

```text
approved
  ├─ observed-unchanged -> monitor-state-only update
  ├─ observed-changed -> draft-review -> approved または rejected
  └─ unavailable / contradictory -> manual-review -> approved または stale
```

変更なしの場合、自動commitが変更できるpathを`monitor-state.yaml`と派生表の鮮度表示に限定し、料金、plan、model、機能、source URLが一文字でも変われば失敗させる。

変更時のdraft PRには、provider、product、market、old/new、単位、source URL、observed_at、抽出警告、生成表差分を含める。自動merge、料金の自動承認、対象serviceの自動追加は禁止する。

## 9. 比較表

### 9.1 Subscription表

service、plan、individual / business、月額、年額、通貨、税込区分、最低seat、主要agent機能、data統制、利用上限、鮮度、sourceを表示する。一時campaignは通常価格と別行または注記にする。

### 9.2 API表

provider、model、提供状態、input、output、cache、tool料金、context、rate-limit条件、鮮度、sourceを表示する。原価格を正本とし、JPY換算は為替source、換算日、計算式を伴う派生値にする。

### 9.3 費用対効果

単価表からmodelの優劣を断定しない。費用対効果は、同じ業務scenario、入力、完了条件、data、権限で、品質、成功率、所要時間、token量、tool料金、人間review時間を測定して判断する。

## 10. API・CLI・MCP・Computer Use

資料では、特定product名でなく実行経路の性質を比較する。

| 経路 | 主な用途 | 強み | 主な制約 |
|---|---|---|---|
| API / CLI | 定型処理、batch、検査可能な操作 | 再現、差分、retryを管理しやすい | 導入に技術知識が必要 |
| MCP | agentから複数tool・dataへ接続 | 接続interfaceを共通化しやすい | server、tool、権限の管理が必要 |
| Computer Use | APIがないUI、既存SaaS、例外処理 | 人間と同じ画面を操作できる | UI変更、誤操作、prompt injection、認証に弱い |
| Hybrid | 安定処理と未接続部分の組合せ | 実用性と安定性を両立しやすい | 経路ごとの監査と停止設計が必要 |

構造化されたAPI、CLI、MCPを安定処理の第一候補にし、Computer Useは必要な画面へ限定する。Computer Useで料金を手動確認する場合も、購入、契約、credential入力は自動実行しない。

## 11. 小さく統制されたAI組織

全社展開の遅さを、企業規模や意思決定者の能力だけで説明しない。system、利用者、data、API、tool、権限が増えると、可視化、審査、test、incident対応の範囲も増える構造として説明する。

「閉じた小さなAI組織」は、次を明示した単位とする。

- 対象業務と観測可能な完了条件。
- 使用してよいdataと正本。
- 人間、agent、Scriptのidentityと担当。
- 接続を許可したAPI、CLI、MCP server、Computer Use画面。
- read / write、外部送信、承認、停止の境界。
- 実行log、費用、品質、incidentの観測方法。

中心messageは「小規模なら安全」ではなく、「小さく統制された単位は、全社一括導入より観測、停止、改善の範囲を限定しやすい」とする。無承認、logなし、個人account依存であれば、小規模でもShadow AIとして扱う。

## 12. Error handling

- source取得失敗: 前回値を更新せず、最終成功日時と失敗理由を表示する。
- HTML構造変更: adapter errorとし、空値を価格ゼロへ変換しない。
- 公式source間の矛盾: `manual-review`へ送り、どちらかを自動採用しない。
- 通貨、単位、market変更: 同じproductの単純な価格変更として処理しない。
- 極端な価格差: typo、campaign、年額・月額、100万token単位を再確認する。
- 認証・JavaScript依存: login回避をせず、Computer Useまたは人間確認へ送る。
- draft PR重複: 同じprovider / productの未解決PRを更新し、重複作成しない。
- renderer失敗: approved dataを維持し、部分的な表を公開しない。

## 13. Security・privacy

- allowlist済みHTTPS domainだけを取得する。
- credential、cookie、購入情報、個人accountを使わない。
- 外部page内のinstructionをdataとして扱い、実行命令として解釈しない。
- 保存fixtureはparser検証に必要な最小断片とfingerprintに限定し、page全体を転載しない。
- draft PRにsecret、cookie、個人情報、決済情報を含めない。
- schedulerには料金確認とdraft作成に必要な最小権限だけを与える。

## 14. Test・quality gate

1. provider registryとapproved dataがschema validationを通る。
2. adapterごとに正常、欠落、構造変更、通貨変更、認証redirectのfixture testがある。
3. allowlist外URL、HTTP downgrade、想定外redirectを拒否する。
4. 差分がold/new、単位、market、source URLを保持する。
5. 変更なし、変更あり、取得不能、矛盾のintegration testがある。
6. 同じapproved dataからMarkdownとPeitho用dataを生成し、値が一致する。
7. `current`、`delayed`、`stale`の境界日をtestする。
8. 一時campaign、年額月換算、税込区分、JPY換算を原値と混同しない。
9. live checkはread-onlyで、契約、購入、login、外部送信を行わない。
10. docs link、format、test、build、secret checkがrepositoryのquality gateを通る。
11. 料金の技術的事実と、採用・推奨の実利用評価を分離する。
12. 料金変更の公開と対象serviceの追加はHuman Approverが承認する。
13. 変更なしのbot commitが、monitor stateと派生表の鮮度表示以外を変更していないことを検査する。

## 15. 完了判定

初版は次を満たしたとき完了とする。

- 初期対象のprovider registryがあり、新規候補の自動追加が無効である。
- subscription表とAPI表が同じapproved dataから再現可能に生成される。
- 3日ごとの確認で、変更なし、変更あり、取得不能を区別できる。
- 変更時にsource付きdraft PRを生成し、人間承認なしに公開値を変更しない。
- 各行にmarket、通貨、単位、鮮度、公式sourceがある。
- Computer UseがAPI、CLI、MCPとの対照軸として説明される。
- 小さく統制されたAI組織の利点と、Shadow AIになる条件が併記される。
- 未確認値、古い値、推測値を「最新」と表示しない。

## 16. 変更影響

| 変更 | 再確認するartifact |
|---|---|
| provider追加・削除 | registry、adapter、両比較表、Peitho脚注 |
| price schema変更 | approved、observations、diff、renderer、fixture |
| 鮮度期間変更 | scheduler、freshness判定、Evidence契約、全比較表 |
| Computer Use方針変更 | 実行経路表、case study、権限・停止境界 |
| 主対象変更 | 表の用語、説明粒度、推奨scenario、Peitho deck |
| Shadow AIの定義変更 | 小規模組織の説明、security checklist、evidence |

## 17. Evidence

次のtableは設計時に参照したsource inventoryである。比較表の個別claimは、[ロードマップのEvidence契約](../../knowledge/materials/agentic-organization-roadmap/evidence/README.md)に従い、一次情報の事実、実利用評価、本資料の推論を分離した完全なfieldで記録する。

| source ID | source | 日付・観測 | 用途 | 扱い |
|---|---|---|---|---|
| `source-bi-ai-pricing-2026-09` | https://www.businessinsider.jp/article/2609-how-much-did-major-generative-ai-service-fees/ | 2026-09-01 | 初期8serviceと比較軸 | 二次情報。料金値の正本にしない |
| `source-openai-pricing-live` | https://chatgpt.com/ja-JP/pricing/ | 2026-09-04観測 | live料金pageの例 | `official-live` |
| `source-claude-pricing-live` | https://claude.com/ja/pricing | 2026-09-04観測 | live料金pageの例 | `official-live` |
| `source-google-ai-pricing-live` | https://one.google.com/intl/ja_jp/about/google-ai-plans/ | 2026-09-04観測 | 地域別subscriptionの例 | `official-live` |
| `source-openai-models-live` | https://developers.openai.com/api/docs/models/compare | 2026-09-04観測 | model、token料金、tool、Computer Useを別項目として扱う根拠 | `official-live` |
| `source-hackerone-ai-security-gap-2026` | https://www.hackerone.com/press-release/hackerone-research-reveals-ai-security-gap-89-organizations-lack-testing-report-more | 2026-03-12 | system増加、test範囲、Shadow AI可視性 | 原調査の発表。vendor biasを伴う |
| `source-akamai-enterprise-ai-risk-2026` | https://www.akamai.com/newsroom/press-release/akamai-research-nearly-half-of-enterprise-ai-use-bypasses-corporate-security-creating-massive-shadow-ai-visibility-gaps | 2026-08-05 | 管理外AI、可視性、agent接続risk | 原調査の発表。vendor biasを伴う |
| `source-reddit-shadow-ai-2026-08` | https://www.reddit.com/r/cybersecurity/comments/1vsm2r4/anyone_else_seeing_shadow_ai_become_worse_than/ | 2026-08-19 | 管理者が把握しないagent接続と規模拡大への実務者反応 | 独立したWeb上の評価。`mixed`、匿名投稿で一般化しない |

「小さく統制された単位の方が観測・停止・改善の範囲を限定しやすい」は、HackerOne、Akamai、実務者投稿が直接証明した因果効果ではなく、本設計がrisk範囲と承認境界から導く推論として表示する。

## 18. 関連

- [エージェント組織導入ロードマップ 設計](2026-09-03-agentic-organization-roadmap-design.md)
- [エージェント組織の将来構想ケーススタディ 設計](2026-09-03-agentic-organization-case-study-design.md)
- [外部情報の選定方針](../../decisions/2026-09-04-external-source-selection-policy.md)
- [ロードマップのEvidence契約](../../knowledge/materials/agentic-organization-roadmap/evidence/README.md)
