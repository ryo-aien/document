# Claude Code — Settings（設定）仕様整理

## 1. 設定スコープと優先順位

Claude Code の設定は複数の **スコープ（適用範囲）** を持ち、
**より優先度の高い設定が下位の設定を上書き**する。

### 設定スコープ一覧

| スコープ    | 設定ファイル                        | 用途            | Git 管理 |
| ------- | ----------------------------- | ------------- | ------ |
| Managed | `managed-settings.json`       | 組織・管理者による強制設定 | 任意     |
| User    | `~/.claude/settings.json`     | ユーザー共通設定      | ×      |
| Project | `.claude/settings.json`       | プロジェクト／チーム共有  | ○      |
| Local   | `.claude/settings.local.json` | 個人用の上書き設定     | ×      |

### 優先順位（高 → 低）

```
Managed > CLI 引数 > Local > Project > User
```

**運用指針**

* チームで揃える設定：`Project`
* 個人差が出る設定：`Local`
* セキュリティ方針：`Managed` または `Project`

---

## 2. settings.json パラメーター一覧

### パラメーター一覧

| パラメーター                      | 型        | 何が起きるか（平易な説明）                  | 実際の設定例                                              |
| --------------------------- | -------- | ------------------------------ | --------------------------------------------------- |
| **permissions**             | object   | Claude が実行してよい／確認する／禁止する操作を決める | `"permissions": { ... }`                            |
| permissions.allow           | string[] | 確認なしで自動実行される                   | `"allow": ["Bash(npm test:*)"]`                     |
| permissions.ask             | string[] | 実行前に毎回確認される                    | `"ask": ["Bash(git push:*)"]`                       |
| permissions.deny            | string[] | 絶対に実行されない                      | `"deny": ["Read(.env)"]`                            |
| permissions.defaultMode     | string   | どのルールにも当たらない操作の基本動作            | `"defaultMode": "ask"`                              |
| **env**                     | object   | コマンド実行時に自動で設定される環境変数           | `"env": { "CLAUDE_CODE_ENABLE_TELEMETRY": "0" }`    |
| **model**                   | string   | 使用する Claude モデルを固定する           | `"model": "claude-sonnet-4-5-20250929"`             |
| **outputStyle**             | string   | Claude の回答を短く／詳しくする            | `"outputStyle": "concise"`                          |
| **hooks**                   | object   | 操作の前後に自動でコマンドを実行する             | `"hooks": { ... }`                                  |
| hooks.PreToolUse            | object   | コマンド実行直前に必ず走る                  | `"PreToolUse": { "Bash": "echo start" }`            |
| hooks.Notification          | array    | 特定イベント時に通知や処理を行う               | `"Notification": [...]`                             |
| **attribution**             | object   | Git コミットや PR に文章を追加する          | `"attribution": { ... }`                            |
| attribution.commit          | string   | コミットメッセージ末尾に追記                 | `"commit": "Generated with Claude Code"`            |
| attribution.pr              | string   | PR 説明文に追記                      | `"pr": ""`                                          |
| **cleanupPeriodDays**       | number   | 古いデータを自動削除する日数                 | `"cleanupPeriodDays": 20`                           |
| **respectGitignore**        | boolean  | `.gitignore` のファイルを見に行かない      | `"respectGitignore": true`                          |
| **statusLine**              | object   | 画面下に使用量や状態を表示する                | `"statusLine": { ... }`                             |
| statusLine.type             | string   | 表示方式                           | `"type": "command"`                                 |
| statusLine.command          | string   | 表示内容を生成するコマンド                  | `"command": "npx ccusage statusline"`               |
| **fileSuggestion**          | object   | `@` 入力時のファイル候補を生成              | `"fileSuggestion": { ... }`                         |
| fileSuggestion.type         | string   | 候補生成方式                         | `"type": "command"`                                 |
| fileSuggestion.command      | string   | 候補生成スクリプト                      | `"command": "~/.claude/file-suggestion.sh"`         |
| **apiKeyHelper**            | string   | APIキー取得用コマンド                   | `"apiKeyHelper": "~/.claude/get-key.sh"`            |
| **enabledPlugins**          | string[] | 使用するプラグインを限定                   | `"enabledPlugins": ["@org/plugin"]`                 |
| **extraKnownMarketplaces**  | string[] | 信頼する追加配布元                      | `"extraKnownMarketplaces": ["https://example.com"]` |
| **strictKnownMarketplaces** | boolean  | 指定配布元以外を禁止                     | `"strictKnownMarketplaces": true`                   |
| **allowedMcpServers**       | string[] | 接続を許可する外部サーバ                   | `"allowedMcpServers": ["local-mcp"]`                |
| **disabledMcpServers**      | string[] | 接続を禁止する外部サーバ                   | `"disabledMcpServers": ["remote-mcp"]`              |
| **additionalDirectories**   | string[] | プロジェクト外で読ませてよいディレクトリ           | `"additionalDirectories": ["../shared"]`            |

---

## 3. 運用上の注意点

* **最優先で設定すべきは `permissions`**
* `.env`、秘密鍵、認証情報は必ず `permissions.deny` に含める
* チーム共通設定は `.claude/settings.json` に集約する
* 個人ごとの差分は `.claude/settings.local.json` で上書きする
* 設定されていない項目は **すべてデフォルト動作**
* 最初は最小構成で運用し、必要に応じて項目を追加する

