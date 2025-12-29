---
name: commit-helper
description: Conventional Commits形式のコミットメッセージを生成する支援スキル
enabled: true
---

# Commit Helper Skill

このスキルは、Conventional Commits形式に準拠したコミットメッセージの生成を支援します。

## 使用方法

このスキルが有効な場合、Claudeはコミット作成時に以下のルールに従います。

## Conventional Commits形式

コミットメッセージは以下の形式に従う必要があります：

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

### Type（必須）

変更の種類を示します：

- `feat`: 新機能の追加
- `fix`: バグ修正
- `docs`: ドキュメントのみの変更
- `style`: コードの動作に影響しない変更（空白、フォーマット、セミコロンの欠落など）
- `refactor`: バグ修正や機能追加を含まないコード変更
- `perf`: パフォーマンスを向上させるコード変更
- `test`: テストの追加や既存テストの修正
- `build`: ビルドシステムや外部依存関係に影響する変更（npm, webpack, etc）
- `ci`: CI設定ファイルやスクリプトの変更（GitHub Actions, CircleCI, etc）
- `chore`: その他の変更（.gitignore更新など）
- `revert`: 以前のコミットの取り消し

### Scope（オプション）

変更の影響範囲を示します（例: `auth`, `api`, `ui`, `database`）

### Description（必須）

変更内容の簡潔な説明：

- 命令形、現在形を使用（"change" であって "changed" や "changes" ではない）
- 最初の文字は小文字
- 末尾にピリオドを付けない
- 50文字以内を推奨

### Body（オプション）

変更の詳細な説明：

- WHY（なぜ）この変更が必要か
- WHAT（何を）変更したか
- 以前の動作との違い
- 1行空けてdescriptionから分離

### Footer（オプション）

破壊的変更やIssue参照：

- `BREAKING CHANGE:` で破壊的変更を明示
- `Fixes #123` でIssueをクローズ
- `Refs #456` でIssue参照

## コミットメッセージ生成ルール

1. **変更内容の分析**
   - git diffやgit statusの出力を確認
   - 変更されたファイルと内容を理解
   - 変更の目的と影響範囲を特定

2. **適切なTypeの選択**
   - 変更内容に最も適したtypeを選択
   - 複数のtypeが該当する場合は、主要な変更に基づいて選択

3. **Scopeの決定**
   - プロジェクト構造に基づいてscopeを決定
   - 複数のscopeにまたがる場合は省略または最も重要なものを選択

4. **Descriptionの作成**
   - 簡潔かつ明確に
   - 技術的な詳細は避け、変更の本質を伝える
   - 日本語プロジェクトの場合は日本語で記述

5. **Bodyの追加（必要に応じて）**
   - 複雑な変更の場合は詳細を追加
   - 設計判断の理由を説明
   - 代替案を検討した場合はその内容も記載

6. **Footerの追加（必要に応じて）**
   - 破壊的変更がある場合は必ず `BREAKING CHANGE:` を追加
   - 関連するIssueがあれば参照を追加

## 例

### 基本例

```
feat(auth): ユーザー登録機能を追加
```

```
fix(api): ユーザー取得時の404エラーを修正
```

```
docs: READMEにインストール手順を追加
```

### Scope付き

```
refactor(database): ユーザーモデルのクエリを最適化
```

```
test(ui): ログインフォームのE2Eテストを追加
```

### Body付き

```
feat(payment): Stripe決済統合を追加

顧客からの要望により、クレジットカード決済機能を実装。
Stripe APIを使用して安全な決済処理を実現。

- 決済フォームコンポーネント追加
- Webhook処理の実装
- 決済履歴の保存機能
```

### 破壊的変更

```
feat(api): 認証APIをv2に移行

BREAKING CHANGE: 認証エンドポイントが /api/v1/auth から /api/v2/auth に変更されました。
クライアントアプリケーションは新しいエンドポイントを使用するように更新する必要があります。
```

### Issue参照

```
fix(ui): モバイル表示時のレイアウト崩れを修正

Fixes #123
```

## 注意事項

- コミットメッセージは**必ず**Conventional Commits形式に従う
- 変更内容を正確に反映したtypeを選択する
- descriptionは簡潔かつ明確に（50文字以内推奨）
- 複数の無関係な変更は分割してコミットする
- 日本語プロジェクトの場合は日本語で記述（英語プロジェクトは英語）
- 自動生成の署名は末尾に追加する

## 参考リンク

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Angular Commit Message Guidelines](https://github.com/angular/angular/blob/main/CONTRIBUTING.md#commit)
