# Codex Development Stack

## Purpose

macOS 上の Codex CLI / Codex desktop で、Spec Kit、context-mode、Serena を組み合わせた開発スタックを再現するための project-neutral guide。

Project runtime は `docs/knowledge/engineering/runtime.md` に書く。この document は terminal-wide AI development stack を扱う。

## Stack

- Spec Kit: `specify -> plan -> tasks -> implement` の spec-driven workflow。
- context-mode: large-output handling、session continuity、token-efficient analysis。
- Serena: semantic code navigation、symbol search、references、diagnostics、refactoring。
- Codex: implementation、verification、handoff。
- Symphony（任意）: issue tracker を control plane にした無人継続実行。対話セッションではなく常駐サービスとして動かす。

## Assumptions

- macOS。
- Codex CLI が `codex` として利用できる。
- Homebrew が利用できる。
- `uv` が利用できる。
- GitHub/network access がある。
- Codex home は通常 `~/.codex`。

## Baseline Checks

```bash
codex --version
brew --version
uv --version
git --version
```

## 実行中の方向修正（steering）

対応環境にON/OFF設定がある場合はONを推奨し、作業中の補足・軌道修正を標準運用にする。標準で利用できる環境では追加設定せず活用する。背景・変更理由・維持する条件を伝え、返答や成果物で反映を確認する。

2026-09-08のローカル確認では、`codex-cli 0.147.0`の`codex features list`は`steer removed true`を返した。この環境へ旧feature flagの有効化コマンドを案内しない。この表示だけから、desktopの設定状態やすべてのmodelでの挙動は断定しない。

[Codex CLI公式資料](https://learn.chatgpt.com/docs/codex/cli)は実行中の方向修正を案内している（確認日: 2026-09-08）。端末ごとにCLI/アプリのバージョンと操作方法を確認し、読み取り・下書きの作業で途中修正が反映されるか確かめる。未確認のWindows・hermes構成へ同じ設定手順を転用しない。

実行前承認は維持する。途中メッセージを送っただけで実行中操作が停止・取消されたとは扱わない。推奨理由・未検証事項は[マネジメントループ調査](../planning/research/2026-09-05-agent-management-loop.md#steeringの追加確認2026-09-08)、記入項目は[委任ワークシート](../knowledge/materials/agentic-organization-roadmap/worksheets/delegation.md)を参照。

更新トリガー: `steering-capability-change`（CLI/アプリ/modelの対応、設定名、反映タイミングの変更）。

## Node.js

context-mode には新しい Node.js が必要。Homebrew Node または Codex bundled Node など、動作確認済みの Node を使う。

```bash
brew install node
node --version
npm --version
```

Codex desktop の bundled Node が必要な場合の代表 path:

```text
/Applications/Codex.app/Contents/Resources/cua_node/bin/node
```

## Spec Kit

```bash
mkdir -p ~/codex-tools
git clone https://github.com/github/spec-kit.git ~/codex-tools/spec-kit
uv tool install specify-cli --from ~/codex-tools/spec-kit
specify --version
specify --help
```

Project initialization:

```bash
cd /path/to/project
specify init --here --integration codex --integration-options="--skills"
mkdir -p .codex
cp -R .agents/skills .codex/skills
```

Recommended flow:

```text
specify -> plan -> tasks -> implement
```

Optional quality gates:

- clarify
- analyze
- checklist
- converge

## context-mode

Install and enable context-mode as a Codex plugin. Then enable hooks in `~/.codex/config.toml`.

```toml
[features]
hooks = true
plugin_hooks = true
```

Verification:

```bash
codex plugin list | grep context-mode
```

If Codex cannot find a modern Node, configure the context-mode plugin MCP/hooks to use a known-good full Node path, such as Codex bundled Node.

After changing plugin/global config, restart Codex and verify in a new session:

```text
ctx stats
ctx doctor
```

## Serena

```bash
uv tool install -p 3.13 serena-agent
serena init
serena setup codex
serena --version
codex mcp list | grep serena
```

For Codex MCP sessions, launch Serena with project-from-current-directory semantics:

```toml
[mcp_servers.serena]
command = "/Users/<YOUR_USER>/.local/bin/serena"
args = ["start-mcp-server", "--context=codex", "--project-from-cwd"]
```

In each project, activate the current project and read Serena initial instructions before semantic code work.

## Global Routing Rules

Recommended routing:

1. Planning/product intent -> Spec Kit.
2. Codebase structure, symbols, references, refactors -> Serena.
3. Large output, repeated searches, long-running context, web/raw data -> context-mode.
4. Current library/framework/SDK/API/CLI/cloud documentation -> context7 (`context7.md`).
5. Small direct file edits and focused test/build commands -> Codex built-in tools.
6. Unattended, tracker-driven runs across many issues -> Symphony (see below). Interactive work stays in the Codex session.

## Symphony（任意 / 無人実行）

[Symphony](https://github.com/openai/symphony) は issue tracker をポーリングし、issue ごとの隔離ワークスペースでコーディングエージェントを継続実行する常駐サービスの仕様。OpenAI は仕様と Elixir 参照実装のみ公開し、製品としては保守しない。

導入する場合の要点:

- 実行方針は `WORKFLOW.md` としてリポジトリに置き、コードと同じくバージョン管理する。変更は PR とレビューを通す。
- リポジトリが run / test / verify できる状態になっていることが前提。整う前に無人実行させない。
- **人間承認領域は `Human Review` などの handoff state で止める。** Symphony に自動 merge させない（`docs/framework/quality-gates.md`）。
- 常駐サービスなので、実行ホスト・ネットワーク・認証情報の扱いは project 側の `docs/knowledge/engineering/` に記録する。tracker の認証情報を子プロセスへ二重に渡さない。

詳細な位置づけと AF 標準フローとの関係は `docs/framework/toolchain-flow.md` の「実行の自動化層」を参照。

## Final Verification

```bash
codex mcp list
codex plugin list | grep context-mode
codex plugin list | grep context7
specify --version
serena --version
node --version
npm --version
```

Expected important entries:

```text
context-mode ... enabled
context7 ... enabled
serena ... enabled
context-mode@context-mode installed, enabled
specify ...
Serena ...
node ...
```

Restart Codex after changing global config. In a new Codex session:

- Type `ctx stats` or `ctx doctor` to verify context-mode tools.
- Resolve a library ID and run one focused documentation query to verify context7; see `context7.md`.
- Ask Serena to activate the current project and read initial instructions.
- In Spec Kit projects, use the Spec Kit prompt/skill flow.

## Recommended Project Workflow

For a new or existing development project:

```bash
cd /path/to/project
specify init --here --integration codex --integration-options="--skills"
mkdir -p .codex
cp -R .agents/skills .codex/skills
```

Then in Codex:

1. Activate the project with Serena and read initial instructions.
2. Use Spec Kit to create or update the feature spec.
3. Use Serena for codebase exploration and symbol-level refactoring.
4. Use context-mode for large searches, logs, web/raw data, and continuity across compaction.
5. Keep Spec Kit tasks and GitHub Issues synchronized during implementation.

## Troubleshooting

### context-mode Cannot Start

- Confirm Codex hooks are enabled.
- Confirm the context-mode plugin is installed and enabled.
- Confirm the configured Node path exists and is compatible with the plugin runtime.
- Restart Codex after changing plugin config.

### Serena Does Not Appear in Codex

```bash
codex mcp list
serena --version
```

If Codex cannot find `serena`, use the full path in MCP config:

```toml
command = "/Users/<YOUR_USER>/.local/bin/serena"
```

Then restart Codex.

### Spec Kit Skills Do Not Appear

Confirm the project was initialized:

```bash
ls .specify
find .agents/skills -maxdepth 2 -name SKILL.md
```

If needed, copy generated skills into `.codex/skills`.

### Homebrew Node Link Fails

Inspect existing Node paths before overwriting anything. Prefer a known-good full Node path in plugin config when global linking is messy.
