# GitHub Pages デプロイメントガイド

## GitHub Pages の有効化方法

1. リポジトリの設定ページに移動します（Settings > Pages）
2. "Source" セクションで、デプロイソースを選択します
3. "Save" をクリックして設定を保存します

## デプロイソースの選択

以下のいずれかの方法でデプロイソースを選択できます：

### main ブランチ

- メインブランチのルートディレクトリをデプロイソースとして使用
- シンプルな構成で、小規模なプロジェクトに適しています

### gh-pages ブランチ

- 専用のデプロイメントブランチを作成
- プロダクションコードとデプロイメントコードを分離できます
- 以下のコマンドで作成可能：
  ```bash
  git checkout -b gh-pages
  git push origin gh-pages
  ```

### docs フォルダ

- プロジェクト内の `docs` ディレクトリをデプロイソースとして使用
- ドキュメントとウェブサイトを同じリポジトリで管理できます

## カスタムドメインの設定方法

1. リポジトリの設定ページで "Custom domain" セクションを探します
2. カスタムドメインを入力（例：`example.com`）
3. "Save" をクリック
4. DNS レコードの設定：
   - A レコード：`185.199.108.153`、`185.199.109.153`、`185.199.110.153`、`185.199.111.153`
   - CNAME レコード：`yourusername.github.io`

## GitHub Actions による自動デプロイ

以下のワークフローファイルを作成して自動デプロイを設定できます：

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches:
      - main # または gh-pages

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2

      - name: Setup Node.js
        uses: actions/setup-node@v2
        with:
          node-version: "16"

      - name: Install Dependencies
        run: npm install

      - name: Build
        run: npm run build

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist # ビルド出力ディレクトリ
```

このワークフローは以下のタイミングで実行されます：

- main ブランチへのプッシュ時
- 手動トリガー時

注意事項：

- `GITHUB_TOKEN` は自動的に提供されます
- `publish_dir` は実際のビルド出力ディレクトリに合わせて変更してください
