# マネジメントループを結論へ反映

## 実施日

2026-09-05

## What

- エージェント組織資料の結論を「背景・目的 → 問い → 判断 → 次の依頼」のloopとして明文化した。
- 次の依頼を`What / Why / How / Done`で作る欄をStage 2とworksheetへ追加した。
- Peitho形式の湯川塾向け結論スライド原稿を追加した。

## Why

- AIのmodelやtoolを追うことより、必要な情報を渡し、結果を判断し、次の仕事を具体的に依頼する管理能力が再現性を左右するため。
- 非エンジニアにも、AI活用を特殊なprompt技術ではなく、既存のマネジメント能力の延長として理解してもらうため。

## How

- 2026-03-05以降に公開された英語圏の一次情報と独立した実務評価を調査した。
- sourceの直接的な事実と、本roadmapの4段階への再構成を区別した。
- AIを人間と同一視せず、暗黙知、権限、禁止事項、人間承認、完了条件の明示が追加で必要とした。
- `audience_ids`、`source_ids`、`management-loop-change`で対象・主張変更時の影響範囲を追跡可能にした。

## 調査・成果物

- [マネジメントループ調査](../planning/research/2026-09-05-agent-management-loop.md)
- [ロードマップ全体](../knowledge/materials/agentic-organization-roadmap/roadmap.md)
- [湯川塾プレゼンの結論スライド](../knowledge/materials/agentic-organization-roadmap/presentations/yukawa-juku-closing.md)

## Peitho検証（2026-09-07）

- 公式手順 `brew install mizzy/tap/peitho` でPeitho CLI 1.25.1を導入した。
- `peitho doctor` は8件pass、外部display未接続の1件warn、fail 0件だった。
- 結論スライドは `peitho build` で1 slideを生成できた。
- `peitho lint` で検出した本文58pxの縦あふれは、タイトル折返しと本文量を最小化して解消した。
- 22.5ptの文字サイズwarnは残る。Peitho 1.25.1の標準scaffoldでも同じwarnを再現したため、独自themeを設計する段階で24pt以上へ調整する。
- 検証生成物は `/private/tmp` に出力し、repositoryには含めていない。

## 未実施

- 湯川塾向けdeck全体の作成と、独自themeを含むPeithoでの表示確認。
- 実際の講義での理解度、質問、判断不能箇所の観測。
- 一枚絵へのマネジメントループ追記。
