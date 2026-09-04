# AIサービス料金監視 Phase 1 実装計画

## 実施日

2026-09-04

## 実施内容

- 人間レビューで大枠承認された料金比較・鮮度監視設計を`approved`へ更新した。
- 既存repositoryのNode.js、test、GitHub Actions、quality gate、docs構造を確認した。
- 外部npm依存を増やさず、Node.js標準機能とJSON互換YAMLで実装する方針をPhase 1計画へ記録した。
- 全13 sourceをregistryへ登録し、ChatGPT subscriptionとOpenAI APIで端から端まで実証する縦切りへ分解した。
- 残り11 sourceは`manual`として明示し、live確認と人間承認前に「最新」と表示しない境界を定義した。
- botの変更なし更新、変更時draft PR、人間による個別承認、workflow write permissionのHuman Approvalを別gateにした。
- Phase 1完了後、subscription 7 sourceとAPI 4 sourceを別planで拡張する方針にした。

## 検証

- `./scripts/check-agent-tools.sh`: must toolすべてOK。
- 既存実装はNode.js ESM、`node:test`、外部packageなしであることを確認した。
- `.github/workflows/`に既存workflowがないため、新設workflowをinfrastructure review対象にした。

## 成果物

- [AIサービス料金監視 Phase 1 実装計画](../superpowers/plans/2026-09-04-ai-service-pricing-monitor-phase-1.md)
- [AIサービス料金比較・鮮度監視 設計](../superpowers/specs/2026-09-04-ai-service-pricing-monitor-design.md)

## 未実施

- GitHub Issue作成とtask branch作成。
- 実装、fixture test、live read-only確認。
- GitHub Actionsの権限承認と有効化。
- 残り11 sourceのadapter調査・実装。
