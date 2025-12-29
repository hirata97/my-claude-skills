# CLAUDE.md

このファイルは、Claude Code（claude.ai/code）がこのリポジトリでコードを作業する際のガイダンスを提供します。

## リポジトリ概要

これは Claude Code 用の**スキル・コマンドライブラリ**であり、再利用可能な開発ワークフローと自動化ツールを含んでいます。カスタムスキル（自動適用されるコンテキスト）とスラッシュコマンド（明示的なワークフロー）の一元管理されたコレクションとして機能し、グローバルまたはプロジェクト単位でデプロイできます。

## アーキテクチャ

### 2 層システム：スキル vs コマンド

```
my-claude-skills/
├── skills/                    # 自動適用コンテキスト（常時読み込み）
│   └── commit-helper/         # Conventional Commits強制
│       ├── SKILL.md           # メインスキル定義（YAMLフロントマター + 指示）
│       ├── references/        # サポートドキュメント
│       └── examples/          # 使用例
└── commands/                  # 明示的ワークフロー（/command-nameで呼び出し）
    ├── start-work/            # Issue → 実装 → PR ワークフロー
    │   └── COMMAND.md         # コマンド定義（YAMLフロントマター + 指示）
    └── create-issue/          # Issue作成品質保証
        └── COMMAND.md
```

### 主な違い

| タイプ       | 起動方法                   | 用途                       | 配置場所                                         |
| ------------ | -------------------------- | -------------------------- | ------------------------------------------------ |
| **スキル**   | 自動（常にコンテキスト内） | ガイドライン、ルール、知識 | `~/.claude/skills/` または `.claude/skills/`     |
| **コマンド** | 手動（`/command-name`）    | ワークフロー、アクション   | `~/.claude/commands/` または `.claude/commands/` |

## 利用可能なスキル

### commit-helper

全てのコミットメッセージに Conventional Commits 形式を強制する**自動適用スキル**。

**主要ルール:**

- 必須形式: `<type>[optional scope]: <description>`
- サポートされる type: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`
- Description: 命令形、小文字、ピリオドなし、50 文字以内推奨
- オプションの body: 変更の理由（WHY）を説明
- オプションの footer: `BREAKING CHANGE:`, `Fixes #123`, `Refs #456`

**例:**

```
feat(auth): ユーザー登録にメール認証を追加

メールベースのサインアップ機能の要望に対応。

Closes #42
```

## 利用可能なコマンド

### /start-work

Issue 選択 → 調査 → 実装 → テスト → PR 作成を自動化する**エンドツーエンドワークフロー**。

**実行フロー:**

1. `gh issue list --state open`でオープンな Issue を取得
2. 優先度（P0 > P1 > P2）とサイズ（S > M > L）に基づいて自動選択
3. 要件を分析して実装計画を作成
4. フィーチャーブランチ作成: `feature/issue-[番号]-[description]`
5. テストと共に段階的に実装
6. 品質チェック実行（lint、型チェック、test、build）
7. `Closes #[issue]`を含む自動生成されたサマリーで PR 作成

**重要なルール:**

- フィーチャーブランチ作成前に**必ず** `git pull origin main` を実行
- `main`ブランチへの直接コミットは**絶対禁止**
- フィーチャーブランチを使用: `feature/issue-123-description`
- 新規コードには必ずテストが必要
- PR 作成前に全ての品質チェックが通過必須

### /create-issue

適切に構造化された GitHub Issue を作成するための**品質保証ワークフロー**。

**実行フロー:**

1. 要件を理解するための対話的ディスカッション
2. 単一 Issue または Parent-Child 構造が必要かを判定
3. タイトル形式の検証（親チケット: `[親チケット] description`）
4. 適切なラベルを適用: `priority:`, `size:`, `type:`
5. 正しいテンプレートを使用（feature/bug/parent）
6. 全ての必須フィールドを検証
7. `gh issue create`で Issue 作成

**ラベルシステム:**

- **Priority**: `priority:P0`（緊急）、`priority:P1`（高）、`priority:P2`（通常）
- **Size**: `size:S`（1-2 日）、`size:M`（3-5 日）、`size:L`（1 週間以上）
- **Type**: `type:feature`, `type:bugfix`, `type:enhancement`, `type:refactor`等

**親子チケット構造:**

- 親チケット: タイトルが`[親チケット]`で始まる、サイズは通常`L`または`M`
- 子チケット: 本文に`親チケット: #XXX`を含む、サイズは通常`S`または`M`

## デプロイ方法

### グローバルデプロイ（推奨）

一度デプロイすれば全プロジェクトで使用可能:

```bash
# リポジトリをクローン
git clone <repo-url> my-claude-skills
cd my-claude-skills

# グローバルにデプロイ
cp -r skills/* ~/.claude/skills/
cp -r commands/* ~/.claude/commands/
```

### プロジェクト固有デプロイ

プロジェクトごとにグローバル設定を上書き:

```bash
cd /path/to/project

# プロジェクトにデプロイ
cp -r /path/to/my-claude-skills/skills/* .claude/skills/
cp -r /path/to/my-claude-skills/commands/* .claude/commands/
```

### シンボリックリンクデプロイ（自動更新）

リポジトリの変更が自動的に反映される:

```bash
# グローバル
ln -s /path/to/my-claude-skills/skills/* ~/.claude/skills/
ln -s /path/to/my-claude-skills/commands/* ~/.claude/commands/

# プロジェクト固有
ln -s /path/to/my-claude-skills/skills/* /path/to/project/.claude/skills/
ln -s /path/to/my-claude-skills/commands/* /path/to/project/.claude/commands/
```

## 設定の優先順位

Claude Code は以下の優先順位で設定を読み込みます:

1. **プロジェクト固有**（`.claude/`）- 最優先
2. **グローバル**（`~/.claude/`）- フォールバック

両方が存在する場合、プロジェクト固有の設定がグローバル設定を上書きします。

## 新しいスキルの作成

### 構造

```bash
mkdir -p skills/my-skill/{references,examples}
```

### SKILL.md 形式

```markdown
---
name: my-skill
description: 簡潔な説明
enabled: true
---

# My Skill

Claude への詳細な指示を Markdown で記述...

## ルール

- ルール 1
- ルール 2
```

### デプロイ

```bash
cp -r skills/my-skill ~/.claude/skills/
```

## 新しいコマンドの作成

### 構造

```bash
mkdir -p commands/my-command
```

### COMMAND.md 形式

```markdown
---
name: my-command
description: 簡潔な説明
enabled: true
---

# My Command

Claude への詳細なワークフロー指示を Markdown で記述...

## 実行フロー

1. ステップ 1
2. ステップ 2
```

### デプロイ

```bash
cp -r commands/my-command ~/.claude/commands/
```

## 一般的なワークフロー

### 既存プロジェクトへの追加

グローバルデプロイ済みの場合:

```bash
cd /path/to/your/project
# コマンドは即座に利用可能
/start-work
/create-issue
```

プロジェクト固有のカスタマイズが必要な場合:

```bash
cd /path/to/your/project
mkdir -p .claude/{skills,commands}
cp ~/.claude/commands/start-work.md .claude/commands/
# プロジェクト固有の要件に合わせて編集
vim .claude/commands/start-work.md
```

### 典型的な開発フロー

```bash
# 1. 品質チェック付きで新しいIssueを作成
/create-issue

# 2. Issueの作業を開始（自動選択または手動）
/start-work

# 3. コミットは自動的にConventional Commits形式を使用
git commit
# 結果: "feat(feature-name): add new functionality"

# 4. PRが適切な形式で自動作成される
```

## ファイル命名規則

- スキル: スキルディレクトリ内の`SKILL.md`（必須）
- コマンド: コマンドディレクトリ内の`COMMAND.md`（必須）
- リファレンス: `references/*.md`（オプション）
- 例: `examples/*.md`（オプション）

## YAML フロントマターの要件

全てのスキルとコマンドには YAML フロントマターが必要:

```yaml
---
name: unique-identifier
description: 簡潔な説明（ヘルプに表示される）
enabled: true
---
```

## マーケットプレイス設定

このリポジトリには配布用の`.claude-plugin/marketplace.json`が含まれています:

- **Owner**: hirata97
- **Version**: 1.0.0
- **Plugins**: commit-helper, start-work, create-issue

## バージョン管理

- スキルとコマンドは Git でバージョン管理
- 大きな変更は main にマージする前にフィーチャーブランチでテスト
- このリポジトリでも conventional commits を使用（commit-helper スキル経由）

## トラブルシューティング

### コマンドが認識されない

```bash
# デプロイを確認
ls -la ~/.claude/commands/
cat ~/.claude/commands/start-work/COMMAND.md

# Claude Codeを再起動
```

### スキルが適用されない

```bash
# スキルファイルを確認
cat ~/.claude/skills/commit-helper/SKILL.md

# フロントマターでenabled: trueを確認
grep "enabled:" ~/.claude/skills/commit-helper/SKILL.md

# Claude Codeを再起動
```

### 設定の競合

```bash
# どの設定が有効か確認
ls -la .claude/          # プロジェクト固有（優先度高）
ls -la ~/.claude/        # グローバル（フォールバック）
```

## 他プロジェクトとの統合

このスキルライブラリは複数のプロジェクトで動作するように設計されています。プロジェクト固有の要件に対して、グローバルスキルは以下に適応します:

- プロジェクト固有のブランチ命名規則
- カスタム PR 作成コマンド
- プロジェクト固有の品質チェックコマンド
- プロジェクト固有の Issue ラベルシステム
