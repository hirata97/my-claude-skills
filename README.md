# My Claude Skills

個人用のClaude Codeカスタマイゼーションコレクション

## 概要

このリポジトリは、Claude Codeで使用するカスタムスキルとスラッシュコマンドを管理するためのものです。プロジェクト間で再利用可能な開発ワークフローを提供します。

## スキル vs スラッシュコマンド

### スキル（Skills）
- **自動適用**: Claudeのコンテキストに常時読み込まれる
- **用途**: ガイドライン、ルール、常時適用される知識
- **配置**: `~/.claude/skills/`

### スラッシュコマンド（Commands）
- **明示的実行**: `/command-name` で呼び出し
- **用途**: 特定のワークフロー、アクション
- **配置**: `~/.claude/commands/`

## 含まれるスキル

### 1. Commit Helper (`commit-helper`)

Conventional Commits形式に準拠したコミットメッセージを生成する支援スキル。

**機能:**
- Conventional Commits形式のコミットメッセージ自動生成
- 適切なtype（feat, fix, docs, etc.）の選択
- Scope、Body、Footerの適切な使用
- 破壊的変更（BREAKING CHANGE）の明示
- Issue参照の自動追加

**詳細:** [skills/commit-helper/SKILL.md](skills/commit-helper/SKILL.md)

## 含まれるスラッシュコマンド

### 1. Start Work (`/start-work`)

GitHub Issueから作業を自動選択し、実装→テスト→PR作成まで一貫実行するワークフローコマンド。

**機能:**
- オープンなIssueから優先度・サイズを考慮して自動選択
- 技術調査・実装計画の自動立案
- フィーチャーブランチの自動作成
- 段階的実装サポート
- PR自動作成

**使用方法:**
```bash
/start-work
```

**詳細:** [commands/start-work.md](commands/start-work.md)

### 2. Create Issue (`/create-issue`)

Issue品質チェック・作成ワークフローを実行するエージェントコマンド。

**機能:**
- 対話的なIssue作成サポート
- タイトル・ラベル・説明文の品質チェック
- 必須項目の自動バリデーション
- 親子チケット構造の提案
- テンプレート自動選択

**使用方法:**
```bash
/create-issue
```

**詳細:** [commands/create-issue.md](commands/create-issue.md)

## インストール

### 方法1: グローバル配置（推奨）

全プロジェクトでスキルとコマンドを使用する場合：

```bash
# リポジトリをクローン
git clone <this-repository-url> my-claude-skills
cd my-claude-skills

# スキルとコマンドをグローバルディレクトリにコピー
cp -r skills/* ~/.claude/skills/
cp -r commands/* ~/.claude/commands/
```

### 方法2: プロジェクト固有配置

特定のプロジェクトでのみ使用する場合：

```bash
# プロジェクトディレクトリで実行
cd /path/to/your/project

# スキルとコマンドをプロジェクトディレクトリにコピー
cp -r /path/to/my-claude-skills/skills/* .claude/skills/
cp -r /path/to/my-claude-skills/commands/* .claude/commands/
```

### 方法3: シンボリックリンク

更新を自動反映させる場合：

```bash
# グローバル配置の場合
ln -s /path/to/my-claude-skills/skills/* ~/.claude/skills/
ln -s /path/to/my-claude-skills/commands/* ~/.claude/commands/

# プロジェクト固有の場合
ln -s /path/to/my-claude-skills/skills/* /path/to/project/.claude/skills/
ln -s /path/to/my-claude-skills/commands/* /path/to/project/.claude/commands/
```

## 使用方法

### スキルの使用（自動適用）

スキルは自動的にClaude Codeのコンテキストに読み込まれます。

**例**: `commit-helper` スキル
```bash
# Claudeにコミット作成を依頼
> git commitを作成して
```
→ 自動的にConventional Commits形式でコミットメッセージを生成

### スラッシュコマンドの使用（明示的実行）

スラッシュコマンドは明示的に呼び出します。

**例**: `/start-work` コマンド
```bash
# Claude Codeで実行
/start-work
```
→ Issue自動選択→実装→PR作成のワークフローを開始

**例**: `/create-issue` コマンド
```bash
# Claude Codeで実行
/create-issue
```
→ Issue品質チェック・作成ワークフローを開始

## 他リポジトリでの使用手順

### グローバル配置している場合（推奨）

グローバル配置（`~/.claude/`）を使用している場合、**追加設定不要**で全プロジェクトで利用可能です。

```bash
# 任意のプロジェクトに移動
cd /path/to/your/project

# Claude Codeを起動
# スキルとコマンドは自動的に利用可能

# スラッシュコマンドを実行
/start-work
/create-issue
```

### 既存プロジェクトでの確認

#### 1. スキル・コマンドの確認

```bash
# グローバル配置の確認
ls -la ~/.claude/skills/
ls -la ~/.claude/commands/

# プロジェクト固有の確認（存在する場合）
ls -la /path/to/project/.claude/skills/
ls -la /path/to/project/.claude/commands/
```

#### 2. プロジェクトでの動作確認

```bash
cd /path/to/your/project

# Claude Codeを起動して確認
# - コミット作成時に自動的にConventional Commits形式が適用されるか
# - /start-work コマンドが認識されるか
# - /create-issue コマンドが認識されるか
```

### プロジェクト固有の設定（オプション）

特定のプロジェクトでのみカスタマイズしたい場合：

#### ステップ1: プロジェクト固有ディレクトリ作成

```bash
cd /path/to/your/project
mkdir -p .claude/skills
mkdir -p .claude/commands
```

#### ステップ2: コマンドのコピーまたはカスタマイズ

```bash
# グローバル版をコピー
cp ~/.claude/commands/start-work.md .claude/commands/
cp ~/.claude/commands/create-issue.md .claude/commands/

# プロジェクト固有にカスタマイズ
vim .claude/commands/start-work.md
```

#### ステップ3: .gitignoreの設定

```bash
# プロジェクトの.gitignoreに追加（個人設定の場合）
echo ".claude/settings.local.json" >> .gitignore

# チーム共有する場合は.gitignoreに追加しない
```

### 実践例

#### 例1: refeelプロジェクト

```bash
cd /home/mizuki/projects/refeel

# グローバルコマンドが利用可能
/start-work      # ✅ Issue自動選択→実装→PR
/create-issue    # ✅ Issue品質チェック・作成

# コミット時に自動適用
git commit       # ✅ Conventional Commits形式で作成
```

#### 例2: personal-portfolio

```bash
cd /home/mizuki/projects/personal-portfolio

# 同様にグローバルコマンドが利用可能
/start-work      # ✅ ポートフォリオのIssue管理
/create-issue    # ✅ Issue作成
```

#### 例3: 新規プロジェクト

```bash
# 新しいプロジェクトを作成
mkdir new-project
cd new-project
git init

# Claude Codeを起動
# スキル・コマンドは自動的に利用可能（グローバル配置済み）

# すぐにワークフローを開始
/create-issue    # Issue作成
/start-work      # 作業開始
```

### プロジェクト固有のカスタマイズ例

プロジェクトごとに異なる設定が必要な場合：

```bash
cd /path/to/project

# プロジェクト固有のコマンドを作成
cat > .claude/commands/custom-workflow.md << 'EOF'
---
description: このプロジェクト専用のカスタムワークフロー
---

# Custom Workflow

[プロジェクト固有の手順を記述]
EOF
```

### 優先順位

Claude Codeは以下の優先順位で設定を読み込みます：

1. **プロジェクト固有** (`.claude/`) - 最優先
2. **グローバル** (`~/.claude/`) - フォールバック

プロジェクト固有の設定がある場合、グローバル設定より優先されます。

### トラブルシューティング

#### コマンドが認識されない場合

```bash
# 1. グローバル配置を確認
ls -la ~/.claude/commands/

# 2. ファイルが存在するか確認
cat ~/.claude/commands/start-work.md

# 3. Claude Codeを再起動
```

#### スキルが適用されない場合

```bash
# 1. スキルファイルを確認
cat ~/.claude/skills/commit-helper/SKILL.md

# 2. YAMLメタデータを確認
# enabled: true になっているか

# 3. Claude Codeを再起動
```

## ディレクトリ構造

```
my-claude-skills/
├── README.md                          # このファイル
├── skills/                            # スキル（自動適用）
│   └── commit-helper/                 # コミット支援スキル
│       ├── SKILL.md                   # スキル定義（必須）
│       ├── references/                # リファレンス資料
│       │   └── conventional-commits-cheatsheet.md
│       └── examples/                  # 使用例
│           └── example-commits.md
├── commands/                          # スラッシュコマンド（明示的実行）
│   ├── start-work.md                  # 作業開始ワークフロー
│   └── create-issue.md                # Issue作成エージェント
└── .gitignore                         # Git除外設定
```

## 新しいスキルの追加

### ステップ1: スキルディレクトリ作成

```bash
mkdir -p skills/my-new-skill/references
mkdir -p skills/my-new-skill/examples
```

### ステップ2: SKILL.md作成

```bash
cat > skills/my-new-skill/SKILL.md << 'EOF'
---
name: my-new-skill
description: スキルの説明
enabled: true
---

# My New Skill

Claudeへの指示をMarkdownで記述...
EOF
```

### ステップ3: グローバルディレクトリに配置

```bash
cp -r skills/my-new-skill ~/.claude/skills/
```

## スキル開発のベストプラクティス

1. **明確な指示**
   - スキルの目的と使用方法を明確に記述
   - 具体的な例を含める

2. **構造化**
   - SKILL.mdは必須
   - 詳細なドキュメントはreferences/に配置
   - 実例はexamples/に配置

3. **テスト**
   - 新しいスキルは実際のプロジェクトでテスト
   - 想定外の動作がないか確認

4. **バージョン管理**
   - スキルの変更はGitで管理
   - 大きな変更は別ブランチで開発

## トラブルシューティング

### スキルが読み込まれない

1. ファイルパスを確認
   ```bash
   ls -la ~/.claude/skills/
   ```

2. SKILL.mdの形式を確認
   - YAMLメタデータが正しいか
   - `enabled: true` になっているか

3. Claude Codeを再起動

### スキルが適用されない

1. スキルの説明が明確か確認
2. Claudeに明示的に指示してみる
3. スキルのenabled状態を確認

## リソース

### 公式ドキュメント
- [Claude Code Skills Documentation](https://code.claude.com/docs/en/skills)
- [Conventional Commits](https://www.conventionalcommits.org/)

### 参考リポジトリ
- [anthropics/skills](https://github.com/anthropics/skills) - 公式スキル集
- [wshobson/commands](https://github.com/wshobson/commands) - コミュニティスキル集

## ライセンス

MIT License

## 貢献

個人用リポジトリですが、アイデアや改善提案は歓迎します。

## 更新履歴

### 2025-12-29
- 初回リリース
- commit-helper スキル追加（自動適用）
- start-work スラッシュコマンド追加（refeel/.claudeから汎用化）
- create-issue スラッシュコマンド追加（refeel/.claudeから汎用化）
- スキルとコマンドの明確な分離
