---
title: エージェント組織資料の外部根拠調査
status: done
updated: 2026-09-04
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-business-outcome-first, principle-platform-independent, principle-human-accountability]
source_ids: [research-agentic-001, research-agentic-002, research-agentic-003, research-agentic-004, research-agentic-005, research-agentic-006, research-agentic-007, research-agentic-008, research-agentic-009, research-agentic-010, research-agentic-011]
derived_artifacts:
  - ../../superpowers/specs/2026-09-03-agentic-organization-case-study-design.md
  - ../../knowledge/materials/agentic-organization-roadmap/evidence/README.md
review_triggers: [target-audience-change, roadmap-outcome-change, platform-capability-change, evidence-contract-change]
---

# エージェント組織資料の外部根拠調査

## 1. 目的

エージェント組織のロードマップとケーススタディで用いる主要主張を、英語圏の原著論文、公的機関、標準、providerの公式技術資料で確認する。製品の新機能を追うことより、事業で再現可能な課題分解、役割分担、統制、評価へ根拠を与えることを目的とする。

## 2. 非公開の参考資料の扱い

共有された非公開資料は、論点の発見と説明表現の改善に限って内部参考として扱う。資料名、ページ、URL、引用箇所、source IDを、本repositoryの出典・参考文献・evidence tableへ記録しない。

内部参考から着想した内容は、次のいずれかを満たす場合だけ成果物へ採用する。

1. 外部の一次資料で独立して確認でき、外部sourceを主張の根拠として記録できる。
2. 根拠が一般化できない場合は、市江氏の設計方針または本資料の推奨と明示する。

固有の文章、見出し順、段階名、図表構成、画面例は転用しない。

## 3. 調査結果

| claim_id | 調査から採用する示唆 | 本資料への適用 | source_url | source_type | checked_at | interpretation / limitation |
|---|---|---|---|---|---|---|
| `research-agentic-001` | AIの有効性は同じ知識workflow内でもtaskごとに不均一で、適用範囲外では人の成果を悪化させうる | Stage 1で課題を小さな作業と完了条件へ分解し、`OUT-07`と`OUT-11`で人の判断点を残す | [Organization Science: Navigating the Jagged Technological Frontier](https://doi.org/10.1287/orsc.2025.21838) | `paper` | 2026-09-04 | 758名の知識労働者を対象とした特定task群の実験。全業務へ効果量を一般化せず、task単位で検証する根拠として使う |
| `research-agentic-002` | 生成AIは特定の専門的writing taskで時間短縮と品質向上を示した | 導入効果を印象でなく、時間、品質、完了率で前後比較する | [Science: Experimental evidence on the productivity effects of generative artificial intelligence](https://doi.org/10.1126/science.adh2586) | `paper` | 2026-09-04 | 453名の専門職によるwriting taskの実験。数値は同種taskの参考であり、agent workflow全体の期待値にはしない |
| `research-agentic-003` | 実業務では経験の浅い担当者ほど支援効果が大きく、熟練者では効果が小さいか一部品質低下もありうる | 主対象者の経験不足を、標準化された手順、根拠表示、修正可能性で補う。熟練者の確認も省かない | [The Quarterly Journal of Economics: Generative AI at Work](https://doi.org/10.1093/qje/qjae044) | `paper` | 2026-09-04 | 一社のcustomer support、5,172名の観測。職種を越えた一般化ではなく、対象者別評価の必要性を支える |
| `research-agentic-004` | 予測可能なtaskはworkflow、柔軟な判断が必要なtaskはagentとし、必要になるまで複雑性を増やさない | `OUT-07`でScript、workflow、agent、人を選び分ける。agent化を成熟度の目的にしない | [Anthropic: Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) | `official` | 2026-09-04 | providerの実装知見。製品固有APIではなく、複雑性を段階的に増やす設計原則だけを採用する |
| `research-agentic-005` | single-agentから始め、役割やtool選択の複雑性が必要な場合にmulti-agentへ進む。高risk操作や失敗閾値超過は人へ戻す | Scenario Bの複数agentを役割数で正当化せず、境界と失敗条件を定義する。SFA確定更新と外部送信は人間承認にする | [OpenAI: A practical guide to building agents](https://openai.com/business/guides-and-resources/a-practical-guide-to-building-ai-agents/) | `official` | 2026-09-04 | providerの実装guide。特定SDKの採用根拠にはせず、段階導入とhuman interventionの根拠に限定する |
| `research-agentic-006` | 複数agentの実装例では、中央のorchestratorが計画、担当指示、進捗追跡、再計画、障害回復を担う | `OUT-05`、`OUT-09`、`OUT-10`にtask ledger、progress ledger、再計画を含める | [Microsoft Research: Magentic-One](https://www.microsoft.com/en-us/research/publication/magentic-one-a-generalist-multi-agent-system-for-solving-complex-tasks/) | `paper` | 2026-09-04 | research prototypeのarchitecture。採用製品ではなく、集中管理の設計patternとして参照する |
| `research-agentic-007` | multi-agentの失敗は、仕様・system設計、agent間の不整合、検証・終了判定に大別できる | Scenario Cを端末障害だけでなく、handoff欠落、状態不整合、誤終了、重複実行まで広げる | [arXiv: Why Do Multi-Agent LLM Systems Fail?](https://arxiv.org/abs/2503.13657) | `paper` | 2026-09-04 | 7 framework、1,642 traceのtaxonomy。preprintとして扱い、failure checklistの候補に限定する |
| `research-agentic-008` | agent評価は精度だけでなく費用を同時に測り、実利用に近いtaskと再現可能な条件で比較する必要がある | `OUT-08`の評価を品質、完了、費用、時間、再現性の組にする | [arXiv: AI Agents That Matter](https://arxiv.org/abs/2407.01502) | `paper` | 2026-09-04 | agent benchmarkの方法論研究。preprintであり、個別agentの優劣の根拠には使わない |
| `research-agentic-009` | AIの能力と限界を明示し、誤りを容易に修正・撤回でき、不確実なときは適用範囲を狭めることが人間との協働に重要 | 非エンジニア向け資料に、できること、できないこと、修正方法、停止方法を必須表示する | [CHI 2019: Guidelines for Human-AI Interaction](https://doi.org/10.1145/3290605.3300233) | `paper` | 2026-09-04 | 20製品を用いた設計guidelineの検証。組織運用へ適用する部分は本資料の推論として明示する |
| `research-agentic-010` | MCPはhost、client、serverを分離し、権限、security policy、consent、capability negotiationを境界として持つ | `OUT-04`を単なるconnector交換でなく、接続能力と権限境界を宣言できる設計にする | [Model Context Protocol: Architecture](https://modelcontextprotocol.io/specification/2026-07-28/architecture) | `standard` | 2026-09-04 | protocol architectureの事実。MCP対応だけで安全性や業務再現性が保証されるとは解釈しない |
| `research-agentic-011` | AI費用はtoken単価だけでなく、business outcomeに結び付くunit metricで評価する | `OUT-08`でcost per completed outcome、human review時間、再作業率を追う | [FinOps Framework: Unit Economics](https://www.finops.org/framework/capabilities/unit-economics/) | `standard` | 2026-09-04 | vendor非依存のcost management framework。本資料では組織規模に合わせて最小metricから始める |

## 4. 設計へ反映する結論

### 4.1 業務をagent数でなく処理特性に分ける

- 形式検査、重複検査、定時収集、状態更新は、可能な限りScriptまたは固定workflowへ置く。
- 情報収集、仮説生成、例外処理のように入力と手順が揺れる部分だけagentへ置く。
- 登録確定、外部送信、権限変更、課金、削除は人間の承認または明示したpolicyへ戻す。

### 4.2 複数agentには共通台帳と終了条件を持たせる

各agentの名称より、入力、出力、責任範囲、参照可能data、操作可能tool、失敗時の戻り先、終了条件を正本にする。orchestratorは担当振り分けだけでなく、進捗、根拠、費用、承認待ち、retry、停止を集中管理する。

### 4.3 効果は対象者とtaskごとに測る

「AIで生産性が上がる」を一般論として置かない。対象task、対象者、baseline、quality、time、cost、再作業、人間承認を対応付ける。特に初心者支援の期待と、適用範囲外での品質低下を同時に検証する。

### 4.4 非エンジニア向けには修正と停止を先に示す

再現手順だけでなく、AIができる範囲、根拠の確認方法、間違いの直し方、止め方、人へ戻す条件を先に示す。これにより、技術知識ではなく観測可能な業務状態で判断できるようにする。

## 5. 変更影響マップ

| 変更対象 | 直接更新する正本 | 連動確認する成果物 |
|---|---|---|
| 主対象者、前提知識、判断責任 | audience profile、本researchの`research-agentic-003`と`009` | roadmapの説明粒度、worksheet、Scenario Bの承認点、Peitho deck |
| 重要成果または達成条件 | case study designの成果表 | outcome matrix、全scenario、OS別実現例、evidence、Peitho deck |
| agent / Script / 人の役割方針 | `OUT-07`、`OUT-10`、本researchの4.1と4.2 | Scenario B、failure scenario、評価worksheet |
| provider、model、CLI、MCP仕様 | OS別実現例と該当claim | `OUT-03`、`OUT-04`、権限境界、model比較、review trigger |
| 費用評価方針 | `OUT-08`、evaluation worksheet | model比較、継続・改善・停止判断、Peitho deckの効果表示 |
| 内部参考またはsource方針 | Evidence契約、本researchの2 | 全claim table、planning research、Peitho deckの出典 |

## 6. 採用しない一般化

- 特定実験の生産性向上率を、エージェント組織全体の効果として掲示しない。
- multi-agent architectureを、single-agentやScriptより上位の成熟状態として扱わない。
- MCP対応を、connector非依存、安全性、権限分離の完了条件とみなさない。
- providerの実装guideを、唯一の正解または特定製品の採用根拠にしない。
- preprintのtaxonomyを、確定した業界標準として扱わない。
