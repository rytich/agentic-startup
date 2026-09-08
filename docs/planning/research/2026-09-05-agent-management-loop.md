---
title: AIへの質問・判断・次の依頼をつなぐマネジメントループ調査
status: active
updated: 2026-09-08
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-business-outcome-first, principle-small-scope-learning-loop, principle-human-accountability]
source_ids: [source-microsoft-wti-2026-05-05, source-microsoft-goals-2026-04, source-google-delegation-2026-08-22, source-span-agent-effectiveness-2026-07, source-simonwillison-claude-code-2026-07-21]
derived_artifacts: [claim-management-loop-001, claim-management-loop-002, presentation-yukawa-closing-management-loop]
review_triggers: [source-age-6-months, management-loop-change, steering-capability-change, audience-profile-change, source-policy-change]
---

# AIへの質問・判断・次の依頼をつなぐマネジメントループ調査

## 調査日

2026-09-05

## 調査した問い

1. AIへ必要十分な背景と目的を渡すことは、成果や手戻りとどう関係するか。
2. AIの結果を判断し、次の依頼へつなぐ行為を、人間のマネジメントと同じ構造として説明できるか。
3. その説明で、AI固有の権限・承認・暗黙知の不足を見落とさないために何を併記すべきか。

## 採用した一次情報

| source_id | 公開日 | source | 確認した事実 | 適用上の注意 |
|---|---|---|---|---|
| source-microsoft-wti-2026-05-05 | 2026-05-05 | [Microsoft 2026 Work Trend Index](https://www.microsoft.com/en-us/worklab/work-trend-index/agents-human-agency-and-the-opportunity-for-every-organization) | 20,000人のsurveyとMicrosoft 365の匿名化signalを基に、AI活用能力を「指示する・出力を判断する・学ぶ」と捉え、明確な意図、品質基準、管理慣行、反復可能なhandoffを重視している | Microsoft製品利用者を中心とした調査であり、全業種・全providerへの因果関係は示さない |
| source-microsoft-goals-2026-04 | 2026-04-14 | [Goals as First-Class Abstractions in Human-AI Collaboration](https://www.microsoft.com/en-us/research/wp-content/uploads/2026/04/2026-goals-as-first-class-abstractions-in-human-ai-collaboration-AutomationXP26_paper_0982.pdf) | agentへtask完了だけでなくgoalを委任し、人とAIが目的を反復的に明確化し、toolやcontextをまたいで引き継ぎ、outcomeで評価する研究課題を提示している | position paperであり、4段階loopの効果を実験で証明したものではない |
| source-google-delegation-2026-08-22 | 2026-08-22 | [How agents can delegate better](https://cloud.google.com/blog/products/ai-machine-learning/how-agents-can-delegate-better) | 人間の委任との共通性、検証可能な単位への分解、情報の正確性・関連性・統制、必要箇所への人間判断、権限最小化を説明している | Google Cloudの解説記事。元論文は鮮度基準外のため、記事で直接確認できる範囲だけを採用する |

## 独立したWeb・実務評価

| source_id | 公開日 | source | signal | 確認した評価と制約 |
|---|---|---|---|---|
| source-span-agent-effectiveness-2026-07 | 2026-07 | [AI Coding Agent Effectiveness: Leading Indicators](https://www.span.app/research/agent-effectiveness-july2026) | positive | 2026年5〜7月、103 engineering teamの実trajectoryを分析。明確な依頼、build/test可能な環境、feedback loop、quality stewardshipが、高いagent autonomyや少ないreview手戻りと関連した。観測研究であり因果関係は未証明 |
| source-simonwillison-claude-code-2026-07-21 | 2026-07-21 | [A Fireside Chat with Cat and Thariq from the Claude Code team](https://simonwillison.net/2026/Jul/21/cat-and-thariq/) | mixed | 第三者によるAnthropic実務者インタビュー。contextを増やす一方で硬い禁止指示を減らし、人間でも誤解し得る指示や例外を見直す運用を紹介。単純に情報量やrule数を増やせばよいわけではない |

## 採用しなかった候補

| source | 理由 |
|---|---|
| [Intelligent AI Delegation](https://arxiv.org/abs/2602.11865) | 原著論文だが2026-02-12公開で、調査日の6か月以内という採用基準を満たさない。Google Cloudの2026-08-22記事で著者自身が説明した範囲だけを使用する |
| [A business leader’s guide to working with agents](https://cdn.openai.com/business-guides-and-resources/a-business-leaders-guide-to-working-with-agents.pdf) | 内容は関連するが、確認できたPDF本文に公開日・実質更新日がなく、鮮度を証明できない |
| 2025年以前のNIST、OECD、OWASP資料 | 既存資料には旧形式で残るが、今回追加するclaimの有効な根拠には数えない |
| Reddit上の個別運用投稿 | 共通運用の候補探索には有用だが、反応数が少なく、本人性・環境・再現条件を十分確認できないため採用しない |

## 結論

次の四段階は、sourceをそのまま転記したものではなく、一次情報と実務評価から今回の対象者向けに導いた推奨である。

1. 必要十分な背景と目的を伝える。
2. その答えで何を判断するかを含めて、質問を決める。
3. 結果を事実、推論、不足情報に分けて、人間が判断する。
4. 判断理由から次の依頼を`What / Why / How / Done`で作る。

この循環を「良い人間のマネジメントと同じ構造」と説明することは妥当である。ただし、AIを人間と同一視しない。AIには組織の暗黙知、継続的な責任、現実世界の権限感覚が自動では備わらないため、参照する情報、許可・禁止、承認、上限、完了条件をより明示する。

## steeringの追加確認（2026-09-08）

- **選択した方針:** ユーザーの「steeringのonも推奨」という依頼と設計承認に基づき、対応環境に設定がある場合はONを推奨する。標準で利用できる場合は追加設定せず活用する。理由は、作業の途中で前提・範囲のずれを指摘し、管理loopを回せるようにするため。
- **公式情報:** [Codex CLI](https://learn.chatgpt.com/docs/codex/cli)の「Keep the coding loop in your terminal」は実行中の方向修正を案内している。source ID: `source-openai-codex-cli-steering`、checked_at: `2026-09-08`。公開・実質更新日は確認できていないため、6か月未満の研究・評価evidenceとしては採用せず、現行の機能説明の参照先として記録する。
- **ローカル観測:** 同日、`codex --version`は`codex-cli 0.147.0`、`codex features list`は`steer removed true`を返した。旧flagをONにする手順は掲載しない。desktop設定の状態や全modelでの挙動をこの出力から推定しない。
- **APIと製品の区別:** [OpenAI Responses APIのsteering仕様](https://developers.openai.com/api/reference/cli/resources/beta/subresources/responses)は前回調査で確認したAPI層の参考情報。APIの対応条件や受付・反映イベントを、そのままCodex UIの仕様として説明しない。
- **評価と未検証:** steering固有の独立したWeb/SNS実利用評価、費用対効果、Windows・hermesでの再現は未検証。過去の一般的なfeedback loopの評価をsteering固有の効果の証拠として流用しない。ON推奨はユーザー指定の運用方針であり、一般効果の実証claimにはしない。資料はdraftを維持する。
- **確認方法:** 読み取り・下書きの小作業で、実行中に対象範囲を修正し、返答と成果物が修正に沿うか観測する。今回この実利用試験は未実施。修正の送信だけを反映・停止・取消の完了と扱わず、高影響操作の事前承認を維持する。

## 影響範囲

| 変更する前提 | 再確認する対象 |
|---|---|
| 主対象者 | `audience-business-leader-ai-user`を持つroadmap、Stage 1/2、worksheet、ケーススタディ、Peitho deck |
| 4段階の管理loop | `management-loop-change`を持つ正本と派生物 |
| `What / Why / How / Done` | collaboration rulesとの意味整合、委任worksheet、完了条件 |
| source鮮度・評価基準 | Evidence契約、本調査、claim table、Peitho脚注 |
| steeringの対応・設定・反映タイミング | `steering-capability-change`を持つroadmap、Stage 2、委任worksheet、Codex環境ガイド、本調査。現在の結論slideには機能名を追加せず、将来deckへ展開するときにこの範囲を引き継ぐ |

## 関連

- [エージェント組織導入ロードマップ](../../knowledge/materials/agentic-organization-roadmap/roadmap.md)
- [ロードマップのEvidence契約](../../knowledge/materials/agentic-organization-roadmap/evidence/README.md)
- [外部情報の選定方針](../../decisions/2026-09-04-external-source-selection-policy.md)
