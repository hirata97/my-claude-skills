# Conventional Commits チートシート

## 基本フォーマット

```
<type>[optional scope]: <description>

[optional body]

[optional footer(s)]
```

## Type一覧

| Type | 説明 | 例 |
|------|------|-----|
| `feat` | 新機能 | `feat: ユーザー登録機能を追加` |
| `fix` | バグ修正 | `fix: ログイン失敗時のエラー処理を修正` |
| `docs` | ドキュメント | `docs: API仕様書を更新` |
| `style` | コードスタイル | `style: インデントを統一` |
| `refactor` | リファクタリング | `refactor: コンポーネント構造を整理` |
| `perf` | パフォーマンス | `perf: データベースクエリを最適化` |
| `test` | テスト | `test: ユニットテストを追加` |
| `build` | ビルド | `build: webpackの設定を更新` |
| `ci` | CI/CD | `ci: GitHub Actionsワークフローを追加` |
| `chore` | その他 | `chore: .gitignoreを更新` |
| `revert` | 取り消し | `revert: feat: ユーザー登録機能を追加` |

## よく使うScope例

- `auth` - 認証関連
- `api` - API関連
- `ui` - UI/UX関連
- `database` - データベース関連
- `deps` - 依存関係
- `config` - 設定ファイル
- `test` - テスト関連

## 実践例

### シンプルなコミット

```bash
feat: ダークモード機能を追加
fix: ログアウトボタンが動作しない問題を修正
docs: README に環境変数の説明を追加
```

### Scope付きコミット

```bash
feat(auth): パスワードリセット機能を実装
fix(api): ユーザー削除時の権限チェックを修正
refactor(ui): ボタンコンポーネントを再設計
```

### Body付きコミット

```bash
feat(payment): サブスクリプション機能を追加

月額・年額プランをサポート。
Stripe APIを使用して定期課金を実装。

- プラン選択UI
- 自動更新処理
- キャンセル機能
```

### 破壊的変更

```bash
feat(api): 認証方式をJWTに変更

BREAKING CHANGE: セッションベースの認証が廃止されました。
すべてのクライアントはJWTトークンを使用する必要があります。
```

### Issue参照

```bash
fix(ui): モバイルでのナビゲーションバグを修正

画面幅が768px以下の場合にメニューが表示されない問題を解決。

Fixes #42
Refs #38
```

## コミット作成のベストプラクティス

1. **1コミット = 1つの論理的変更**
   - 無関係な変更は分割する
   - 例：バグ修正と新機能は別々にコミット

2. **descriptionは命令形で書く**
   - ✅ `fix: エラーを修正`
   - ❌ `fix: エラーを修正した`
   - ❌ `fix: エラーの修正`

3. **descriptionは簡潔に**
   - 50文字以内を目標
   - 詳細はbodyに記述

4. **bodyで「なぜ」を説明**
   - コードを見れば「何を」は分かる
   - 「なぜ」その変更が必要だったかを説明

5. **破壊的変更は明示**
   - `BREAKING CHANGE:` を必ず使用
   - 移行方法も記載

## NGパターン

❌ `update: いろいろ修正`
- typeが不適切、descriptionが不明瞭

❌ `feat: ログイン機能追加、バグ修正、テスト追加`
- 複数の変更を1つのコミットにまとめている

❌ `Fixed the bug.`
- Conventional Commits形式に従っていない

❌ `feat(auth): Added user registration.`
- descriptionが命令形ではない

## ツール

### Commitizen
対話的にConventional Commitsを生成

```bash
npm install -g commitizen
commitizen init cz-conventional-changelog --save-dev --save-exact
```

### commitlint
コミットメッセージを検証

```bash
npm install --save-dev @commitlint/cli @commitlint/config-conventional
```

### husky
Gitフックでコミットメッセージをチェック

```bash
npm install --save-dev husky
npx husky install
npx husky add .husky/commit-msg 'npx --no-install commitlint --edit $1'
```
