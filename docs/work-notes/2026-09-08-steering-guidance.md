# steeringのON推奨を委任運用へ反映

## What / Why / How

- What: 対応環境ではsteeringをONにし、実行中の補足・方向修正を活用する運用方針を追加した。
- Why: ユーザー依頼と設計承認に基づき、背景と理由を伝え、途中のずれを修正するマネジメントloopを具体化するため。
- How: roadmap、Stage 2、委任worksheet、Codex環境ガイド、調査記録へ反映。変更時の確認対象を`steering-capability-change`で追跡する。

## 根拠と境界

- [調査記録](../planning/research/2026-09-05-agent-management-loop.md#steeringの追加確認2026-09-08)に公式URL、確認日、CLI 0.147.0の観測、独立評価と実利用試験の未実施を記録した。
- `removed / true`だけを根拠にdesktop設定をONと断定しない。端末設定自体は変更していない。
- 結論slideの本文は維持。steeringの推奨は[Stage 2](../knowledge/materials/agentic-organization-roadmap/stages/02-delegate.md)を正本とした。
- 追跡先: [Issue #3](https://github.com/rytich/agentic-startup/issues/3)、[PR #4](https://github.com/rytich/agentic-startup/pull/4)への同じ管理loopの追記。マージは未実施。

## 検証

- `bash scripts/check-doc-links.sh`: 154 docs、broken links 0、orphans 0。
- `git diff --check`: 成功。
- 今回は文書変更のみ。steeringの実利用試験や効果検証を完了とは扱わない。
