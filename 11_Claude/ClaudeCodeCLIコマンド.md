# Claude Code CLI コマンド・操作完全リファレンス

## 目次
1. [スラッシュコマンド（/）](#1-スラッシュコマンド)
2. [特殊記号・接頭辞](#2-特殊記号接頭辞)
3. [キーボードショートカット](#3-キーボードショートカット)
4. [コマンドライン操作](#4-コマンドライン操作)
5. [カスタムコマンド作成](#5-カスタムコマンド作成)

---

## 1. スラッシュコマンド（/）

### 1.1 セッション管理コマンド

| コマンド | 説明 | 使用例 | カテゴリ |
|---------|------|--------|---------|
| `/help` | 全コマンド表示（組み込み+カスタム+MCP） | `/help` | ヘルプ |
| `/clear` | 現在のコンテキストをクリア | `/clear` | コンテキスト管理 |
| `/compact` | 会話履歴を要約して圧縮 | `/compact`<br>`/compact focus on auth errors` | コンテキスト管理 |
| `/rewind` | 会話を特定のメッセージまで巻き戻し | `/rewind` | 履歴操作 |
| `/continue` | 最新セッションを継続 | `/continue`<br>`/c` | セッション |
| `/context` | 現在のコンテキスト情報・使用状況を表示 | `/context` | 情報 |

### 1.2 設定・初期化コマンド

| コマンド | 説明 | 使用例 | カテゴリ |
|---------|------|--------|---------|
| `/config` | 設定UIを開く | `/config` | 設定 |
| `/init` | プロジェクトにCLAUDE.mdを作成 | `/init` | 初期化 |
| `/terminal-setup` | ターミナル設定を最適化 | `/terminal-setup` | 初期化 |
| `/status` | 現在のモデルとシステム状態を表示 | `/status` | 情報 |
| `/model` | 使用するClaudeモデルを切り替え | `/model opus`<br>`/model sonnet`<br>`/model haiku` | 設定 |

### 1.3 ファイル・プロジェクト管理コマンド

| コマンド | 説明 | 使用例 | カテゴリ |
|---------|------|--------|---------|
| `/add-dir` | セッションに追加ディレクトリを追加 | `/add-dir /path/to/project` | ファイル |
| `/explain-tree` | ディレクトリ構造を説明 | `/explain-tree` | 情報 |
| `/repo-summary` | リポジトリの概要を生成 | `/repo-summary` | 情報 |

### 1.4 コード品質・レビューコマンド

| コマンド | 説明 | 使用例 | カテゴリ |
|---------|------|--------|---------|
| `/review` | コードレビューを実行 | `/review` | コード品質 |
| `/todos` | TODO項目をリスト表示 | `/todos` | タスク管理 |
| `/doctor` | Claude Codeインストールの健全性チェック | `/doctor` | デバッグ |

### 1.5 統合・拡張コマンド

| コマンド | 説明 | 使用例 | カテゴリ |
|---------|------|--------|---------|
| `/plugin` | プラグインマーケットプレイス管理UI | `/plugin` | プラグイン |
| `/mcp` | MCPサーバー管理 | `/mcp add github`<br>`/mcp list` | MCP |
| `/hooks` | フック設定UI | `/hooks` | 設定 |
| `/install-github-app` | GitHub App インストール（PR自動レビュー） | `/install-github-app` | 統合 |

### 1.6 パーミッション・ツール管理コマンド

| コマンド | 説明 | 使用例 | カテゴリ |
|---------|------|--------|---------|
| `/allowed-tools` | 許可ツールの表示・設定 | `/allowed-tools` | パーミッション |

### 1.7 その他のコマンド

| コマンド | 説明 | 使用例 | カテゴリ |
|---------|------|--------|---------|
| `/usage` | トークン使用量・コスト見積もり | `/usage` | 情報 |
| `/export` | 会話をファイル/クリップボードにエクスポート | `/export report.md` | 出力 |
| `/bashes` | バックグラウンドBashタスク管理 | `/bashes` | タスク管理 |

### 1.8 カスタムコマンド

| コマンドパターン | 説明 | 定義場所 | 使用例 |
|--------------|------|---------|--------|
| `/project:コマンド名` | プロジェクト固有コマンド | `.claude/commands/コマンド名.md` | `/project:test`<br>`/project:deploy` |
| `/workflows:ワークフロー名` | 複合ワークフロー | `.claude/commands/workflows/` | `/workflows:feature-development` |
| `/tools:ツール名` | 単一目的ツール | `.claude/commands/tools/` | `/tools:security-scan` |
| `/コマンド名` | ユーザーグローバルコマンド | `~/.claude/commands/コマンド名.md` | `/test-all`<br>`/deploy-prod` |

### 1.9 プラグイン・スキル由来コマンド

| コマンドパターン | 説明 | 使用例 |
|--------------|------|--------|
| `/プラグイン名:コマンド` | プラグインコマンド | `/gemini:visual`<br>`/codex:review`<br>`/astral:uv` |
| `/スキル名` | スキル実行 | `/explain-code`<br>`/pr-summary` |

---

## 2. 特殊記号・接頭辞

### 2.1 @ - ファイル・リソース参照

| 用途 | 構文 | 説明 | 使用例 |
|------|------|------|--------|
| **単一ファイル** | `@ファイルパス` | ファイル内容をコンテキストに追加 | `@src/auth.ts`<br>`@package.json`<br>`@README.md` |
| **複数ファイル** | `@file1 @file2 @file3` | 複数ファイルを同時参照 | `@src/auth.ts @src/db.ts @types.ts` |
| **ディレクトリ** | `@ディレクトリ/` | ディレクトリ全体を参照 | `@src/`<br>`@tests/`<br>`@components/` |
| **絶対パス** | `@/path/to/file` | ルートからの絶対パス | `@/src/components/Header.tsx` |
| **ホームディレクトリ** | `@~/path` | ユーザーホームディレクトリから | `@~/.claude/commands/test.md` |
| **MCPリソース** | `@server:protocol://path` | MCP経由の外部リソース | `@github:issue/123`<br>`@datadog:dashboard/abc` |
| **サブエージェント** | `@エージェント名` | カスタムサブエージェント起動 | `@code-reviewer`<br>`@db-expert` |

**特徴:**
- Tab補完: `@` 入力後にTabキーで補完
- Shift+ドラッグ: ファイルをShift押しながらドラッグで `@` 参照挿入
- 画像対応: PNG、JPEGなどの画像ファイルも参照可能

### 2.2 ! - Bashコマンド直接実行

| 用途 | 構文 | 説明 | 使用例 |
|------|------|------|--------|
| **会話中の直接実行** | `!コマンド` | Bashコマンドを即座に実行 | `!npm test`<br>`!git status`<br>`!ls -la` |
| **プリプロセッシング** | `!\`コマンド\`` | カスタムコマンド内で実行結果を挿入 | `!\`gh pr diff\``<br>`!\`git log -1\``<br>`!\`cat config.json\`` |

**動作:**
- 会話モードをバイパスしてトークンを節約
- シェルが直接実行（Claudeは結果のみを受け取る）
- カスタムコマンド内では実行前に処理され、出力がマークダウンに挿入される

### 2.3 # - インラインメモリ（クイックコンテキスト）

| 用途 | 構文 | 説明 | 使用例 |
|------|------|------|--------|
| **セッション内ルール** | `# ルール/メモ` | Claudeに覚えておいてほしいことを記録 | `# Use TypeScript strict mode`<br>`# All functions need tests`<br>`# Follow Airbnb style guide` |

**使い方:**
```bash
# Use JWT tokens for authentication
# Follow React hooks conventions  
# Write tests for all API endpoints

# 上記のルールを適用して認証ミドルウェアを作成
Create an auth middleware for Express
```

### 2.4 | - パイプライン操作

| 構文 | 説明 | 使用例 |
|------|------|--------|
| `コマンド \| claude -p "質問"` | コマンド出力をClaudeに渡す | `cat logs.txt \| claude -p "エラー分析"`<br>`git diff \| claude -p "変更レビュー"`<br>`cat data.csv \| claude -p "データ分析"` |
| `claude -p "質問" > file` | Claude出力をファイルに保存 | `claude -p "レポート生成" > report.md` |
| `command \| claude \| command` | パイプラインチェーン | `cat input.txt \| claude -p "変換" \| grep "重要"` |

### 2.5 > - 引用・継続

| 構文 | 説明 | 使用例 |
|------|------|--------|
| `> テキスト` | 前のメッセージを引用 | `> このエラーについて`<br>`> 認証機能の実装` |

---

## 3. キーボードショートカット

### 3.1 基本操作

| ショートカット | 機能 | 説明 |
|-------------|------|------|
| `Ctrl+C` | キャンセル | Claudeの実行を中断 |
| `Ctrl+R` | 履歴検索 | コマンド履歴を検索 |
| `Esc Esc` | 巻き戻しメニュー | 会話を前の状態に戻す |

### 3.2 表示切替

| ショートカット | 機能 | 説明 |
|-------------|------|------|
| `Tab` | 思考モード切替 | Claudeの思考プロセス表示のオン/オフ |
| `Shift+Tab` | モード切替 | 異なる動作モード間を切り替え |

### 3.3 入力操作

| ショートカット | 機能 | 説明 | 備考 |
|-------------|------|------|------|
| `Shift+Enter` | 改行 | プロンプト内で改行挿入 | `/terminal-setup` で設定が必要 |
| `Ctrl+V` | 画像貼り付け | クリップボードから画像を貼り付け | macOSでも`Cmd+V`ではなく`Ctrl+V` |

### 3.4 ファイル操作

| 操作 | 方法 | 結果 |
|------|------|------|
| **ファイルドラッグ** | ファイルをドラッグ | 新しいタブで開く |
| **Shift+ドラッグ** | Shift押しながらドラッグ | `@ファイルパス` として参照挿入 |
| **スクリーンショット** | macOS: `Cmd+Ctrl+Shift+4` → `Ctrl+V` | 画面キャプチャをクリップボード経由で挿入 |

---

## 4. コマンドライン操作

### 4.1 基本コマンド

| コマンド | 説明 | 使用例 |
|---------|------|--------|
| `claude` | インタラクティブモード起動 | `claude` |
| `claude "質問"` | 初期プロンプト付き起動 | `claude "このプロジェクトを説明して"` |
| `claude -c` | 最新セッションを継続 | `claude -c`<br>`claude --continue` |
| `claude -p "質問"` | プリントモード（非インタラクティブ） | `claude -p "コードレビュー"` |
| `claude --resume <id>` | 特定セッションを再開 | `claude --resume abc123` |

### 4.2 主要フラグ

| フラグ | 説明 | 使用例 |
|-------|------|--------|
| `-p, --print` | プリントモード（ヘッドレス） | `claude -p "質問"` |
| `-c, --continue` | 最新セッションを継続 | `claude -c` |
| `--resume <id>` | 特定セッションを再開 | `claude --resume abc123` |
| `--output-format <fmt>` | 出力フォーマット指定 | `--output-format json`<br>`--output-format stream-json` |
| `--allowedTools <tools>` | 使用可能ツールを制限 | `--allowedTools "Read,Write,Bash"` |
| `--max-turns <n>` | 最大ターン数制限 | `--max-turns 5` |
| `--add-dir <path>` | 追加ディレクトリを指定 | `--add-dir /path/to/dir` |
| `--append-system-prompt` | システムプロンプトを追加 | `--append-system-prompt "You are SRE expert"` |
| `--system-prompt` | システムプロンプトを置き換え | `--system-prompt "Custom prompt"` |
| `--agents <json>` | カスタムエージェントを定義 | `--agents '{"reviewer": {...}}'` |
| `--dangerously-skip-permissions` | パーミッション確認をスキップ | `--dangerously-skip-permissions` |
| `--debug` | デバッグモード | `--debug` |
| `--verbose` | 詳細ログ出力 | `--verbose` |

### 4.3 自動化・CI/CD向けコマンド

| コマンド例 | 説明 | 用途 |
|----------|------|------|
| `claude -p "query" --output-format json` | JSON形式で出力 | スクリプト処理 |
| `cat logs.txt \| claude -p "分析"` | パイプ入力 | ログ分析 |
| `claude -p "レビュー" --allowedTools Read,Grep` | ツール制限 | CI/CDパイプライン |
| `claude -c -p "続行" > output.txt` | 非対話的継続 | 自動化スクリプト |

---

## 5. カスタムコマンド作成

### 5.1 カスタムコマンドの配置

| 種類 | パス | スコープ | 使用例 |
|------|------|---------|--------|
| **プロジェクトコマンド** | `.claude/commands/名前.md` | 現在のプロジェクトのみ | `/project:名前` |
| **グローバルコマンド** | `~/.claude/commands/名前.md` | すべてのプロジェクト | `/名前` |

### 5.2 コマンド定義のYAMLフロントマター

| フィールド | 型 | 必須 | 説明 | 例 |
|-----------|-----|------|------|-----|
| `name` | string | - | コマンド名（ファイル名から自動） | `test-all` |
| `description` | string | ✓ | コマンドの説明 | `Run all unit tests` |
| `argument-hint` | string | - | 引数のヒント表示 | `[issue-number]`<br>`[branch-name]` |
| `allowed-tools` | array | - | 使用可能ツールの制限 | `Bash(git:*)`, `Read`, `Write` |
| `context` | string | - | コンテキスト設定 | `fork` |
| `agent` | string | - | 使用するエージェント | `Explore` |

### 5.3 コマンドで使用可能な変数

| 変数 | 説明 | 使用例 |
|------|------|--------|
| `$ARGUMENTS` | すべての引数 | `Process: $ARGUMENTS` |
| `$1`, `$2`, `$3`, ... | 個別の引数 | `File: $1, Action: $2` |
| `$CLAUDE_PROJECT_DIR` | プロジェクトディレクトリパス | `cd $CLAUDE_PROJECT_DIR` |
| `$CLAUDE_ENV_FILE` | 環境ファイルパス | `source $CLAUDE_ENV_FILE` |
| `$file` | フックで処理されるファイルパス | `prettier --write $file` |

### 5.4 コマンド作成例

#### 例1: シンプルなテストコマンド
`.claude/commands/test.md`:
```markdown
---
description: Run all unit tests and report results
---

Run all the unit tests and report the results.
```
**使用**: `/project:test`

#### 例2: 引数付きコマンド
`.claude/commands/fix-issue.md`:
```markdown
---
description: Fix a GitHub issue
argument-hint: [issue-number]
allowed-tools: Bash(gh:*)
---

Please analyze and fix GitHub issue: $ARGUMENTS

Steps:
1. Use `gh issue view $ARGUMENTS` to get details
2. Search codebase for relevant files
3. Implement the fix
4. Create tests
5. Create a PR
```
**使用**: `/project:fix-issue 123`

#### 例3: プリプロセッシング付きコマンド
`.claude/commands/pr-summary.md`:
```markdown
---
description: Summarize PR changes
allowed-tools: Bash(gh:*)
---

## Pull Request Context
- PR diff: !`gh pr diff`
- PR comments: !`gh pr view --comments`
- Changed files: !`gh pr diff --name-only`

## Task
Summarize the above pull request changes.
```
**使用**: `/project:pr-summary`

---

## 6. 実践的な組み合わせパターン

### パターン1: 完全なコンテキスト設定
```bash
# Use TypeScript strict mode
# All functions must have tests
# Follow Airbnb style guide

@package.json @tsconfig.json @src/auth.ts

!git status

この設定で新しい認証モジュールを作成
```

### パターン2: デバッグフロー
```bash
# 1. ログ分析
cat error.log | claude -p "エラー原因を特定"

# 2. ファイル参照して修正
@src/auth.ts @src/db.ts 
上記のエラーを修正して

# 3. テスト
!npm test -- auth

# 4. レビュー
/review
```

### パターン3: PR作成フロー
```bash
# 1. 変更確認
!git diff

# 2. コミットメッセージ生成
/project:commit

# 3. レビュー後PR作成
@code-reviewer レビューしてからPRを作成
```

### パターン4: CI/CD自動化
```bash
# セキュリティスキャン + JSONレポート
claude -p "セキュリティスキャン実行" \
  --allowedTools "Read,Grep,Bash" \
  --output-format json \
  --max-turns 3 > security-report.json
```

---

## 7. クイックリファレンス

### よく使うコマンド TOP 10

| ランク | コマンド | 用途 |
|-------|---------|------|
| 1 | `@ファイルパス` | ファイル参照 |
| 2 | `/clear` | コンテキストクリア |
| 3 | `!コマンド` | Bash実行 |
| 4 | `/help` | コマンド一覧 |
| 5 | `/review` | コードレビュー |
| 6 | `/context` | コンテキスト確認 |
| 7 | `/compact` | 履歴圧縮 |
| 8 | `claude -c` | セッション継続 |
| 9 | `/init` | CLAUDE.md作成 |
| 10 | `/project:コマンド名` | カスタムコマンド |

### トラブルシューティング

| 問題 | 解決方法 |
|------|---------|
| コンテキスト制限に到達 | `/compact` で圧縮、または `/clear` で新規開始 |
| 不要な変更がされた | `/rewind` で巻き戻し |
| コマンドが見つからない | `/help` で確認、または `claude --debug` でデバッグ |
| ファイルが参照できない | `@` の後にTabキーで補完、またはパスを確認 |
| パーミッションエラー | `/allowed-tools` で確認、または `--dangerously-skip-permissions` |

---

## 付録: 設定ファイル関連パス

| ファイル | パス | 用途 |
|---------|------|------|
| **ユーザー設定** | `~/.claude/settings.json` | グローバル設定 |
| **プロジェクト設定** | `.claude/settings.json` | プロジェクト共有設定 |
| **ローカル設定** | `.claude/settings.local.json` | 個人用プロジェクト設定 |
| **グローバルメモリ** | `~/.claude/CLAUDE.md` | 全プロジェクト共通コンテキスト |
| **プロジェクトメモリ** | `./CLAUDE.md` | プロジェクト固有コンテキスト |
| **カスタムコマンド** | `~/.claude/commands/` | グローバルコマンド |
| **プロジェクトコマンド** | `.claude/commands/` | プロジェクトコマンド |
| **サブエージェント** | `~/.claude/agents/` | グローバルエージェント |
| **プロジェクトエージェント** | `.claude/agents/` | プロジェクトエージェント |
| **スキル** | `~/.claude/skills/` | ユーザースキル |

---
