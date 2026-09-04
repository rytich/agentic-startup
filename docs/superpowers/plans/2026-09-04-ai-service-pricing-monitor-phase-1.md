# AI Service Pricing Monitor Phase 1 Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 初期13 sourceを監視対象として登録し、ChatGPT subscriptionとOpenAI APIを使って、公式source取得、検証、差分検知、人間承認、比較表生成、3日ごとの監視を端から端まで実証する。

**Architecture:** 依存packageを追加せず、Node.js標準機能とJSON互換YAMLを使う。provider registry、承認済み値、観測値、monitor stateを分離し、adapterは公式sourceから正規化した候補だけを返す。変更なしは許可pathだけをbot更新し、価格・条件の変更はobservationとdraft PRへ送り、`approve.mjs`を人間が明示実行するまで比較表へ反映しない。

**Tech Stack:** Node.js ESM、`node:test`、Node.js `fetch` / `crypto` / `fs`、JSON-compatible YAML 1.2、Markdown、GitHub Actions、GitHub CLI。

**Spec:** `docs/superpowers/specs/2026-09-04-ai-service-pricing-monitor-design.md`

## Global Constraints

- Phase 1の自動取得はChatGPT subscriptionとOpenAI APIだけに限定する。残り11 sourceもregistryへ登録するが、`monitoring_mode: manual`として明示し、値を「最新」と表示しない。
- 料金値は実装時に公式sourceをread-onlyでlive確認して記録する。計画書やfixtureに実在providerの価格を固定しない。
- 公式sourceは直接URLを使う。公開日を示さない公式料金pageは、設計で承認済みの`official-live`条件を満たす場合だけ現在値の根拠にする。
- 共有された國光式PDFは表現の参考に限定し、ファイル名、ページ、引用、参照箇所を成果物へ記録しない。
- 共有PDF以外の外部sourceは6か月未満を原則とする。継続更新される公式live pageは、`observed_at`とfingerprintを記録する例外に従う。
- login、cookie、credential、購入、契約、trial開始、bot回避を行わない。取得不能は`manual-review`へ送る。
- 新service/providerを自動追加しない。registry変更は人間が共有・承認した候補だけに限定する。
- 変更PRを自動mergeしない。料金、plan、model、source URL、market、currency、unitの変更はHuman Approverの対象とする。
- fixtureの成功を実利用完了としない。Phase 1完了には2つの公式sourceを使ったread-only live checkが必要である。
- 主対象は`audience-business-leader-ai-user`。用語と表の列は非エンジニアが価格、条件、鮮度、実行経路、承認状態を判断できる粒度にする。
- 既存の依存なし構成を維持するため、`.yaml`はJSON互換のYAML 1.2に限定し`JSON.parse`で読む。一般YAML構文が必要になった場合は別decisionでpackage追加を審査する。
- source HTML全体をfixtureやobservationへ保存しない。価格・条件の検証に必要な最小断片、抽出結果、fingerprintだけを保存する。
- 実装開始前にGitHub Issueを作成し、`1a-m4/ai-pricing-monitor-phase-1` branchで作業する。現在の`develop`へ直接実装commitを追加しない。
- GitHub Actionsのwrite permissionとbot commitはinfrastructure変更としてPR上でHuman Approverの承認を受ける。branch protectionを迂回しない。

---

## File Map

- `data/ai-service-pricing/providers.yaml`: 人間承認済みのprovider/source allowlistと監視方式。
- `data/ai-service-pricing/approved.yaml`: 比較表へ掲載できる承認済みの値。
- `data/ai-service-pricing/monitor-state.yaml`: sourceごとの最終成功、fingerprint、失敗、workflow run URL。
- `data/ai-service-pricing/observations/`: 変更・取得不能・矛盾だけを保存するreview入力。
- `scripts/ai-service-pricing/io.mjs`: JSON互換YAMLの安定read/write。
- `scripts/ai-service-pricing/schema.mjs`: registry、approved、observation、stateのvalidation。
- `scripts/ai-service-pricing/freshness.mjs`: `current` / `delayed` / `stale` / `unavailable`判定。
- `scripts/ai-service-pricing/diff.mjs`: 承認値と観測値の意味差分。
- `scripts/ai-service-pricing/fetch-source.mjs`: HTTPS、allowlist、redirectを強制するread-only fetch。
- `scripts/ai-service-pricing/adapters/index.mjs`: adapter IDから実装への明示mapping。
- `scripts/ai-service-pricing/adapters/chatgpt-subscription.mjs`: ChatGPT料金pageの正規化。
- `scripts/ai-service-pricing/adapters/openai-api.mjs`: OpenAI model/API料金pageの正規化。
- `scripts/ai-service-pricing/check.mjs`: source確認と状態遷移のCLI。
- `scripts/ai-service-pricing/approve.mjs`: 人間が選んだobservationだけを承認値へ反映するCLI。
- `scripts/ai-service-pricing/render.mjs`: subscription表、API表、Peitho入力を同じ承認値から生成。
- `scripts/ai-service-pricing/guard-update-scope.mjs`: bot更新pathと意味差分の制限。
- `scripts/ai-service-pricing/review-output.mjs`: draft PR本文用のold/new/source/warning出力。
- `scripts/test-ai-service-pricing-*.mjs`: unit、integration、CLI、renderer、scope guard test。
- `scripts/fixtures/ai-service-pricing/`: 架空providerの最小HTML断片と各状態のdata。
- `.github/workflows/ai-service-pricing-monitor.yml`: 3日を超えないscheduleとmanual dispatch。
- `docs/knowledge/engineering/ai-service-pricing-monitor.md`: runtime、data flow、権限、障害時運用。
- `docs/knowledge/materials/agentic-organization-roadmap/comparisons/README.md`: 読み方、鮮度、承認状態、非目標。
- `docs/knowledge/materials/agentic-organization-roadmap/comparisons/subscriptions.md`: 承認済みsubscription比較表。
- `docs/knowledge/materials/agentic-organization-roadmap/comparisons/api-pricing.md`: 承認済みAPI比較表。
- `docs/knowledge/materials/agentic-organization-roadmap/comparisons/peitho-pricing.json`: Peithoへ渡す同値data。
- `docs/work-notes/2026-09-04-ai-service-pricing-monitor-phase-1.md`: 実装、検証、live確認、未対応。

---

### Task 0: GitHub Issueとisolated branchを用意する

**Files:**
- Modify after Issue creation: `docs/superpowers/plans/2026-09-04-ai-service-pricing-monitor-phase-1.md`

**Interfaces:**
- Produces: What / Why / How、acceptance criteria、plan URLを持つopen GitHub Issue。
- Produces: `develop`起点の`1a-m4/ai-pricing-monitor-phase-1` branch。

- [ ] **Step 1: remoteの実状態を再取得する**

Run:

```bash
git fetch origin
gh repo view --json nameWithOwner,defaultBranchRef
gh issue list --state open --search 'AI service pricing monitor in:title' --json number,title,url
git status --short --branch
```

Expected: repository、base branch、重複Issue、local変更が確認できる。既存open Issueがあれば新規作成せず再利用する。

- [ ] **Step 2: IssueにWhat / Why / HowとDoneを記録する**

Issue本文は次のacceptance criteriaを含める。

```markdown
## What
- 初期13 sourceをallowlist registryへ登録する
- ChatGPT subscriptionとOpenAI APIのread-only監視を実装する
- 変更なし、変更あり、取得不能を区別する
- 人間承認後だけ比較表を更新する

## Why
- 変化の速いAI料金を固定記事へ転記せず、事業判断に使える鮮度と根拠を維持するため
- 非エンジニアにも価格、条件、鮮度、承認状態を同じ表で示すため

## How
- Node.js標準機能、構造化data、provider adapter、GitHub Actionsを使う
- 仕様と実装計画を正本にし、変更PRはHuman Approverで止める

## Done
- fixture testと2 sourceのread-only live checkが通る
- 比較表とPeitho入力が同じapproved dataから再生成できる
- scheduled workflowの権限とscope guardをPRで確認できる
```

重複Issueがなければ次のtitleで作成し、返されたURLをplanの`Spec`直下へ`**Issue:**`として追記する。

```bash
gh issue create \
  --title "AI service pricing monitor Phase 1" \
  --body "## What
- 初期13 sourceをallowlist registryへ登録する
- ChatGPT subscriptionとOpenAI APIのread-only監視を実装する
- 変更なし、変更あり、取得不能を区別する
- 人間承認後だけ比較表を更新する

## Why
- 変化の速いAI料金を固定記事へ転記せず、事業判断に使える鮮度と根拠を維持するため
- 非エンジニアにも価格、条件、鮮度、承認状態を同じ表で示すため

## How
- Node.js標準機能、構造化data、provider adapter、GitHub Actionsを使う
- 仕様と実装計画を正本にし、変更PRはHuman Approverで止める

## Done
- fixture testと2 sourceのread-only live checkが通る
- 比較表とPeitho入力が同じapproved dataから再生成できる
- scheduled workflowの権限とscope guardをPRで確認できる

## Planning
- docs/superpowers/specs/2026-09-04-ai-service-pricing-monitor-design.md
- docs/superpowers/plans/2026-09-04-ai-service-pricing-monitor-phase-1.md"
```

- [ ] **Step 3: branchを作成しplanへIssue URLを追記する**

Run:

```bash
git switch -c 1a-m4/ai-pricing-monitor-phase-1 develop
```

Expected: `git branch --show-current`が`1a-m4/ai-pricing-monitor-phase-1`。

- [ ] **Step 4: plan linkの差分をcommitする**

Run:

```bash
git add docs/superpowers/plans/2026-09-04-ai-service-pricing-monitor-phase-1.md
git commit -m "docs: link pricing monitor implementation issue"
```

---

### Task 1: Registryとdata contractを作る

**Files:**
- Create: `data/ai-service-pricing/providers.yaml`
- Create: `data/ai-service-pricing/approved.yaml`
- Create: `data/ai-service-pricing/monitor-state.yaml`
- Create: `scripts/fixtures/ai-service-pricing/valid-registry.yaml`
- Create: `scripts/fixtures/ai-service-pricing/invalid-registry.yaml`
- Create: `scripts/test-ai-service-pricing-schema.mjs`
- Create: `scripts/ai-service-pricing/io.mjs`
- Create: `scripts/ai-service-pricing/schema.mjs`

**Interfaces:**

```text
readDataFile(filePath: string) -> Promise<object>
writeDataFile(filePath: string, value: object) -> Promise<void>
validateRegistry(value: object) -> object
validateApproved(value: object) -> object
validateMonitorState(value: object) -> object
validateObservation(value: object) -> object
```

- [ ] **Step 1: schema testを失敗させる**

```js
import test from "node:test";
import assert from "node:assert/strict";
import { validateRegistry } from "./ai-service-pricing/schema.mjs";

test("registryは未承認providerの自動追加を許可しない", () => {
  assert.throws(
    () => validateRegistry({ schema_version: 1, auto_discover: true, sources: [] }),
    /auto_discover must be false/,
  );
});

test("registryはHTTPSかつ明示adapter modeを要求する", () => {
  const registry = {
    schema_version: 1,
    auto_discover: false,
    sources: [{ source_id: "acme", source_url: "http://example.com", monitoring_mode: "http" }],
  };
  assert.throws(() => validateRegistry(registry), /HTTPS/);
});
```

Run: `node --test scripts/test-ai-service-pricing-schema.mjs`

Expected: module missingまたはexport missingでFAIL。

- [ ] **Step 2: JSON互換YAML I/Oを実装する**

```js
export async function readDataFile(filePath) {
  const text = await readFile(filePath, "utf8");
  try {
    return JSON.parse(text);
  } catch (error) {
    throw new Error(`${filePath}: JSON-compatible YAML is required`, { cause: error });
  }
}

export async function writeDataFile(filePath, value) {
  const text = `${JSON.stringify(value, null, 2)}\n`;
  await mkdir(dirname(filePath), { recursive: true });
  const temporary = `${filePath}.tmp`;
  await writeFile(temporary, text, "utf8");
  await rename(temporary, filePath);
}
```

- [ ] **Step 3: 共通fieldと列挙値をfail-fastで検証する**

`schema.mjs`はvalidation errorを全件収集せず、最初の違反をpath付きでthrowする。金額はnumberでなくdecimal文字列、未確認値は`null`、availabilityは`available | limited | unavailable | unknown`だけを許可する。

```js
const VERIFICATION = new Set(["verified", "changed", "unavailable", "manual-review"]);
const AVAILABILITY = new Set(["available", "limited", "unavailable", "unknown"]);
const SOURCE_TYPES = new Set(["official-dated", "official-live"]);

export function validateRegistry(value) {
  assertObject(value, "registry");
  if (value.schema_version !== 1) throw new Error("registry.schema_version must be 1");
  if (value.auto_discover !== false) throw new Error("registry.auto_discover must be false");
  assertUnique(value.sources, (source) => source.source_id, "registry.sources.source_id");
  for (const source of value.sources) validateRegistrySource(source);
  return value;
}
```

同じmoduleにCLI entryを置き、引数で渡された3 fileを順にreadし、それぞれのvalidatorを実行する。引数不足・余分な引数・validation失敗は非0、成功時は検証したfile名だけをJSONで返す。

```js
if (import.meta.url === pathToFileURL(process.argv[1]).href) {
  const [registryPath, approvedPath, statePath, ...extra] = process.argv.slice(2);
  if (!registryPath || !approvedPath || !statePath || extra.length > 0) {
    throw new Error("usage: schema.mjs <providers.yaml> <approved.yaml> <monitor-state.yaml>");
  }
  validateRegistry(await readDataFile(registryPath));
  validateApproved(await readDataFile(approvedPath));
  validateMonitorState(await readDataFile(statePath));
  process.stdout.write(`${JSON.stringify({ validated: [registryPath, approvedPath, statePath] })}\n`);
}
```

- [ ] **Step 4: 初期13 sourceをregistryへ登録する**

`providers.yaml`は設計第6.2節の13 sourceをすべて持つ。次の2 sourceだけを`http`、残りを`manual`にする。

```json
{
  "schema_version": 1,
  "auto_discover": false,
  "sources": [
    {
      "source_id": "chatgpt-subscription-jp",
      "provider_id": "openai",
      "category": "subscription",
      "market": "JP",
      "source_url": "https://chatgpt.com/ja-JP/pricing/",
      "source_type": "official-live",
      "monitoring_mode": "http",
      "adapter_id": "chatgpt-subscription"
    },
    {
      "source_id": "openai-api-global",
      "provider_id": "openai",
      "category": "api",
      "market": "global",
      "source_url": "https://developers.openai.com/api/docs/models/compare",
      "source_type": "official-live",
      "monitoring_mode": "http",
      "adapter_id": "openai-api"
    },
    {
      "source_id": "gemini-subscription-jp",
      "provider_id": "google",
      "category": "subscription",
      "market": "JP",
      "source_url": "https://one.google.com/intl/ja_jp/about/google-ai-plans/",
      "source_type": "official-live",
      "monitoring_mode": "manual",
      "adapter_id": null
    },
    {
      "source_id": "claude-subscription-jp",
      "provider_id": "anthropic",
      "category": "subscription",
      "market": "JP",
      "source_url": "https://claude.com/ja/pricing",
      "source_type": "official-live",
      "monitoring_mode": "manual",
      "adapter_id": null
    },
    {
      "source_id": "microsoft-copilot-subscription-jp",
      "provider_id": "microsoft",
      "category": "subscription",
      "market": "JP",
      "source_url": "https://www.microsoft.com/ja-jp/microsoft-365-copilot/pricing/individuals",
      "source_type": "official-live",
      "monitoring_mode": "manual",
      "adapter_id": null
    },
    {
      "source_id": "perplexity-subscription-global",
      "provider_id": "perplexity",
      "category": "subscription",
      "market": "global",
      "source_url": "https://www.perplexity.ai/help-center/",
      "source_type": "official-live",
      "monitoring_mode": "manual",
      "adapter_id": null
    },
    {
      "source_id": "felo-subscription-global",
      "provider_id": "felo",
      "category": "subscription",
      "market": "global",
      "source_url": "https://felo.ai/search",
      "source_type": "official-live",
      "monitoring_mode": "manual",
      "adapter_id": null
    },
    {
      "source_id": "genspark-subscription-global",
      "provider_id": "genspark",
      "category": "subscription",
      "market": "global",
      "source_url": "https://genspark.ai/pricing",
      "source_type": "official-live",
      "monitoring_mode": "manual",
      "adapter_id": null
    },
    {
      "source_id": "grok-subscription-global",
      "provider_id": "xai",
      "category": "subscription",
      "market": "global",
      "source_url": "https://grok.com/plans",
      "source_type": "official-live",
      "monitoring_mode": "manual",
      "adapter_id": null
    },
    {
      "source_id": "anthropic-api-global",
      "provider_id": "anthropic",
      "category": "api",
      "market": "global",
      "source_url": "https://platform.claude.com/docs/en/about-claude/pricing",
      "source_type": "official-live",
      "monitoring_mode": "manual",
      "adapter_id": null
    },
    {
      "source_id": "google-api-global",
      "provider_id": "google",
      "category": "api",
      "market": "global",
      "source_url": "https://ai.google.dev/gemini-api/docs/pricing",
      "source_type": "official-live",
      "monitoring_mode": "manual",
      "adapter_id": null
    },
    {
      "source_id": "xai-api-global",
      "provider_id": "xai",
      "category": "api",
      "market": "global",
      "source_url": "https://docs.x.ai/developers/models",
      "source_type": "official-live",
      "monitoring_mode": "manual",
      "adapter_id": null
    },
    {
      "source_id": "perplexity-api-global",
      "provider_id": "perplexity",
      "category": "api",
      "market": "global",
      "source_url": "https://docs.perplexity.ai/docs/getting-started/pricing",
      "source_type": "official-live",
      "monitoring_mode": "manual",
      "adapter_id": null
    }
  ]
}
```

`manual` sourceの`adapter_id`は`null`にする。直接料金URLが実装時のlive確認で見つからない場合はtop pageのまま`verification_status: manual-review`とし、自動でURLを差し替えない。

- [ ] **Step 5: 空の承認値と初期stateを作りtestを通す**

```json
{
  "schema_version": 1,
  "subscriptions": [],
  "api_products": []
}
```

Run:

```bash
node --test scripts/test-ai-service-pricing-schema.mjs
node scripts/ai-service-pricing/schema.mjs data/ai-service-pricing/providers.yaml data/ai-service-pricing/approved.yaml data/ai-service-pricing/monitor-state.yaml
```

Expected: all tests PASS、3 data fileがvalid。

- [ ] **Step 6: Commit**

Run:

```bash
git add data/ai-service-pricing scripts/ai-service-pricing/io.mjs scripts/ai-service-pricing/schema.mjs scripts/fixtures/ai-service-pricing scripts/test-ai-service-pricing-schema.mjs
git commit -m "feat: define AI pricing monitor data contracts"
```

---

### Task 2: 鮮度判定と意味差分を実装する

**Files:**
- Create: `scripts/ai-service-pricing/freshness.mjs`
- Create: `scripts/ai-service-pricing/diff.mjs`
- Create: `scripts/test-ai-service-pricing-diff.mjs`

**Interfaces:**

```text
classifyFreshness({ now, lastSuccessfulObservedAt, available }) -> "current" | "delayed" | "stale" | "unavailable"
semanticDiff({ approvedRecords, observedRecords }) -> Change[]
```

- [ ] **Step 1: 境界日と意味差分のtestを失敗させる**

```js
test("4日以内current、5日delayed、8日stale", () => {
  assert.equal(classifyFreshness({ now: "2026-09-09T00:00:00Z", lastSuccessfulObservedAt: "2026-09-05T00:00:00Z" }), "current");
  assert.equal(classifyFreshness({ now: "2026-09-10T00:00:00Z", lastSuccessfulObservedAt: "2026-09-05T00:00:00Z" }), "delayed");
  assert.equal(classifyFreshness({ now: "2026-09-13T00:00:00Z", lastSuccessfulObservedAt: "2026-09-05T00:00:00Z" }), "stale");
});

test("unit変更を単純なprice変更に畳み込まない", () => {
  const [change] = semanticDiff({ approvedRecords: [approved], observedRecords: [{ ...approved, unit: "1K tokens" }] });
  assert.deepEqual(change.changed_fields, ["unit"]);
  assert.equal(change.requires_manual_review, true);
});
```

Run: `node --test scripts/test-ai-service-pricing-diff.mjs`

Expected: importsまたはassertionでFAIL。

- [ ] **Step 2: 鮮度を経過ミリ秒で実装する**

```js
const DAY_MS = 24 * 60 * 60 * 1000;

export function classifyFreshness({ now, lastSuccessfulObservedAt, available = true }) {
  if (!available || !lastSuccessfulObservedAt) return "unavailable";
  const ageDays = Math.floor((Date.parse(now) - Date.parse(lastSuccessfulObservedAt)) / DAY_MS);
  if (ageDays <= 4) return "current";
  if (ageDays <= 7) return "delayed";
  return "stale";
}
```

- [ ] **Step 3: stable keyとfield allowlistで意味差分を実装する**

```js
const IDENTITY_FIELDS = ["category", "provider_id", "product_id", "market"];
const MANUAL_REVIEW_FIELDS = new Set(["market", "currency", "unit", "source_url"]);

export function recordKey(record) {
  return IDENTITY_FIELDS.map((field) => record[field]).join("::");
}

export function semanticDiff({ approvedRecords, observedRecords }) {
  const approvedByKey = new Map(approvedRecords.map((record) => [recordKey(record), record]));
  const observedByKey = new Map(observedRecords.map((record) => [recordKey(record), record]));
  const keys = [...new Set([...approvedByKey.keys(), ...observedByKey.keys()])].sort();
  return keys.map((key) => {
    const before = approvedByKey.get(key) ?? null;
    const after = observedByKey.get(key) ?? null;
    if (!before) return { key, kind: "added", before, after, changed_fields: [], requires_manual_review: true };
    if (!after) return { key, kind: "removed", before, after, changed_fields: [], requires_manual_review: true };
    const fields = [...new Set([...Object.keys(before), ...Object.keys(after)])]
      .filter((field) => !["observed_at", "source_fingerprint", "approved_at", "approved_by"].includes(field))
      .filter((field) => JSON.stringify(before[field] ?? null) !== JSON.stringify(after[field] ?? null))
      .sort();
    return {
      key,
      kind: fields.length === 0 ? "unchanged" : "changed",
      before,
      after,
      changed_fields: fields,
      requires_manual_review: fields.some((field) => MANUAL_REVIEW_FIELDS.has(field)),
    };
  });
}
```

実装は`price`、`plan`、`model_id`、tool料金、条件、availabilityの差を保持し、`observed_at`、fingerprint、approval metadataだけの差を`unchanged`とする。added recordは登録済みprovider内の`candidate`として扱う。

- [ ] **Step 4: testを通す**

Run: `node --test scripts/test-ai-service-pricing-diff.mjs`

Expected: current/delayed/stale/unavailable、added/removed/changed/unchanged、unit/currency/market/source変更がPASS。

- [ ] **Step 5: Commit**

Run:

```bash
git add scripts/ai-service-pricing/freshness.mjs scripts/ai-service-pricing/diff.mjs scripts/test-ai-service-pricing-diff.mjs
git commit -m "feat: classify pricing freshness and semantic changes"
```

---

### Task 3: 比較表とPeitho入力を同じdataから生成する

**Files:**
- Create: `scripts/ai-service-pricing/render.mjs`
- Create: `scripts/test-ai-service-pricing-render.mjs`
- Create: `scripts/fixtures/ai-service-pricing/approved-sample.yaml`
- Create: `scripts/fixtures/ai-service-pricing/state-sample.yaml`
- Create: `docs/knowledge/materials/agentic-organization-roadmap/comparisons/README.md`
- Create: `docs/knowledge/materials/agentic-organization-roadmap/comparisons/subscriptions.md`
- Create: `docs/knowledge/materials/agentic-organization-roadmap/comparisons/api-pricing.md`
- Create: `docs/knowledge/materials/agentic-organization-roadmap/comparisons/peitho-pricing.json`
- Modify: `docs/knowledge/materials/agentic-organization-roadmap/README.md`

**Interfaces:**

```text
buildComparisonModel({ approved, monitorState, generatedAt }) -> ComparisonModel
renderSubscriptions(model: ComparisonModel) -> string
renderApiPricing(model: ComparisonModel) -> string
renderPeithoPricing(model: ComparisonModel) -> string
writeRenderedOutputs({ model, outputDir }) -> Promise<void>
```

- [ ] **Step 1: 3出力の同値性testを失敗させる**

```js
test("MarkdownとPeitho入力は同じ承認値を使う", () => {
  const model = buildComparisonModel({ approved, monitorState, generatedAt: "2026-09-09T00:00:00Z" });
  const subscriptionMarkdown = renderSubscriptions(model);
  const apiMarkdown = renderApiPricing(model);
  const peitho = JSON.parse(renderPeithoPricing(model));
  assert.match(subscriptionMarkdown, /Acme Pro/);
  assert.match(apiMarkdown, /acme-model/);
  assert.equal(peitho.subscriptions[0].product_id, model.subscriptions[0].product_id);
  assert.equal(peitho.api_products[0].output_price, model.api_products[0].output_price);
});
```

Run: `node --test scripts/test-ai-service-pricing-render.mjs`

Expected: module missingでFAIL。

- [ ] **Step 2: 表示modelを作る**

`buildComparisonModel`はapproved dataへfreshnessをjoinする。`stale`と`unavailable`は削除せず警告表示し、推薦や最安値判定へ使えるfieldを出力しない。

```js
return {
  schema_version: 1,
  generated_at: generatedAt,
  subscriptions: approved.subscriptions.map(withFreshness),
  api_products: approved.api_products.map(withFreshness),
};
```

- [ ] **Step 3: subscription/APIの列を固定する**

subscriptionは`service / plan / 対象 / 月額 / 年額 / 通貨 / 税 / 最低seat / agent機能 / data統制 / 利用上限 / 実行経路 / 鮮度 / source`、APIは`provider / model / status / input / output / cache / tool料金 / context / rate-limit条件 / market / 鮮度 / source`をこの順で出す。

- [ ] **Step 4: Peitho向けdataに説明用の比較軸を加える**

```js
const executionPaths = [
  { id: "api-cli", label: "API / CLI", use: "定型処理・batch", primary_risk: "導入知識" },
  { id: "mcp", label: "MCP", use: "tool・data接続", primary_risk: "serverと権限管理" },
  { id: "computer-use", label: "Computer Use", use: "APIがない画面", primary_risk: "UI変更・誤操作・prompt injection" },
  { id: "hybrid", label: "Hybrid", use: "安定処理と未接続部分", primary_risk: "経路別の監査と停止" },
];
```

小規模組織の説明は`small_is_safe: false`とし、`scope / data / identity / connections / permissions / approval / logs / stop`の8統制を出力する。

CLIは`--approved`、`--state`、`--output`を各1回だけ要求し、未知flagを拒否する。3 fileは一時fileへ書いてからrenameし、途中失敗で部分更新しない。

- [ ] **Step 5: test、再生成、link checkを通す**

Run:

```bash
node --test scripts/test-ai-service-pricing-render.mjs
node scripts/ai-service-pricing/render.mjs --approved data/ai-service-pricing/approved.yaml --state data/ai-service-pricing/monitor-state.yaml --output docs/knowledge/materials/agentic-organization-roadmap/comparisons
./scripts/check-doc-links.sh
git diff --exit-code -- docs/knowledge/materials/agentic-organization-roadmap/comparisons
```

Expected: testsとlink checkがPASS、再生成差分0。

- [ ] **Step 6: Commit**

Run:

```bash
git add scripts/ai-service-pricing/render.mjs scripts/test-ai-service-pricing-render.mjs scripts/fixtures/ai-service-pricing docs/knowledge/materials/agentic-organization-roadmap
git commit -m "feat: render approved AI pricing comparisons"
```

---

### Task 4: 安全な公式source取得を実装する

**Files:**
- Create: `scripts/ai-service-pricing/fetch-source.mjs`
- Create: `scripts/test-ai-service-pricing-fetch.mjs`

**Interfaces:**

```text
assertAllowedSourceUrl({ sourceUrl, allowedHosts }) -> URL
fetchOfficialSource({ sourceUrl, allowedHosts, fetchImpl, timeoutMs }) -> Promise<OfficialSourceResponse>
```

- [ ] **Step 1: URLとredirect拒否testを失敗させる**

```js
test("HTTP downgradeとallowlist外redirectを拒否する", async () => {
  assert.throws(
    () => assertAllowedSourceUrl({ sourceUrl: "http://example.com/pricing", allowedHosts: ["example.com"] }),
    /HTTPS/,
  );
  await assert.rejects(
    fetchOfficialSource({
      sourceUrl: "https://example.com/pricing",
      allowedHosts: ["example.com"],
      fetchImpl: async () => new Response(null, { status: 302, headers: { location: "https://login.example.net/" } }),
    }),
    /redirect host is not allowed/,
  );
});
```

Run: `node --test scripts/test-ai-service-pricing-fetch.mjs`

Expected: module missingでFAIL。

- [ ] **Step 2: manual redirectとtimeoutを実装する**

```js
const response = await fetchImpl(currentUrl, {
  method: "GET",
  redirect: "manual",
  signal: AbortSignal.timeout(timeoutMs),
  headers: { "user-agent": "agentic-startup-pricing-monitor/1.0" },
});
```

最大5 redirect、HTTPS維持、registryで許可されたhostだけを認める。`401 / 403 / 429`、login redirect、JavaScript challengeはretryで回避せず、型付きerrorで`manual-review`へ渡す。

- [ ] **Step 3: responseを最小情報へ制限する**

返却値は`final_url`、`status`、`content_type`、`body`、`fetched_at`だけにする。headerのcookie、authorization、set-cookieはlog/observationへ渡さない。最大body sizeを5 MiBとし、超過を拒否する。

- [ ] **Step 4: testを通す**

Run: `node --test scripts/test-ai-service-pricing-fetch.mjs`

Expected: HTTPS、redirect、timeout、oversize、401/403/429、HTML successがPASS。

- [ ] **Step 5: Commit**

Run:

```bash
git add scripts/ai-service-pricing/fetch-source.mjs scripts/test-ai-service-pricing-fetch.mjs
git commit -m "feat: constrain official pricing source fetches"
```

---

### Task 5: ChatGPT subscription adapterを実装・live検証する

**Files:**
- Create: `scripts/ai-service-pricing/adapters/chatgpt-subscription.mjs`
- Create: `scripts/ai-service-pricing/adapters/index.mjs`
- Create: `scripts/fixtures/ai-service-pricing/chatgpt-subscription-minimal.html`
- Create: `scripts/test-ai-service-pricing-chatgpt-adapter.mjs`
- Modify: `data/ai-service-pricing/approved.yaml`
- Modify: `data/ai-service-pricing/monitor-state.yaml`

**Interfaces:**

```text
observeChatGptSubscription({ html, source, observedAt }) -> ObservationResult
getAdapter(adapterId: string) -> AdapterFunction
```

- [ ] **Step 1: 公式pageをread-onlyで再確認する**

Run:

```bash
node -e 'fetch("https://chatgpt.com/ja-JP/pricing/", {redirect:"manual"}).then(async r => console.log(JSON.stringify({status:r.status,location:r.headers.get("location"),contentType:r.headers.get("content-type"),bytes:(await r.arrayBuffer()).byteLength})))'
```

Expected: status、redirect、content type、body sizeだけを表示する。本文全体はterminal、fixture、commitへ保存しない。取得不能ならこのTaskの値登録を止め、`manual-review`の観測だけを作る。

- [ ] **Step 2: 価格・条件に必要な最小断片を特定する**

公式pageのplan名、対象区分、月/年、currency、税、主要条件の近傍だけをfixtureへ手作業で縮小する。個人情報、cookie、script全体、page本文全体を含めない。

- [ ] **Step 3: 欠落と構造変更testを先に失敗させる**

```js
test("plan、currency、billing periodが揃った候補を返す", () => {
  const result = observeChatGptSubscription({ html: fixture, source, observedAt: "2026-09-04T00:00:00Z" });
  assert.equal(result.verification_status, "verified");
  assert.ok(result.records.every((record) => record.category === "subscription"));
  assert.ok(result.records.every((record) => record.currency && record.source_fingerprint));
});

test("必須labelが消えたら0円にせずmanual-review", () => {
  const result = observeChatGptSubscription({ html: "<main>plans</main>", source, observedAt: "2026-09-04T00:00:00Z" });
  assert.equal(result.verification_status, "manual-review");
  assert.deepEqual(result.records, []);
});
```

Run: `node --test scripts/test-ai-service-pricing-chatgpt-adapter.mjs`

Expected: module missingでFAIL。

- [ ] **Step 4: semantic labelを基準にadapterを実装する**

```js
export function observeChatGptSubscription({ html, source, observedAt }) {
  const normalizedText = normalizeVisibleText(html);
  const extracted = extractSubscriptionPlans(normalizedText);
  if (extracted.length === 0) return manualReview("required pricing labels were not found");
  return {
    source_id: source.source_id,
    observed_at: observedAt,
    verification_status: "verified",
    records: extracted.map((plan) => normalizeSubscription(plan, source, observedAt)),
  };
}
```

CSS class hashやelement順序だけに依存しない。plan名、通貨記号、billing period、説明labelを組み合わせる。campaignと通常価格を別fieldにし、年額月換算を原値へ上書きしない。

- [ ] **Step 5: fixture testを通しlive observationをreviewする**

Run:

```bash
node --test scripts/test-ai-service-pricing-chatgpt-adapter.mjs
node --input-type=module -e '
import { readDataFile } from "./scripts/ai-service-pricing/io.mjs";
import { fetchOfficialSource } from "./scripts/ai-service-pricing/fetch-source.mjs";
import { observeChatGptSubscription } from "./scripts/ai-service-pricing/adapters/chatgpt-subscription.mjs";
const registry = await readDataFile("data/ai-service-pricing/providers.yaml");
const source = registry.sources.find((item) => item.source_id === "chatgpt-subscription-jp");
const response = await fetchOfficialSource({ sourceUrl: source.source_url, allowedHosts: [new URL(source.source_url).hostname] });
const result = observeChatGptSubscription({ html: response.body, source, observedAt: response.fetched_at });
process.stdout.write(`${JSON.stringify(result, null, 2)}\n`);
'
```

Expected: fixture test PASS。live出力にsource URL、observed_at、market、currency、plan、fingerprintがあり、raw HTMLがない。人間が公式pageと値を照合する。

- [ ] **Step 6: 人間確認済みの値だけをapprovedへ初期登録する**

`approved_at`と`approved_by: human-approver`を付ける。確認できないplanは登録せず、`manual-review`のまま残す。

- [ ] **Step 7: Commit**

Run:

```bash
git add scripts/ai-service-pricing/adapters scripts/fixtures/ai-service-pricing scripts/test-ai-service-pricing-chatgpt-adapter.mjs data/ai-service-pricing
git commit -m "feat: observe ChatGPT subscription pricing"
```

---

### Task 6: OpenAI API adapterを実装・live検証する

**Files:**
- Create: `scripts/ai-service-pricing/adapters/openai-api.mjs`
- Create: `scripts/fixtures/ai-service-pricing/openai-api-minimal.html`
- Create: `scripts/test-ai-service-pricing-openai-adapter.mjs`
- Modify: `scripts/ai-service-pricing/adapters/index.mjs`
- Modify: `data/ai-service-pricing/approved.yaml`
- Modify: `data/ai-service-pricing/monitor-state.yaml`

**Interfaces:**

```text
observeOpenAiApi({ html, source, observedAt }) -> ObservationResult
```

- [ ] **Step 1: 公式model比較pageをread-onlyで再確認する**

Run:

```bash
node -e 'fetch("https://developers.openai.com/api/docs/models/compare", {redirect:"manual"}).then(async r => console.log(JSON.stringify({status:r.status,location:r.headers.get("location"),contentType:r.headers.get("content-type"),bytes:(await r.arrayBuffer()).byteLength})))'
```

Expected: metadataだけを表示する。pageが価格を直接示さなくなった場合は、同一公式domain内の直接料金URLをregistryへ変更するcandidateとして提示し、自動変更しない。

- [ ] **Step 2: model IDと料金単位を含む最小fixtureを作る**

input、output、cache、unit、context、statusのうちsourceで直接確認できたfieldだけを含める。確認できないfieldは`null`または`unknown`にし、推測で補わない。

- [ ] **Step 3: model追加と単位欠落testを失敗させる**

```js
test("登録済みproviderの新modelをcandidateとして返す", () => {
  const result = observeOpenAiApi({ html: fixtureWithTwoModels, source, observedAt: "2026-09-04T00:00:00Z" });
  assert.equal(result.records.length, 2);
  assert.ok(result.records.every((record) => record.unit === "1M tokens"));
});

test("単位不明のpriceはmanual-review", () => {
  const result = observeOpenAiApi({ html: fixtureWithoutUnit, source, observedAt: "2026-09-04T00:00:00Z" });
  assert.equal(result.verification_status, "manual-review");
});
```

Run: `node --test scripts/test-ai-service-pricing-openai-adapter.mjs`

Expected: module missingでFAIL。

- [ ] **Step 4: API料金を原単位のまま正規化する**

```js
return {
  category: "api",
  provider_id: source.provider_id,
  product_id: model.id,
  model_id: model.id,
  status: model.status ?? "unknown",
  market: source.market,
  currency: model.currency,
  unit: model.unit,
  input_price: model.input_price,
  output_price: model.output_price,
  cache_read_price: model.cache_read_price ?? null,
  cache_write_price: model.cache_write_price ?? null,
  source_url: source.source_url,
  observed_at: observedAt,
  source_fingerprint: fingerprint(model),
};
```

decimalは文字列のまま保持する。JPY換算、最安値、性能順位は生成しない。long context、batch、priority、tool料金は通常単価を上書きせず別条件として保持する。

- [ ] **Step 5: fixture testとlive dry-runを通す**

Run:

```bash
node --test scripts/test-ai-service-pricing-openai-adapter.mjs
node --input-type=module -e '
import { readDataFile } from "./scripts/ai-service-pricing/io.mjs";
import { fetchOfficialSource } from "./scripts/ai-service-pricing/fetch-source.mjs";
import { observeOpenAiApi } from "./scripts/ai-service-pricing/adapters/openai-api.mjs";
const registry = await readDataFile("data/ai-service-pricing/providers.yaml");
const source = registry.sources.find((item) => item.source_id === "openai-api-global");
const response = await fetchOfficialSource({ sourceUrl: source.source_url, allowedHosts: [new URL(source.source_url).hostname] });
const result = observeOpenAiApi({ html: response.body, source, observedAt: response.fetched_at });
process.stdout.write(`${JSON.stringify(result, null, 2)}\n`);
'
```

Expected: fixture test PASS。live出力がmodel、currency、unit、input/output、source、observed_at、fingerprintを持つ。人間が公式pageと照合する。

- [ ] **Step 6: 人間確認済みの値だけをapprovedへ初期登録しcommitする**

Run:

```bash
git add scripts/ai-service-pricing/adapters scripts/fixtures/ai-service-pricing scripts/test-ai-service-pricing-openai-adapter.mjs data/ai-service-pricing
git commit -m "feat: observe OpenAI API pricing"
```

---

### Task 7: Checker、observation、承認CLIを実装する

**Files:**
- Create: `scripts/ai-service-pricing/check.mjs`
- Create: `scripts/ai-service-pricing/approve.mjs`
- Create: `scripts/ai-service-pricing/review-output.mjs`
- Create: `scripts/test-ai-service-pricing-check.mjs`
- Create: `scripts/test-ai-service-pricing-cli.mjs`

**Interfaces:**

```text
runCheck({ registry, approved, monitorState, selectedSourceIds, fetchImpl, now, dryRun }) -> Promise<CheckResult>
buildReviewOutput({ observation, diff }) -> string
approveObservation({ observationPath, approvedPath, approver, approvedAt }) -> Promise<ApprovalResult>
```

- [ ] **Step 1: 4状態のintegration testを失敗させる**

```js
for (const expected of ["unchanged", "changed", "unavailable", "manual-review"]) {
  test(`checkerは${expected}を区別する`, async () => {
    const result = await runScenario(expected);
    assert.equal(result.sources[0].outcome, expected);
  });
}

test("dry-runはfileを書き換えない", async () => {
  const before = await snapshotTree(tempDir);
  await runCheck({ ...context, dryRun: true });
  assert.deepEqual(await snapshotTree(tempDir), before);
});
```

Run: `node --test scripts/test-ai-service-pricing-check.mjs`

Expected: module missingでFAIL。

- [ ] **Step 2: 状態遷移を実装する**

```js
switch (outcome) {
  case "unchanged":
    updateSuccessfulState();
    break;
  case "changed":
  case "manual-review":
  case "unavailable":
    writeObservation();
    updateAttemptState();
    break;
  default:
    throw new Error(`unsupported outcome: ${outcome}`);
}
```

`unchanged`はapprovedをwriteしない。`changed`はobservationだけを作る。`unavailable`は前回承認値を保持し、last successを進めない。1 source失敗で他sourceの確認を捨てず、process exitはreview必要を表す固定codeへ集約する。

- [ ] **Step 3: review出力を実装する**

```markdown
## acme / acme-pro

- Outcome: changed
- Market / currency / unit: JP / JPY / month
- Source: https://pricing.example.test/subscriptions
- Observed at: 2026-09-04T00:00:00Z
- Warning: none

| Field | Approved | Observed |
|---|---|---|
```

raw HTML、cookie、header、個人情報は含めない。追加model/planは`candidate`、削除は`removed-candidate`と表示する。

- [ ] **Step 4: 承認CLIを明示指定に限定する**

```bash
node scripts/ai-service-pricing/approve.mjs \
  --observation data/ai-service-pricing/observations/2026-09-04-openai-api-global.yaml \
  --approver human-approver \
  --approved-at 2026-09-04T12:00:00+09:00
```

`--all`、latest自動選択、環境変数だけでの承認を実装しない。observationのsource fingerprintとregistry URLが一致しない場合は拒否する。承認後にrendererを実行し、approved、state、3派生物を同一transaction相当で更新する。

- [ ] **Step 5: CLI testを通す**

Run:

```bash
node --test scripts/test-ai-service-pricing-check.mjs scripts/test-ai-service-pricing-cli.mjs
node scripts/ai-service-pricing/check.mjs --help
node scripts/ai-service-pricing/approve.mjs --help
```

Expected: 4状態、dry-run、個別承認、不正fingerprint、未知source、manual source skipがPASS。

- [ ] **Step 6: Commit**

Run:

```bash
git add scripts/ai-service-pricing/check.mjs scripts/ai-service-pricing/approve.mjs scripts/ai-service-pricing/review-output.mjs scripts/test-ai-service-pricing-check.mjs scripts/test-ai-service-pricing-cli.mjs
git commit -m "feat: gate pricing observations with human approval"
```

---

### Task 8: Bot更新scope guardを実装する

**Files:**
- Create: `scripts/ai-service-pricing/guard-update-scope.mjs`
- Create: `scripts/test-ai-service-pricing-scope.mjs`

**Interfaces:**

```text
validateAutomatedDiff({ changedPaths, approvedBefore, approvedAfter }) -> void
```

- [ ] **Step 1: 許可・拒否path testを失敗させる**

```js
test("unchanged runはstateと鮮度表示だけを許可する", () => {
  assert.doesNotThrow(() => validateAutomatedDiff({
    changedPaths: [
      "data/ai-service-pricing/monitor-state.yaml",
      "docs/knowledge/materials/agentic-organization-roadmap/comparisons/subscriptions.md",
      "docs/knowledge/materials/agentic-organization-roadmap/comparisons/api-pricing.md",
      "docs/knowledge/materials/agentic-organization-roadmap/comparisons/peitho-pricing.json",
    ],
    approvedBefore,
    approvedAfter: approvedBefore,
  }));
});

test("approved変更を拒否する", () => {
  assert.throws(() => validateAutomatedDiff({
    changedPaths: ["data/ai-service-pricing/approved.yaml"],
    approvedBefore,
    approvedAfter: changedApproved,
  }), /approved data cannot be changed automatically/);
});
```

Run: `node --test scripts/test-ai-service-pricing-scope.mjs`

Expected: module missingでFAIL。

- [ ] **Step 2: allowlistとapproved byte equalityを実装する**

```js
const UNCHANGED_RUN_PATHS = new Set([
  "data/ai-service-pricing/monitor-state.yaml",
  "docs/knowledge/materials/agentic-organization-roadmap/comparisons/subscriptions.md",
  "docs/knowledge/materials/agentic-organization-roadmap/comparisons/api-pricing.md",
  "docs/knowledge/materials/agentic-organization-roadmap/comparisons/peitho-pricing.json",
]);
```

派生表は`generated_at`、鮮度、最終確認日時、監視状態以外の値が変わっていないことを比較modelでも検証する。Git pathだけのguardにしない。

- [ ] **Step 3: testを通しcommitする**

Run:

```bash
node --test scripts/test-ai-service-pricing-scope.mjs
git add scripts/ai-service-pricing/guard-update-scope.mjs scripts/test-ai-service-pricing-scope.mjs
git commit -m "test: guard automated pricing monitor updates"
```

---

### Task 9: 3日ごとのGitHub Actionsを実装する

**Files:**
- Create: `.github/workflows/ai-service-pricing-monitor.yml`
- Create: `scripts/test-ai-service-pricing-workflow.mjs`
- Modify: `docs/knowledge/engineering/README.md`
- Create: `docs/knowledge/engineering/ai-service-pricing-monitor.md`

**Interfaces:**
- Schedule: `0 0 1,4,7,10,13,16,19,22,25,28,31 * *`（09:00 JST）。
- Manual input: `source_id`、`dry_run`。
- Workflow permission: `contents: write`、`pull-requests: write`。他permissionは`none`。

- [ ] **Step 1: workflow contract testを失敗させる**

```js
test("scheduleは月内最大3日間隔でJST 09:00に起動する", async () => {
  const workflow = await readFile(".github/workflows/ai-service-pricing-monitor.yml", "utf8");
  assert.match(workflow, /0 0 1,4,7,10,13,16,19,22,25,28,31 \* \*/);
  assert.match(workflow, /pull-requests: write/);
  assert.doesNotMatch(workflow, /id-token: write/);
});
```

Run: `node --test scripts/test-ai-service-pricing-workflow.mjs`

Expected: workflow missingでFAIL。

- [ ] **Step 2: read-only check jobを作る**

```yaml
on:
  schedule:
    - cron: "0 0 1,4,7,10,13,16,19,22,25,28,31 * *"
  workflow_dispatch:
    inputs:
      source_id:
        description: "Optional registered source ID"
        required: false
        type: string
      dry_run:
        description: "Do not write repository state"
        required: true
        default: true
        type: boolean
permissions:
  contents: write
  pull-requests: write
```

checkout後に全test、schema validation、`check.mjs`を実行する。source IDはregistry存在確認し、URL入力は受け付けない。

- [ ] **Step 3: outcome別のwrite動作を実装する**

- `unchanged`: scope guard成功後だけmonitor stateと派生鮮度をbot commitする。protected branchが拒否した場合は迂回せずworkflowをFAILにする。
- `changed`: `automation/ai-pricing-review-${SOURCE_ID}` branchへobservationとreview Markdownだけをcommitし、既存open PRを更新またはdraft PRを作る。
- `manual-review` / `unavailable`: observationを同じdraft PRへ追加し、approved dataを変更しない。
- `dry_run`: artifactへreview出力だけを保存し、branch、commit、PRを作らない。

PR重複判定は実行直前に次でremote stateを取得する。

```bash
gh pr list --base develop --head "automation/ai-pricing-review-${SOURCE_ID}" --state open --json number,url
```

- [ ] **Step 4: bot権限と停止手順をdocument化する**

engineering noteへ、必要permission、schedule停止、manual dry-run、失敗時のlast success維持、draft承認手順、branch protection、raw source非保存を書く。botが直接`approved.yaml`を変更できないことを明記する。

- [ ] **Step 5: workflow testとdry-runを通す**

Run:

```bash
node --test scripts/test-ai-service-pricing-workflow.mjs scripts/test-ai-service-pricing-scope.mjs
node scripts/ai-service-pricing/check.mjs --all-http --dry-run --json
./scripts/check-doc-links.sh
```

Expected: test PASS。live dry-runはrepository、GitHub branch、PRを変更しない。

- [ ] **Step 6: Human Approverへinfrastructure reviewを依頼してcommitする**

Run:

```bash
git add .github/workflows/ai-service-pricing-monitor.yml scripts/test-ai-service-pricing-workflow.mjs docs/knowledge/engineering
git commit -m "ci: monitor AI pricing sources every three days"
```

workflow write permissionを含むため、PRをready/mergeする前にHuman Approverがpath、permission、token scope、branch protectionを確認する。

---

### Task 10: End-to-end検証とPhase 2境界を記録する

**Files:**
- Create: `docs/work-notes/2026-09-04-ai-service-pricing-monitor-phase-1.md`
- Modify: `docs/knowledge/materials/agentic-organization-roadmap/comparisons/README.md`
- Modify: `docs/superpowers/specs/2026-09-04-ai-service-pricing-monitor-design.md`

**Interfaces:**
- Produces: fixtureとliveを分離した検証記録。
- Produces: 残り11 sourceのadapter化条件と優先順位。

- [ ] **Step 1: 全unit/integration testを実行する**

Run:

```bash
node --test scripts/test-ai-service-pricing-*.mjs
node --check scripts/ai-service-pricing/*.mjs
node --check scripts/ai-service-pricing/adapters/*.mjs
```

Expected: all PASS、syntax error 0。

- [ ] **Step 2: schema、再生成、docsを検証する**

Run:

```bash
node scripts/ai-service-pricing/schema.mjs data/ai-service-pricing/providers.yaml data/ai-service-pricing/approved.yaml data/ai-service-pricing/monitor-state.yaml
node scripts/ai-service-pricing/render.mjs --approved data/ai-service-pricing/approved.yaml --state data/ai-service-pricing/monitor-state.yaml --output docs/knowledge/materials/agentic-organization-roadmap/comparisons
git diff --exit-code -- docs/knowledge/materials/agentic-organization-roadmap/comparisons
./scripts/check-doc-links.sh
git diff --check
```

Expected: schema PASS、生成差分0、broken link 0、whitespace error 0。

- [ ] **Step 3: real-use gateを実行する**

Run:

```bash
node scripts/ai-service-pricing/check.mjs --source chatgpt-subscription-jp --dry-run --json
node scripts/ai-service-pricing/check.mjs --source openai-api-global --dry-run --json
```

Expected: 公式sourceへread-onlyで接続し、`unchanged`、`changed`、`manual-review`、`unavailable`のいずれかを正しく返す。取得不能を成功扱いしない。人間がsourceと正規化値を照合した結果をwork noteへ書く。

- [ ] **Step 4: securityとsecret checklistを実施する**

Run:

```bash
rg -n "authorization|set-cookie|cookie|api[_-]?key|bearer" data/ai-service-pricing scripts/fixtures/ai-service-pricing docs/knowledge/materials/agentic-organization-roadmap/comparisons
git diff --cached --check
```

Expected: credential値0。説明文・test名の語だけが出る場合はwork noteへ記録する。

- [ ] **Step 5: Phase 2対象を明示する**

comparisons READMEとwork noteに次を記録する。

```markdown
## Phase 2候補

- Subscription: Gemini、Claude、Microsoft Copilot、Perplexity、Felo、Genspark、Grok
- API: Anthropic、Google、xAI、Perplexity
- 着手条件: 直接公式URL、取得可否、最小fixture、必須field、manual fallbackをsourceごとに調査し、人間がadapter planをレビューする
- 表示条件: live確認とHuman Approverの承認が終わるまで「監視準備中」または「手動確認」と表示し、「最新」と表現しない
```

- [ ] **Step 6: work noteを完成させる**

fixture test、live test、GitHub Actions dry-run、Human Approver確認、未実装provider、既知の制約、rollback（workflow disableとbot branch停止）を分離して記録する。

- [ ] **Step 7: metrics doctorとrepository状態を確認する**

Run:

```bash
node scripts/metrics.mjs doctor --completion-warning
git status --short --branch
git log --oneline --decorate -12
```

Expected: metrics警告は記録するがcompletionの代替にしない。task外差分がない。

- [ ] **Step 8: Commit**

Run:

```bash
git add docs/work-notes/2026-09-04-ai-service-pricing-monitor-phase-1.md docs/knowledge/materials/agentic-organization-roadmap/comparisons docs/superpowers/specs/2026-09-04-ai-service-pricing-monitor-design.md
git commit -m "docs: record pricing monitor phase one validation"
```

---

### Task 11: Objective review、PR、Human Approvalで完了する

**Files:**
- Create by standard pipeline: `docs/work-notes/*-objective-review.md`
- Modify if review finds defects: files in the exact failing task only.

- [ ] **Step 1: GitHub実状態を再取得する**

Run:

```bash
ISSUE_NUMBER="$(gh issue list --state open --search 'AI service pricing monitor Phase 1 in:title' --json number --jq '.[0].number // empty')"
test -n "$ISSUE_NUMBER"
gh issue view "$ISSUE_NUMBER" --json state,title,url,updatedAt
gh pr list --base develop --head 1a-m4/ai-pricing-monitor-phase-1 --state open --json number,url,headRefOid
git rev-parse HEAD
```

Expected: Issue OPEN、PR重複なしまたは再利用対象1件、review対象headが確定する。

- [ ] **Step 2: 標準pipelineをmergeなしで実行する**

Run:

```bash
VALIDATION_COMMANDS=$'node --test scripts/test-ai-service-pricing-*.mjs\nnode --check scripts/ai-service-pricing/*.mjs\nnode --check scripts/ai-service-pricing/adapters/*.mjs\n./scripts/check-doc-links.sh\ngit diff --check' \
  scripts/complete-task.sh \
  --issue "$ISSUE_NUMBER" \
  --base develop \
  --stage-all \
  --commit-message "feat: complete AI pricing monitor phase one"
```

Expected: objective review PASS、PR作成または再利用。`--merge`と`--close-issue`はworkflow write permissionのHuman Approval前には付けない。

- [ ] **Step 3: 人間レビュー項目を確認する**

- ChatGPT/OpenAIの表示値が公式sourceと一致する。
- source URL、market、currency、unit、observed_at、fingerprintが揃う。
- workflow permissionが`contents`と`pull-requests`の必要範囲だけである。
- botが`approved.yaml`を変更できない。
- changed PRがdraftで作成され、自動mergeされない。
- manual 11 sourceが「最新」と表示されない。
- Computer Useと小さく統制されたAI組織の説明が設計と一致する。

- [ ] **Step 4: review後のexact headを再確認してmergeする**

Human Approverが承認した場合だけ、GitHub実状態とhead SHAを再取得して標準pipelineの`--merge --close-issue`を実行する。承認がない場合はPRとIssueをopenのままhandoffする。

---

## Phase 1完了後の次計画

Phase 1でsource取得、正規化、承認、表示、schedulerのcontractが実証できた後に、残り11 sourceを次の2 planへ分ける。

1. Subscription adapter expansion: Gemini、Claude、Microsoft Copilot、Perplexity、Felo、Genspark、Grok。
2. API adapter expansion: Anthropic、Google、xAI、Perplexity。

各planはsourceごとのlive調査結果を先に残し、HTTP自動取得、Computer Useによる手動補助、人間確認のどれを使うかを個別に決める。Phase 1の汎用adapterへ無理に合わせず、取得不能を明示する方を優先する。
