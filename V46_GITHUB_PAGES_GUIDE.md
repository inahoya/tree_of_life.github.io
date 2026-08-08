# v46 GitHub Pages 更新手順

v46では、ページ起動時に同じディレクトリの `all_in_one.json` を自動読み込みします。

## GitHub Pagesでの最短運用

1. `phylogeny_v46.html` を GitHub Pages の `index.html` として置くか、内容を既存 `index.html` に反映する。
2. v46 Editorでデータを編集する。
3. `all_in_one_v46.json` を書き出す。
4. そのファイルを `all_in_one.json` にリネームする。
5. GitHubで公開HTMLと同じ階層にある `all_in_one.json` を上書きする。
6. GitHub Pagesを再読み込みする。

通常はこれだけで内容が反映されます。

## 反映されないとき

ページ上部の `all_in_one再読込` を押してください。
このボタンはキャッシュ回避付きで `all_in_one.json` を再取得します。

それでも反映されない場合は、GitHub Pages上の配置が次の形になっているか確認してください。

```text
/
├─ index.html        ← v46
└─ all_in_one.json   ← 最新データ
```

HTMLとJSONが別フォルダの場合、現在のv46は自動読込できません。

## ローカルでHTMLを直接開く場合

`file://` で開いたHTMLからの `fetch("all_in_one.json")` はブラウザ制限で失敗することがあります。
その場合は従来どおり `all_in_one読込` を使ってください。

GitHub Pages上では自動読込が本来の使い方です。
