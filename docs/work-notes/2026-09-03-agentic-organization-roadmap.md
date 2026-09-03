# エージェント組織導入ロードマップ初版

## 実施日

2026-09-03

## 実施内容

- 主対象者 `audience-business-leader-ai-user` の前提、支援、到達点、変更影響を定義した。
- 主要主張と直接URL、source type、確認日、適用範囲、推論、review triggerを対応付けるevidence contractを作成した。
- 7領域の全体設計図とStage 0〜6を作成した。
- 課題整理、限定委任、効果・品質・費用評価のworksheetを作成した。
- macOS、Codex、Hermes、agentic-frameworkを共通要件にせず、platform-neutralな到達状態を正本にした。

## 主な情報源

- [NIST AI Risk Management Framework](https://www.nist.gov/itl/ai-risk-management-framework)
- [NIST AI RMF Core](https://airc.nist.gov/airmf-resources/airmf/5-sec-core/)
- [NIST Generative AI Profile](https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf)
- [OECD Transparency and explainability](https://oecd.ai/en/dashboards/ai-principles/P7)
- [OWASP Excessive Agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/)
- [NIST least privilege](https://csrc.nist.gov/glossary/term/least_privilege)
- [NIST Zero Trust Architecture](https://csrc.nist.gov/pubs/sp/800/207/final)
- [FinOps Framework](https://www.finops.org/framework/)
- [Google Cloud AI/ML cost optimization](https://docs.cloud.google.com/architecture/framework/perspectives/ai-ml/cost-optimization)

## 検証

- 全Stageに `## Evidence`、直接URL、`checked_at` があることを確認する。
- `scripts/check-doc-links.sh` でbroken linkと孤立noteがないことを確認する。
- CLI、Git、API、MCP、token、provider、connector、orchestration、Scriptの説明を正本に追加した。
- 事実とroadmap recommendationをevidence表の `interpretation` で区別した。

## 未対応

- 市江氏のmacOS環境のcase studyとWindows parity調査
- cloud同期、remote操作、CLI/MCP接続、agentic-framework集中管理、定期Scriptの実現pattern集
- 正本のclaim IDを引き継ぐPeitho deck

これらは正本初版とは独立した成果物であり、各topicの調査とevidence確認を行う別planで実施する。

## 成果物

- [エージェント組織導入ロードマップ](../knowledge/materials/agentic-organization-roadmap/README.md)
- [ロードマップ全体](../knowledge/materials/agentic-organization-roadmap/roadmap.md)
- [設計仕様](../superpowers/specs/2026-09-03-agentic-organization-roadmap-design.md)
- [実装計画](../superpowers/plans/2026-09-03-agentic-organization-roadmap.md)
