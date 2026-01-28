## Claude Code CLIの標準ツール

| カテゴリ | ツール名 | 説明 |
|---------|---------|------|
| **ファイル操作** | Read | ファイルやディレクトリの内容を読み取る |
| | Write | 新しいファイルを作成する |
| | Edit | 既存ファイルを編集する（文字列置換） |
| | MultiEdit | 複数ファイルを一括編集する |
| | LS | ディレクトリの内容をリスト表示 |
| **検索・探索** | Grep | ripgrepベースの高速テキスト検索（正規表現対応） |
| | Glob | ファイル名パターンマッチング（例：`**/*.js`） |
| | Task/Agent | 複雑な探索タスク用のサブエージェントを起動 |
| **コマンド実行** | Bash | シェルコマンドを実行（git、npm、dockerなど） |
| | BashOutput | バックグラウンド実行中のコマンドの出力を取得 |
| | KillShell | シェルプロセスを終了 |
| **タスク管理** | TodoWrite | タスクリストを作成・更新 |
| | TodoRead | タスクリストを読み取る |
| **Web関連** | WebFetch | ウェブページの内容を取得 |
| | WebSearch | ウェブ検索を実行 |
| **ノートブック** | NotebookRead | Jupyterノートブックを読み取る |
| | NotebookEdit | Jupyterノートブックを編集する |
| **MCP統合** | ListMcpResources | MCPサーバーのリソースをリスト表示 |
| | ReadMcpResource | MCPリソースを読み取る |
| **対話** | AskUserQuestion | ユーザーに質問する |
| **モード制御** | ExitPlanMode | プランモードから抜ける |


## タスク実行フロー

### **基本的な実行フロー**

```
1. プラン作成（Plan Mode）
   ↓
2. 探索・調査（Explore Agent）
   ↓
3. タスク実行（Task Agent）
   ↓
4. 検証・テスト
   ↓
5. リント・型チェック
   ↓
6. 完了
```

### **詳細フロー図**

| フェーズ | 使用ツール/エージェント | 実行内容 |
|---------|---------------------|---------|
| **1. プラン作成** | Plan Agent<br>TodoWrite | ・タスクを分析してプランを立てる<br>・ユーザーに確認を求める<br>・TodoWriteでタスクリストを作成 |
| **2. コードベース理解** | Explore Agent<br>Grep, Glob, Read | ・関連ファイルを検索<br>・既存コードの構造を理解<br>・依存関係を把握 |
| **3. 実装** | Task Agent<br>Write, Edit, MultiEdit | ・コードを実装<br>・複数ファイルを編集<br>・TodoWriteでステータス更新 |
| **4. テスト実行** | Bash | ・テストフレームワークを検出<br>・テストを実行<br>・失敗したら修正して再実行 |
| **5. 品質チェック** | Bash | ・`npm run lint`<br>・`npm run typecheck`<br>・`ruff`など |
| **6. Git操作** | Bash | ・`git add`<br>・`git commit`<br>・`git push`（ユーザーが明示的に要求した場合のみ） |

### **具体例：機能追加タスク**

```bash
# ユーザー入力
claude "ユーザー認証機能を追加して"

# Claude Codeの実行フロー：

1. [Plan Mode]
   - TodoWrite: タスクリストを作成
     □ 認証ロジックを検索
     □ 認証システムを実装
     □ テストを書く
     □ リントとテストを実行

2. [Explore]
   - Grep: "authentication" "login" "JWT"
   - Read: 関連ファイルを読み込み
   - 結果: 既存の認証パターンを理解

3. [Task Execution]
   - Write: auth.ts を作成
   - Edit: routes.ts に認証ルートを追加
   - TodoWrite: ステータスを "in_progress" に更新

4. [Testing]
   - Bash: "npm test"
   - 失敗 → Edit: テストを修正
   - Bash: "npm test" 再実行 → 成功

5. [Quality Check]
   - Bash: "npm run lint"
   - Bash: "npm run typecheck"
   
6. [Completion]
   - TodoWrite: すべて "completed" に更新
   - ユーザーに結果を報告
```
