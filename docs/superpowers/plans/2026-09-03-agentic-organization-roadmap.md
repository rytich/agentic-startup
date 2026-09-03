# Agentic Organization Roadmap Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** AI service 利用経験はあるが CLI、Git、server 運用は未経験の事業責任者が、エージェント組織の全体設計図を描き、事業課題を一つの検証可能な委任へ絞り、次の改善を AI または専門家へ指示できる正本ロードマップを作る。

**Architecture:** 正本を対象者 profile、7 領域の設計図、成熟度別 stage、worksheet、evidence に分け、ID で接続する。各 stage は調査を先行し、主張、直接 URL、確認日、適用範囲、資料側の推論を残す。macOS/Windows case study と Peitho deck は正本完成後の別 plan で派生させる。

**Tech Stack:** Markdown、YAML frontmatter、Git、`rg`、既存 `scripts/check-doc-links.sh`、公式 web documentation。

**Spec:** `docs/superpowers/specs/2026-09-03-agentic-organization-roadmap-design.md`

## Global Constraints

- 主対象は `audience-business-leader-ai-user` とする。
- 第一到達点は自力実装ではなく、事業適用の判断、構築・改善指示、結果の採否を本人が行える状態とする。
- 原則、到達状態、評価方法は `platform-neutral` とし、macOS、Codex、Hermes、agentic-framework を必須条件にしない。
- 各 stage は本文作成前に調査し、主要主張ごとに直接 URL、source type、確認日、適用範囲、推論の有無、review trigger を記録する。
- 法令、公的機関、標準化団体、公式 documentation、公式 repository、原著論文を優先する。
- 検索結果 URL、無関係な top page、AI response、access 不能な URL を evidence にしない。
- 変更されやすい product 仕様、料金、model、API、CLI は実行時に再確認する。
- 未検証の代替環境や効果は `candidate` または `inference` と明記する。
- 本文は非エンジニア向けの日本語とし、技術用語は初出で平易に説明する。
- 各 stage の末尾で、読者が次に行う一項目を選べるようにする。
- 外部 package を追加しない。

---

## File Structure

- `docs/knowledge/materials/agentic-organization-roadmap/README.md`: map、正本と派生物の境界、読む順序。
- `docs/knowledge/materials/agentic-organization-roadmap/audiences/business-leader-ai-user.md`: 対象者の前提、支援、到達点、変更影響。
- `docs/knowledge/materials/agentic-organization-roadmap/roadmap.md`: 7 領域、現在地診断、stage 間の関係。
- `docs/knowledge/materials/agentic-organization-roadmap/stages/00-map.md`: 全体を描く。
- `docs/knowledge/materials/agentic-organization-roadmap/stages/01-decide.md`: 判断を補助させる。
- `docs/knowledge/materials/agentic-organization-roadmap/stages/02-delegate.md`: 一作業を委任する。
- `docs/knowledge/materials/agentic-organization-roadmap/stages/03-reproduce.md`: 再現可能にする。
- `docs/knowledge/materials/agentic-organization-roadmap/stages/04-separate-roles.md`: 人、AI、Script の役割を分ける。
- `docs/knowledge/materials/agentic-organization-roadmap/stages/05-operate.md`: 組織として運用する。
- `docs/knowledge/materials/agentic-organization-roadmap/stages/06-optimize.md`: 品質と費用を最適化する。
- `docs/knowledge/materials/agentic-organization-roadmap/worksheets/system-map.md`: 7 領域の初期設計図。
- `docs/knowledge/materials/agentic-organization-roadmap/worksheets/problem-framing.md`: 事業課題を一成果へ絞る。
- `docs/knowledge/materials/agentic-organization-roadmap/worksheets/delegation.md`: 委任範囲、権限、完了条件。
- `docs/knowledge/materials/agentic-organization-roadmap/worksheets/evaluation.md`: 効果、品質、費用、risk、次の判断。
- `docs/knowledge/materials/agentic-organization-roadmap/evidence/README.md`: claim table schema と source 品質基準。
- `docs/work-notes/2026-09-03-agentic-organization-roadmap.md`: 調査、作成物、検証、未対応。
- `docs/knowledge/materials/README.md`: 新資料群への入口。

全 content file は `audience_ids`、`principle_ids`、`stage_ids`、`platform_ids`、`source_ids`、`derived_artifacts`、`review_triggers` を frontmatter に持つ。各 stage の `## Evidence` は次の列をこの順で持つ。

```markdown
| claim_id | claim | source_url | source_type | checked_at | applies_to | interpretation | review_trigger |
|---|---|---|---|---|---|---|---|
```

---

### Task 1: 資料 contract と対象者 profile

**Files:**
- Create: `docs/knowledge/materials/agentic-organization-roadmap/README.md`
- Create: `docs/knowledge/materials/agentic-organization-roadmap/audiences/business-leader-ai-user.md`
- Create: `docs/knowledge/materials/agentic-organization-roadmap/evidence/README.md`
- Modify: `docs/knowledge/materials/README.md`

**Interfaces:**
- Produces: 全 artifact が使う metadata と evidence contract。
- Produces: `audience-business-leader-ai-user` の正本。

- [x] **Step 1: 対象者の5区分を設計仕様から抽出する**

`前提知識`、`提供する支援`、`期待する到達点`、`対象外`、`変更時に再確認する artifact` を設計仕様の第 1、2、9、15 節と照合する。

- [x] **Step 2: 対象者と透明性について調査する**

OECD AI Principle と NIST AI RMF Core を読み、能力・限界・根拠・責任を理解可能にする要件を記録する。

- `https://oecd.ai/en/dashboards/ai-principles/P7`
- `https://airc.nist.gov/airmf-resources/airmf/5-sec-core/`

- [x] **Step 3: profile、evidence schema、map を作成する**

AI service は利用済みだが CLI、Git、server 運用は未経験、自力実装は必須でない、採否と改善指示を行えることが到達点、と明記する。source 優先順位、8 列の claim table、直接 URL、推論表示、再確認ルールを書く。

- [x] **Step 4: 検証する**

Run: `./scripts/check-doc-links.sh && rg -n "audience-business-leader-ai-user|claim_id|source_url|checked_at|interpretation" docs/knowledge/materials/agentic-organization-roadmap`

Expected: link check が `OK`。対象者 ID と evidence contract が検出される。

- [x] **Step 5: Commit**

Run: `git add docs/knowledge/materials/agentic-organization-roadmap docs/knowledge/materials/README.md && git commit -m "docs: define roadmap audience and evidence contract"`

---

### Task 2: Stage 0 と全体設計図

**Files:**
- Create: `docs/knowledge/materials/agentic-organization-roadmap/stages/00-map.md`
- Create: `docs/knowledge/materials/agentic-organization-roadmap/worksheets/system-map.md`
- Create: `docs/knowledge/materials/agentic-organization-roadmap/roadmap.md`
- Modify: `docs/knowledge/materials/agentic-organization-roadmap/README.md`

**Interfaces:**
- Consumes: Task 1 の contract。
- Produces: 7 領域、6 状態の診断、`stage-0` の移行条件。

- [x] **Step 1: 調査質問を固定する**

「AI 導入を事業目的、context、risk、測定へ結び付けるには何を先に描くか」「未完成の profile を更新する根拠は何か」を問う。

- [x] **Step 2: 公的 framework を調査する**

- `https://www.nist.gov/itl/ai-risk-management-framework`
- `https://airc.nist.gov/airmf-resources/airmf/5-sec-core/`
- `https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf`

NIST の govern、map、measure、manage と、組織目標、risk tolerance、resource に合わせた profile を確認する。AI RMF 1.0 が改訂中である点を review trigger にする。

- [x] **Step 3: Stage 0、worksheet、roadmap 骨格を作成する**

事業目的、役割、data、実行環境、接続、境界と統制、効果と費用を、`未決定`、`今回検証`、`現在は決めない` に分ける。次へ進む条件は「今回扱う課題を一つ選べる」とする。

- [x] **Step 4: 検証して commit する**

Run: `./scripts/check-doc-links.sh && rg -n "https://|checked_at|今回検証|次に行う" docs/knowledge/materials/agentic-organization-roadmap/stages/00-map.md`

Expected: link check が `OK`。直接 URL、確認日、検証範囲、次の一項目が存在する。

Run: `git add docs/knowledge/materials/agentic-organization-roadmap && git commit -m "docs: add roadmap system map and stage zero"`

---

### Task 3: Stage 1 判断補助と課題の絞り込み

**Files:**
- Create: `docs/knowledge/materials/agentic-organization-roadmap/stages/01-decide.md`
- Create: `docs/knowledge/materials/agentic-organization-roadmap/worksheets/problem-framing.md`
- Modify: `docs/knowledge/materials/agentic-organization-roadmap/roadmap.md`

**Interfaces:**
- Consumes: Stage 0 の一課題。
- Produces: 一成果、採否基準、根拠、責任者、停止条件。

- [x] **Step 1: 判断に必要な情報を調査する**

- `https://oecd.ai/en/dashboards/ai-principles/P7`
- `https://airc.nist.gov/airmf-resources/airmf/5-sec-core/`
- `https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf`

能力・限界・根拠・risk・責任を人間が理解して判断する要素を抽出する。

- [x] **Step 2: Stage 1 と worksheet を作成する**

困りごと、対象者、期待成果、観測方法、期間、対象外、採用・保留・却下基準、最終責任者を一つずつ書く。複数成果、部門、data source、権限変更を同時に含む場合は分割候補とする。

- [x] **Step 3: Evidence と推論を分離して検証する**

公的 source が直接述べる内容は `fact`、本 roadmap が導く手順は `roadmap recommendation` とする。

Run: `./scripts/check-doc-links.sh && rg -n "採用|保留|却下|最終責任|roadmap recommendation|https://" docs/knowledge/materials/agentic-organization-roadmap/stages/01-decide.md`

Expected: 判断 3 分岐、責任者、evidence、推論区分が存在する。

- [x] **Step 4: Commit**

Run: `git add docs/knowledge/materials/agentic-organization-roadmap && git commit -m "docs: add decision and problem framing stage"`

---

### Task 4: Stage 2 一作業の委任

**Files:**
- Create: `docs/knowledge/materials/agentic-organization-roadmap/stages/02-delegate.md`
- Create: `docs/knowledge/materials/agentic-organization-roadmap/worksheets/delegation.md`
- Modify: `docs/knowledge/materials/agentic-organization-roadmap/roadmap.md`

**Interfaces:**
- Consumes: Stage 1 の一成果、採否基準、責任者。
- Produces: 入力、出力、data、権限、承認、停止、検証条件を持つ委任定義。

- [x] **Step 1: Agent 委任の risk と統制を調査する**

- `https://nvlpubs.nist.gov/nistpubs/ai/NIST.AI.600-1.pdf`
- `https://genai.owasp.org/llmrisk/llm062025-excessive-agency/`
- `https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/`

tool、権限、自律範囲、人間承認、logging、停止条件の根拠を抽出する。

- [x] **Step 2: Stage 2 と delegation worksheet を作成する**

目的、入力、許可 tool、禁止事項、参照 data、変更対象、人間承認、期待出力、合否確認、timeout、停止条件へ分解する。高影響操作は human-in-the-loop を既定にする。

- [x] **Step 3: 検証して commit する**

Run: `./scripts/check-doc-links.sh && rg -n "許可|禁止|人間承認|停止条件|https://" docs/knowledge/materials/agentic-organization-roadmap/stages/02-delegate.md`

Expected: 権限、承認、停止、合否、直接 URL が存在する。

Run: `git add docs/knowledge/materials/agentic-organization-roadmap && git commit -m "docs: add bounded delegation stage"`

---

### Task 5: Stage 3 再現性

**Files:**
- Create: `docs/knowledge/materials/agentic-organization-roadmap/stages/03-reproduce.md`
- Modify: `docs/knowledge/materials/agentic-organization-roadmap/roadmap.md`

**Interfaces:**
- Consumes: Stage 2 の委任定義と観測結果。
- Produces: 正本、入力例、例外、version、実行記録。

- [x] **Step 1: 再現性と記録を調査する**

NIST AI RMF Core と GenAI Profile から documentation、measurement、monitoring、condition 記録を確認する。`docs/framework/environment-reproducibility.md` と `docs/framework/agent-handoff.md` は local case として読み、公的 source と混同しない。

- [x] **Step 2: Stage 3 を作成する**

正本、入力条件、期待出力、例外、使用 version、実行日、担当、検証結果を記録する。「同じ prompt」だけを再現性と呼ばず、data、tool、権限、model/service version の変化を含める。

- [x] **Step 3: 検証して commit する**

Run: `./scripts/check-doc-links.sh && rg -n "正本|入力条件|version|例外|検証結果|https://" docs/knowledge/materials/agentic-organization-roadmap/stages/03-reproduce.md`

Expected: 全記録項目と evidence URL が存在する。

Run: `git add docs/knowledge/materials/agentic-organization-roadmap && git commit -m "docs: add reproducible operation stage"`

---

### Task 6: Stage 4 と 5 の役割・権限・集中管理

**Files:**
- Create: `docs/knowledge/materials/agentic-organization-roadmap/stages/04-separate-roles.md`
- Create: `docs/knowledge/materials/agentic-organization-roadmap/stages/05-operate.md`
- Modify: `docs/knowledge/materials/agentic-organization-roadmap/roadmap.md`

**Interfaces:**
- Consumes: 再現可能な一作業。
- Produces: 役割表、project 境界、最小権限、承認経路、集中管理の正本。

- [x] **Step 1: 最小権限と agent risk を調査する**

- `https://csrc.nist.gov/glossary/term/least_privilege`
- `https://csrc.nist.gov/pubs/sp/800/207/final`
- `https://genai.owasp.org/resource/agentic-ai-threats-and-mitigations/`
- `https://genai.owasp.org/llmrisk/llm062025-excessive-agency/`

- [x] **Step 2: Stage 4 を作成する**

判断を人、曖昧さを含む生成・分析を AI、決定済み rule の定期実行を Script へ割り当てる。担当、最終責任、許可、禁止、承認、代替担当を表にする。この割り当ては `roadmap recommendation` と明示する。

- [x] **Step 3: Stage 5 を作成する**

project ごとに目的、data、権限、担当、予算、成果、log を分離し、集中管理層には index、状態、承認待ち、費用、incident を集約する。集中管理を全 data の無制限共有と同義にしない。

- [x] **Step 4: 検証して commit する**

Run: `./scripts/check-doc-links.sh && rg -n "最小権限|最終責任|project|集中管理|無制限共有|https://" docs/knowledge/materials/agentic-organization-roadmap/stages/04-separate-roles.md docs/knowledge/materials/agentic-organization-roadmap/stages/05-operate.md`

Expected: 役割、責任、境界、集中管理、evidence が存在する。

Run: `git add docs/knowledge/materials/agentic-organization-roadmap && git commit -m "docs: add role separation and organization stages"`

---

### Task 7: Stage 6 効果・費用最適化

**Files:**
- Create: `docs/knowledge/materials/agentic-organization-roadmap/stages/06-optimize.md`
- Create: `docs/knowledge/materials/agentic-organization-roadmap/worksheets/evaluation.md`
- Modify: `docs/knowledge/materials/agentic-organization-roadmap/roadmap.md`

**Interfaces:**
- Consumes: project 別の目的、実行、成果、費用、品質記録。
- Produces: 継続、改善、停止、対象拡大の判断表。

- [x] **Step 1: 事業価値と費用最適化を調査する**

- `https://www.finops.org/framework/`
- `https://docs.cloud.google.com/architecture/framework/perspectives/ai-ml/cost-optimization`

Google 固有 tool は例に限定し、事業目標、KPI、cost owner、継続的改善の原則だけを provider-neutral 層へ採用する。

- [x] **Step 2: Stage 6 と evaluation worksheet を作成する**

成果、品質、人の確認時間、token、service 費、失敗、retry、頻度を記録し、`継続`、`一要素だけ改善`、`Script 化`、`停止` の 4 分岐を作る。

- [x] **Step 3: 検証して commit する**

Run: `./scripts/check-doc-links.sh && rg -n "継続|改善|Script 化|停止|token|https://" docs/knowledge/materials/agentic-organization-roadmap/stages/06-optimize.md`

Expected: 4 分岐、費用と成果、evidence が存在する。

Run: `git add docs/knowledge/materials/agentic-organization-roadmap && git commit -m "docs: add value and cost optimization stage"`

---

### Task 8: 正本統合と evidence quality gate

**Files:**
- Modify: `docs/knowledge/materials/agentic-organization-roadmap/README.md`
- Modify: `docs/knowledge/materials/agentic-organization-roadmap/roadmap.md`
- Modify: `docs/knowledge/materials/README.md`
- Create: `docs/work-notes/2026-09-03-agentic-organization-roadmap.md`

**Interfaces:**
- Consumes: Task 1〜7 の profile、stage、worksheet、evidence。
- Produces: 読む順序、現在地診断、次の一歩、変更影響、未対応を辿れる初版。

- [x] **Step 1: Spec coverage を点検する**

設計仕様の第 1〜16 節を読み、各要求を profile、roadmap、stage、worksheet、evidence、work note のいずれかへ対応付ける。case study と Peitho deck は別 plan の未対応として明記する。

- [x] **Step 2: Evidence table を全件監査する**

各 stage の主要主張に `claim_id`、直接 `https://` URL、`source_type`、`checked_at`、`applies_to`、`interpretation`、`review_trigger` があることを確認する。

- [x] **Step 3: 非エンジニア向け表現を監査する**

CLI、Git、API、MCP、token、provider、connector、orchestration、Script の初出説明を確認する。tool 導入なしでも Stage 0 と 1 を実行できることを確認する。

- [x] **Step 4: 機械検査を実行する**

Run: `git diff --check && ./scripts/check-doc-links.sh`

Run: `rg --files-without-match "## Evidence" docs/knowledge/materials/agentic-organization-roadmap/stages/*.md`

Run: `rg --files-without-match "https://" docs/knowledge/materials/agentic-organization-roadmap/stages/*.md`

Run: `rg --files-without-match "checked_at" docs/knowledge/materials/agentic-organization-roadmap/stages/*.md`

Expected: diff と link check が exit 0。3 つの `rg --files-without-match` は何も出力しない。

- [x] **Step 5: Work note を作成する**

作成 artifact、主要 source、検証、未検証の主張、case study と Peitho を別 plan にした理由を記録する。

- [x] **Step 6: Final commit**

Run: `git add docs/knowledge/materials docs/work-notes/2026-09-03-agentic-organization-roadmap.md && git commit -m "docs: complete agentic organization roadmap foundation"`

---

## Follow-up Plans

1. **macOS case study と Windows parity 調査:** 市江氏の環境を実例化し、Windows 代替は目的ごとに検証状態と evidence URL を記録する。
2. **実現 pattern 集:** cloud 同期、remote 操作、CLI/MCP 接続、agentic-framework 集中管理、定期 Script の pattern と選択基準を作る。
3. **Peitho deck:** 正本の `claim_id` を引き継ぎ、湯川塾向けの発表時間、section、speaker notes、footnotes、共通 slide を構成する。
