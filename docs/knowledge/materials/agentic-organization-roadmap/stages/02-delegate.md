---
title: Stage 2 - 一作業を委任する
status: draft
updated: 2026-09-08
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-small-scope-learning-loop, principle-human-accountability]
stage_ids: [stage-2]
platform_ids: [platform-neutral]
source_ids: [source-google-delegation-2026-08-22, source-simonwillison-claude-code-2026-07-21]
derived_artifacts: [worksheet-delegation]
review_triggers: [management-loop-change, steering-capability-change, source-age-6-months, stage-2-change, source-policy-change]
---

# Stage 2 - 一作業を委任する

## このStageの目的

Stage 1で採用した課題から、一度に結果を確認できる作業を一つだけAIへ委任します。Google Cloudは、複雑な目的を監視・検証できる小さな単位まで分解し、必要な権限だけを渡すことを、AIと人間が混在する委任の原則として説明しています。[How agents can delegate better](https://cloud.google.com/blog/products/ai-machine-learning/how-agents-can-delegate-better)

## 委任定義

[委任ワークシート](../worksheets/delegation.md)で次を決めます。

- 目的と期待出力
- AIに渡す入力と、渡してはいけないデータ
- 許可する機能と禁止する操作
- AIが参照・変更できる対象
- 人間承認が必要な操作
- 合否を確認する方法
- 時間、回数、費用の上限
- 失敗時の停止条件と戻し方

## 安全側の初期設定

- 読み取りから始め、変更権限は必要になってから追加する。
- 目的に不要な機能や接続をAIへ渡さない。
- 投稿、送信、削除、購入、権限変更など影響の大きい操作は実行前に人が承認する。
- 権限確認をAIの判断だけに任せず、接続先でも強制する。
- 操作履歴を残し、上限到達または想定外の結果で停止する。

これらは、Google Cloudが2026年8月に示した検証可能な分解、必要最小限の情報と権限、人間判断の配置を、非エンジニア向け委任手順へ具体化したものです。[How agents can delegate better](https://cloud.google.com/blog/products/ai-machine-learning/how-agents-can-delegate-better)

## 結果から次の依頼を作る

一回の依頼で完成を目指しません。結果を確認した人が、採用、保留、却下の理由を残し、その判断から次の一作業を作ります。次の依頼には、人間への仕事の依頼と同じように、次の四つを含めます。

| 要素 | 伝えること |
|---|---|
| `What` | 今回行う一作業と成果物 |
| `Why` | 前回の結果から、なぜこの作業が必要になったか |
| `How` | 使ってよいdata・tool・手順と、禁止事項・承認境界 |
| `Done` | 何を観測できれば完了と判断するか |

AIは暗黙の前提を共有しないため、人間への依頼以上に、前回結果の参照先、権限、停止条件、確認方法を明示します。Google Cloudは、委任を検証可能な単位へ分解し、曖昧な依頼では人間確認を求められる構造を提案しています。[How agents can delegate better](https://cloud.google.com/blog/products/ai-machine-learning/how-agents-can-delegate-better) Anthropicの実務者への第三者インタビューでは、contextを増やしつつ矛盾する硬い指示を減らし、人間でも誤解し得る表現を見直す運用が紹介されています。[Simon Willison, July 21 2026](https://simonwillison.net/2026/Jul/21/cat-and-thariq/)

## 実行中にも方向を修正する（steering）

steeringは、AIが作業している途中に補足や修正を伝える機能です。本資料の運用方針として、対応環境にON/OFF設定がある場合はONを推奨します。標準で利用できる環境では、そのまま活用します。目的は、前提の誤解や対象範囲のずれに気づいた時点で、理由を添えて修正することです。

例えば「提案の対象は既存顧客です。新規向けの説明は対象外なので、既存顧客向けの提案に切り替えてください。送信は引き続き承認待ちにしてください」と伝えます。変更点、理由、維持する条件をまとめ、AIの返答や成果物で反映を確かめます。

steeringの対応、設定、反映タイミングは利用画面・バージョン・modelによって確認します。使えない場合は、作業を止めるか次の確認時点で修正を渡します。送信・削除などの実行前承認は維持し、途中メッセージを送っただけで停止・取消・権限変更が完了したとは扱いません。

[Codex CLI公式資料](https://learn.chatgpt.com/docs/codex/cli)は実行中の方向修正を案内しています。ON推奨はユーザーが選んだ本資料の運用方針であり、費用削減や品質改善が実証された一般則ではありません。確認日・適用範囲・未検証事項は[steeringの調査追記](../../../../planning/research/2026-09-05-agent-management-loop.md#steeringの追加確認2026-09-08)を参照してください。

## 次へ進む条件

- AIが実行してよいこと、禁止することを説明できる。
- 人間承認が必要な操作と責任者が明確である。
- 合否、上限、停止、復旧を事前に決めている。
- 前回の結果と判断理由から、次の依頼の`What / Why / How / Done`を説明できる。
- 利用環境での途中修正の方法と、反映確認・停止・再依頼の方法を説明できる。

## 次に行うこと

委任ワークシートで「許可する操作」を一つだけ選び、最初は読み取りまたは下書き作成として試します。結果を[効果・品質・費用の評価](../worksheets/evaluation.md)へ記録し、次の依頼を一つだけ作ります。

## Evidence

| claim_id | claim | primary_source_url | source_type | source_published_or_updated_at | observed_at | source_fingerprint | checked_at | reception_urls | reception_published_at | reception_signal | reception_summary | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| claim-management-loop-002 | 有効な委任では、作業を検証可能に分け、正確で関連する情報を渡し、必要箇所に人間判断を置く | https://cloud.google.com/blog/products/ai-machine-learning/how-agents-can-delegate-better | official | 2026-08-22 | — | — | 2026-09-05 | https://simonwillison.net/2026/Jul/21/cat-and-thariq/ | 2026-07-21 | mixed | Anthropic実務者への第三者インタビューでは、contextを増やす一方で硬い指示を減らし、誤解され得る表現を見直す必要性を指摘 | audience-business-leader-ai-user, stage-2, platform-neutral | `What / Why / How / Done`は本repositoryのInstruction Patternへ接続した推論 | management-loop-change, source-age-6-months |

## 関連

- [ロードマップ全体](../roadmap.md)
- [委任ワークシート](../worksheets/delegation.md)
- [マネジメントループ調査](../../../../planning/research/2026-09-05-agent-management-loop.md)
