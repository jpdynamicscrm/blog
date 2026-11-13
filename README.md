# Japan Dynamics CRM & Power Platform Support Blog

日本マイクロソフト Dynamics CRM & Power Platform サポート チームのブログです。

公開先: [https://jpdynamicscrm.github.io/blog/](https://jpdynamicscrm.github.io/blog/)

## 記事を追加・更新する

1. `articles/` の下の該当するフォルダー (`powerapps`、`powerautomate`、`dataverse` など) に Markdown ファイルを置きます。
   - 新規作成・リネームするファイルの名前は kebab-case にします (例: `how-to-use-xyz.md`)。
   - 画像は記事と同じ名前のフォルダーに置き、`![](./how-to-use-xyz/01-image.png)` のように参照します。
2. ファイルの先頭は次の形で書きます。

   ```markdown
   ---
   title: 記事のタイトル
   date: 2026-11-12 10:00
   tags:
     - Power Apps
   categories:
     - [Power Apps, Canvas app]
   ---

   こんにちは、Power Platform サポート チームです。

   本記事では、〇〇についてご案内します。

   <!-- more -->

   (本文)
   ```

3. Pull Request を作成すると、次の項目が CI で自動チェックされます。

   | 項目 | ルール |
   | --- | --- |
   | ファイル名 | 新規・リネーム時は kebab-case |
   | `title` | 必須 |
   | `date` | `YYYY-MM-DD`、`YYYY-MM-DD HH:mm`、`YYYY-MM-DD HH:mm:ss` のいずれか |
   | `tags` | 1 つ以上 |
   | `categories` | 1 つ以上。`- [Power Apps, Canvas app]` の形式で、[`_config.yml`](_config.yml) の `root_cat_names` にあるもの |
   | `<!-- more -->` | 本文に必須 (トップページの記事一覧で、ここまでが表示されます) |

Pull Request が `master` にマージされると、サイトに自動で公開されます。

## ローカルでプレビューする

Docker が必要です。

```bash
git submodule update --init
docker compose up
```

起動後、[http://localhost:4000/blog/](http://localhost:4000/blog/) で確認できます。
