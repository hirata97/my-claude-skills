# My Claude Skills

個人用のClaude Codeカスタムスキルコレクション

## 概要

このリポジトリは、Claude Codeで使用するカスタムスキル（スラッシュコマンド）を管理するためのものです。各スキルは独立したディレクトリで管理され、プロジェクト間で再利用可能です。

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

### 2. Start Work (`start-work`)

GitHub Issueから作業を自動選択し、実装→テスト→PR作成まで一貫実行するワークフロースキル。

**機能:**
- オープンなIssueから優先度・サイズを考慮して自動選択
- 技術調査・実装計画の自動立案
- フィーチャーブランチの自動作成
- 段階的実装サポート
- PR自動作成

**使用方法:** `/start-work`

**詳細:** [skills/start-work/SKILL.md](skills/start-work/SKILL.md)

### 3. Create Issue (`create-issue`)

Issue品質チェック・作成ワークフローを実行するエージェントスキル。

**機能:**
- 対話的なIssue作成サポート
- タイトル・ラベル・説明文の品質チェック
- 必須項目の自動バリデーション
- 親子チケット構造の提案
- テンプレート自動選択

**使用方法:** `/create-issue`

**詳細:** [skills/create-issue/SKILL.md](skills/create-issue/SKILL.md)

## インストール

### 方法1: グローバル配置（推奨）

全プロジェクトでスキルを使用する場合：

```bash
# リポジトリをクローン
git clone <this-repository-url> my-claude-skills
cd my-claude-skills

# スキルをグローバルディレクトリにコピー
cp -r skills/* ~/.claude/skills/
```

### 方法2: プロジェクト固有配置

特定のプロジェクトでのみ使用する場合：

```bash
# プロジェクトディレクトリで実行
cd /path/to/your/project

# スキルをプロジェクトディレクトリにコピー
cp -r /path/to/my-claude-skills/skills/* .claude/skills/
```

### 方法3: シンボリックリンク

スキルの更新を自動反映させる場合：

```bash
# グローバル配置の場合
ln -s /path/to/my-claude-skills/skills/* ~/.claude/skills/

# プロジェクト固有の場合
ln -s /path/to/my-claude-skills/skills/* /path/to/project/.claude/skills/
```

## 使用方法

### スキルの有効化確認

Claude Codeでスキルが読み込まれているか確認：

```bash
# Claude Codeのセッションで確認
# スキルは自動的に読み込まれます
```

### スキルの使用

スキルは自動的にClaude Codeのコンテキストに読み込まれます。特定のスキルを明示的に呼び出す必要はなく、Claude Codeが自動的に適用します。

例えば、`commit-helper` スキルが有効な場合：

```bash
# Claudeにコミット作成を依頼
> git commitを作成して
```

Claudeは自動的にConventional Commits形式でコミットメッセージを生成します。

## ディレクトリ構造

```
my-claude-skills/
├── README.md                          # このファイル
├── skills/                            # スキルディレクトリ
│   ├── commit-helper/                 # コミット支援スキル
│   │   ├── SKILL.md                   # スキル定義（必須）
│   │   ├── references/                # リファレンス資料
│   │   │   └── conventional-commits-cheatsheet.md
│   │   └── examples/                  # 使用例
│   │       └── example-commits.md
│   ├── start-work/                    # 作業開始ワークフロー
│   │   └── SKILL.md                   # スキル定義
│   └── create-issue/                  # Issue作成エージェント
│       └── SKILL.md                   # スキル定義
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
- commit-helper スキル追加
- start-work スキル追加（refeel/.claudeから汎用化）
- create-issue スキル追加（refeel/.claudeから汎用化）
