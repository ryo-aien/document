# Claude Code — Settings（設定）仕様整理

## 1. 設定スコープと優先順位

Claude Code の設定は複数の **スコープ（適用範囲）** を持ち、
**より優先度の高い設定が下位の設定を上書き**する。

## 設定ファイルの配置場所と優先順位

| 優先度 | ファイルパス | 用途 | 備考 |
|-------|------------|------|------|
| 1（最高） | macOS: `/Library/Application Support/ClaudeCode/managed-settings.json`<br>Linux: `/etc/claude-code/managed-settings.json`<br>Windows: `C:\ProgramData\ClaudeCode\managed-settings.json` | 企業全体の管理設定 | ユーザー・プロジェクト設定で上書き不可 |
| 2 | `.claude/settings.json` | プロジェクト共有設定 | Git にコミット推奨 |
| 3 | `.claude/settings.local.json` | ローカルプロジェクト設定 | Git で無視される（個人用） |
| 4（最低） | `~/.claude/settings.json` | ユーザーグローバル設定 | 全プロジェクトに適用 |

**運用指針**

* チームで揃える設定：`Project`
* 個人差が出る設定：`Local`
* セキュリティ方針：`Managed` または `Project`

---

## 2. settings.json パラメーター一覧
# Claude Code settings.json 仕様書

## 1. 認証・API設定

| 設定項目 | 型 | デフォルト値 | 説明 | 使用例 |
|---------|-----|------------|------|--------|
| `apiKeyHelper` | string | - | API キーを出力するスクリプトのパス | `"/path/to/api-key-script"` |
| `awsCredentialExport` | string | - | AWS 認証情報をエクスポートするスクリプトのパス | `"/path/to/aws-cred-script"` |
| `awsAuthRefresh` | string | - | AWS 認証をリフレッシュするスクリプトのパス | `"/path/to/aws-refresh"` |
| `forceLoginMethod` | string | - | 強制的に使用するログイン方法 | `"claudeai"` または `"console"` |
| `forceLoginOrgUUID` | string | - | OAuth ログインに使用する組織 UUID | `"org-uuid-here"` |

## 2. モデル設定

| 設定項目 | 型 | デフォルト値 | 説明 | 使用例 |
|---------|-----|------------|------|--------|
| `model` | string | `"claude-sonnet-4-20250514"` | 使用する Claude モデル | `"claude-opus-4-5-20251101"` |
| `maxTokens` | integer | `4096` | 最大出力トークン数 | `8192` |

## 3. 環境変数 (`env` オブジェクト内)

### 3.1 API 関連

| 環境変数名 | 型 | 説明 | 使用例 |
|-----------|-----|------|--------|
| `ANTHROPIC_API_KEY` | string | Anthropic API キー | `"sk-ant-..."` |
| `ANTHROPIC_AUTH_TOKEN` | string | 認証トークン | `"your-token"` |
| `ANTHROPIC_BASE_URL` | string | API のベース URL | `"https://api.anthropic.com"` |
| `API_TIMEOUT_MS` | string | API タイムアウト（ミリ秒） | `"300000"` |

### 3.2 モデル指定

| 環境変数名 | 型 | 説明 | 使用例 |
|-----------|-----|------|--------|
| `ANTHROPIC_MODEL` | string | 使用するモデル | `"claude-sonnet-4-20250514"` |
| `ANTHROPIC_DEFAULT_SONNET_MODEL` | string | デフォルトの Sonnet モデル | `"claude-sonnet-4-5-20250929"` |
| `ANTHROPIC_DEFAULT_OPUS_MODEL` | string | デフォルトの Opus モデル | `"claude-opus-4-5-20251101"` |
| `ANTHROPIC_DEFAULT_HAIKU_MODEL` | string | デフォルトの Haiku モデル | `"claude-haiku-4-5-20251001"` |

### 3.3 トークン・コンテキスト制限

| 環境変数名 | 型 | 説明 | 使用例 |
|-----------|-----|------|--------|
| `CLAUDE_CODE_MAX_OUTPUT_TOKENS` | string | 最大出力トークン数 | `"16384"` |
| `CLAUDE_CODE_MAX_CONTEXT_WINDOW` | string | 最大コンテキストウィンドウサイズ | `"200000"` |

### 3.4 Bash 設定

| 環境変数名 | 型 | 説明 | 使用例 |
|-----------|-----|------|--------|
| `BASH_DEFAULT_TIMEOUT_MS` | string | Bash コマンドのデフォルトタイムアウト | `"30000"` |
| `CLAUDE_BASH_MAINTAIN_PROJECT_WORKING_DIR` | string | コマンド実行後にプロジェクトディレクトリに戻る | `"1"` |

### 3.5 トラフィック・テレメトリ制御

| 環境変数名 | 型 | 説明 | 使用例 |
|-----------|-----|------|--------|
| `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` | string | 不要なネットワークトラフィックを無効化 | `"1"` |
| `DISABLE_NON_ESSENTIAL_MODEL_CALLS` | string | 不要なモデル呼び出しを無効化 | `"1"` |
| `DISABLE_TELEMETRY` | string | テレメトリを無効化 | `"1"` |

### 3.6 その他の環境変数

| 環境変数名 | 型 | 説明 | 使用例 |
|-----------|-----|------|--------|
| `CLAUDE_CODE_SKIP_CONTEXT_OPTIMIZATION` | string | コンテキスト最適化をスキップ | `"0"` |
| `CLAUDE_CODE_ENABLE_TOOL_SEARCH` | string | ツール検索を有効化 | `"1"` |

## 4. パーミッション設定 (`permissions` オブジェクト内)

### 4.1 基本パーミッション配列

| 設定項目 | 型 | 説明 | 使用例 |
|---------|-----|------|--------|
| `allow` | array[string] | 自動的に許可するツール | `["Read", "Write", "Bash(git *)"]` |
| `ask` | array[string] | 確認が必要なツール | `["Bash(rm *)", "WebSearch"]` |
| `deny` | array[string] | 完全に拒否するツール | `["Read(./.env)", "Bash(sudo:*)"]` |

### 4.2 パーミッション動作設定

| 設定項目 | 型 | デフォルト値 | 説明 | 使用可能な値 |
|---------|-----|------------|------|-------------|
| `defaultMode` | string | `"default"` | デフォルトの権限モード | `"default"`, `"acceptEdits"`, `"plan"`, `"bypassPermissions"` |
| `disableBypassPermissionsMode` | string | - | バイパスモードを無効化 | `"disable"` |
| `additionalDirectories` | array[string] | `[]` | アクセスを許可する追加ディレクトリ | `["/path/to/dir"]` |

### 4.3 利用可能なツール

| ツール名 | パターン対応 | 説明 | 使用例 |
|---------|------------|------|--------|
| `Read` | ✓ | ファイル読み取り | `Read(.env)`, `Read(**/.env)` |
| `Write` | ✓ | ファイル書き込み | `Write(*.py)`, `Write(./prod/*)` |
| `Bash` | ✓ | シェルコマンド実行 | `Bash(git *)`, `Bash(sudo:*)` |
| `WebFetch` | ✓ | Web コンテンツ取得 | `WebFetch(domain:example.com)` |
| `WebSearch` | ✗ | Web 検索 | `WebSearch` |
| `Grep` | ✓ | ファイル検索 | `Grep` |
| `Glob` | ✓ | ファイルパターンマッチング | `Glob` |

## 5. フック設定 (`hooks` オブジェクト内)

| フックタイプ | 実行タイミング | 説明 | マッチャー例 |
|------------|-------------|------|-----------|
| `SessionStart` | セッション開始時 | セッション開始時に実行 | `"startup"` |
| `PreToolUse` | ツール実行前 | ツール実行前に実行 | `"Write(*.py)"`, `"Read"` |
| `PostToolUse` | ツール実行後 | ツール実行後に実行 | `"Write(*.ts)"`, `"Edit\|Write"` |

### フック設定の構造

| 設定項目 | 型 | 必須 | 説明 | 使用例 |
|---------|-----|------|------|--------|
| `matcher` | string | ✓ | マッチングパターン（正規表現可） | `"Write(*.py)"`, `"Edit\|Write"` |
| `hooks` | array[object] | ✓ | 実行するフックの配列 | 下記参照 |
| `hooks[].type` | string | ✓ | フックのタイプ | `"command"` |
| `hooks[].command` | string | ✓ | 実行するコマンド | `"black $file"` |

### フックで使用可能な変数

| 変数名 | 説明 | 使用例 |
|-------|------|--------|
| `$file` | 対象ファイルのパス | `"prettier --write $file"` |
| `$CLAUDE_PROJECT_DIR` | プロジェクトディレクトリのパス | `"cd $CLAUDE_PROJECT_DIR"` |
| `$CLAUDE_ENV_FILE` | 環境ファイルのパス | `"source $CLAUDE_ENV_FILE"` |

## 6. UI/UX 設定

| 設定項目 | 型 | デフォルト値 | 説明 | 使用可能な値 |
|---------|-----|------------|------|-------------|
| `spinnerTipsEnabled` | boolean | `true` | ローディング中のヒント表示 | `true`, `false` |
| `outputStyle` | string | `"default"` | 出力スタイルの制御 | `"default"` など |

## 7. アトリビューション設定 (`attribution` オブジェクト内)

| 設定項目 | 型 | デフォルト値 | 説明 |
|---------|-----|------------|------|
| `commits` | boolean | `true` | Git コミットに Co-Authored-By を含める |
| `pullRequests` | boolean | `true` | プルリクエストに Claude Code フッターを含める |

## 8. クリーンアップ設定

| 設定項目 | 型 | デフォルト値 | 説明 |
|---------|-----|------------|------|
| `cleanupPeriodDays` | integer | `30` | チャット履歴の保持期間（日数）。0 で無効化 |

## 9. ツール制御設定

| 設定項目 | 型 | デフォルト値 | 説明 | 使用例 |
|---------|-----|------------|------|--------|
| `disallowedTools` | array[string] | `[]` | 使用を禁止するツールのリスト | `["SomeTool"]` |
| `fileCompletionCommand` | string | - | ファイル補完用のカスタムコマンド | `"/path/to/completion"` |

## 10. Web 関連設定

| 設定項目 | 型 | デフォルト値 | 説明 |
|---------|-----|------------|------|
| `skipWebFetchPreflight` | boolean | `false` | WebFetch のプリフライトチェックをスキップ |

## 11. OpenTelemetry 設定

| 設定項目 | 型 | デフォルト値 | 説明 |
|---------|-----|------------|------|
| `otelHeadersHelper` | string | - | OpenTelemetry ヘッダーを出力するスクリプトのパス |

## 12. マーケットプレイス設定

| 設定項目 | 型 | 説明 | 使用例 |
|---------|-----|------|--------|
| `extraKnownMarketplaces` | object | 追加のマーケットプレイス定義 | 下記参照 |
| `requiredMarketplaces` | array[string] | 必須のマーケットプレイス | `["my-marketplace"]` |
| `skippedMarketplaces` | array[string] | スキップするマーケットプレイス | `["marketplace-name"]` |
| `skippedPlugins` | array[string] | スキップするプラグイン | `["plugin@marketplace"]` |

### マーケットプレイスソースタイプ

| ソースタイプ | 必須フィールド | オプションフィールド | 使用例 |
|------------|--------------|-------------------|--------|
| `github` | `repo` | `ref`, `path` | `{"source": "github", "repo": "owner/repo"}` |
| `url` | `url` | - | `{"source": "url", "url": "https://..."}` |
| `git` | `url` (末尾.git) | `path` | `{"source": "git", "url": "https://....git"}` |

## 13. 管理設定専用（managed-settings.json のみ）

| 設定項目 | 型 | 説明 | 使用例 |
|---------|-----|------|--------|
| `allowedMarketplaces` | array[object] | 許可するマーケットプレイスのホワイトリスト | `[{"source": "github", "repo": "approved/plugins"}]` |
| `allowManagedHooksOnly` | boolean | 管理されたフックのみ実行を許可 | `true` |



## 4. 運用上の注意点

* **最優先で設定すべきは `permissions`**
* `.env`、秘密鍵、認証情報は必ず `permissions.deny` に含める
* チーム共通設定は `.claude/settings.json` に集約する
* 個人ごとの差分は `.claude/settings.local.json` で上書きする
* 設定されていない項目は **すべてデフォルト動作**
* 最初は最小構成で運用し、必要に応じて項目を追加する

