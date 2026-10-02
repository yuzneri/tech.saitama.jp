# tech.saitama.jp — Tech Saitama コミュニティサイト

Tech Saitama（[盆栽.dev](https://bonsai-dev.connpass.com/) が主催する埼玉のエンジニアコミュニティ）の情報をまとめた Hugo サイト。

## 必要なもの

- Hugo extended v0.146 以上（新テンプレート構造を使用）

## 開発

```sh
hugo server        # http://localhost:1313/ でプレビュー
hugo build         # public/ に静的ファイルを生成
```

※ イベントは未来日付で作るため `buildFuture = true` を設定済み。

## デプロイ

`main` への push で GitHub Actions（`.github/workflows/hugo.yaml`）がビルドし、GitHub Pages に公開する。
開催予定/過去の振り分けを更新するため、毎日 0:00 JST にも再ビルドする。
`public/` はビルド成果物なのでコミットしない。

初回のみリポジトリ側で次を設定する。

- Settings > Pages > Build and deployment の Source を「GitHub Actions」にする
- 同じ画面の Custom domain に `tech.saitama.jp` を設定し、DNS で `tech.saitama.jp` の CNAME を `<ユーザー名>.github.io` に向ける

## イベントの追加

```sh
hugo new events/2026-07-example.md
```

front matter の `date`（開始日時）・`venue`・`address`・`fee`・`connpass`（イベントURL）・
`endTime`（終了時刻の文字列）を埋め、本文に概要を書いて `draft: true` を外す。
開催予定/過去の振り分けは `date` とビルド時刻の比較で自動。

## 構成

- `content/_index.md` — トップ（コミュニティ紹介）
- `content/code-of-conduct.md` — 行動規範
- `content/events/` — イベント（1イベント = 1ファイル）
- `layouts/` — 自作の最小テーマ（baseof / home / section / page + event-card partial）
- `assets/css/main.css` — スタイル一式
