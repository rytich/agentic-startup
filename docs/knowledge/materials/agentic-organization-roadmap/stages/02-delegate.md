---
title: Stage 2 - 一作業を委任する
status: active
updated: 2026-09-03
audience_ids: [audience-business-leader-ai-user]
principle_ids: [principle-small-scope-learning-loop, principle-human-accountability]
stage_ids: [stage-2]
platform_ids: [platform-neutral]
source_ids: [claim-stage2-001, claim-stage2-002, claim-stage2-003]
derived_artifacts: [worksheet-delegation]
review_triggers: [owasp-llm-top10-change, nist-genai-profile-revision, stage-2-change]
---

# Stage 2 - 一作業を委任する

## このStageの目的

Stage 1で採用した課題から、一度に結果を確認できる作業を一つだけAIへ委任します。AIが使える機能、権限、自律範囲が広すぎると、誤解、誤った出力、悪意ある入力から不要な変更を実行する危険が増えます。[OWASP Excessive Agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/)

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

これらはOWASPが示す、機能・権限・自律性を必要最小限にし、高影響操作に人の承認を求める対策を、非エンジニア向け委任手順にしたものです。[OWASP Excessive Agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/)

## 次へ進む条件

- AIが実行してよいこと、禁止することを説明できる。
- 人間承認が必要な操作と責任者が明確である。
- 合否、上限、停止、復旧を事前に決めている。

## 次に行うこと

委任ワークシートで「許可する操作」を一つだけ選び、最初は読み取りまたは下書き作成として試します。

## Evidence

| claim_id | claim | source_url | source_type | checked_at | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|
| claim-stage2-001 | 過剰な機能、権限、自律性は agent が損害を与える Excessive Agency の主因になる | https://genai.owasp.org/llmrisk/llm062025-excessive-agency/ | official | 2026-09-03 | agentic-ai, stage-2 | OWASP の直接的な内容 | owasp-llm-top10-change |
| claim-stage2-002 | 機能と権限を最小化し、高影響操作に人の承認を求め、接続先で認可を強制する | https://genai.owasp.org/llmrisk/llm062025-excessive-agency/ | official | 2026-09-03 | agentic-ai, stage-2 | OWASP の直接的な対策を委任worksheetへ具体化 | owasp-llm-top10-change |
| claim-stage2-003 | GenAI のriskはmodel、system、use caseで異なり、contextと影響に応じて対策を調整する | https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf | public | 2026-09-03 | generative-ai, stage-2 | NIST GenAI Profile の直接的な内容。小さな一作業から始めるのはroadmap recommendation | nist-genai-profile-revision |

## 関連

- [ロードマップ全体](../roadmap.md)
- [委任ワークシート](../worksheets/delegation.md)
