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

Cloudflare Pages でビルドし公開する。`main` への push でビルドが走る。
開催予定/過去の振り分けを更新するため、[connpass-sync](https://github.com/yuzneri/connpass-sync) が毎日 6:00 JST に deploy hook を叩いて再ビルドする。
`public/` はビルド成果物なのでコミットしない。

Cloudflare Pages のビルドコマンドは次のとおり。
Hugo でビルドしたあと、[HugoOGPImageGenerator](https://github.com/yuzneri/HugoOGPImageGenerator) で各ページの OGP 画像を `public/<ページ>/ogp.png` に生成する。

```sh
hugo && curl -fsSL https://raw.githubusercontent.com/yuzneri/HugoOGPImageGenerator/main/install.sh | INSTALL_DIR=bin VERSION=v26 sh && bin/ogp . --config ogp/config.yaml
```

## OGP 画像

- トップページは `static/ogp.png` をそのまま使う（生成しない）。
- それ以外のページは `ogp/config.yaml` の設定で生成する。背景は `ogp/background.png`、イベントは `ogp/events.yaml` でタイトルの下に日時と会場を入れる。
- `ogp/background.png` と `static/ogp.png` は、`ogp/source/` の元画像（`background.png`・`top.png`）を 1200x630 に縮小（トップは上下を中央で切り詰め）したもの。ジェネレーターは背景を拡縮しないため、出力サイズに合わせてある。
- フォントは `ogp/fonts/` の BIZ UDPGothic（SIL OFL 1.1、`ogp/fonts/OFL.txt`）。
- 手元で確認するときも、上のビルドコマンドと同じく `hugo` のあとに `bin/ogp . --config ogp/config.yaml` を実行する。

## イベントの追加

```sh
hugo new events/2026-07-example.md
```

connpass のイベントは connpass-sync が自動で追加する。手で追加する場合は上のコマンドで作り、
front matter の `date`（開始日時）・`end`（終了日時）・`venue`・`address`・`fee`・`connpass`（イベントURL）を埋め、本文に概要を書いて `draft: true` を外す。
開催予定/過去の振り分けは `date` とビルド時刻の比較で自動。

イベントは RSS（`/events/index.xml`）と iCalendar（`/events/index.ics`）にも自動で出力される。
テンプレートは `layouts/events/section.rss.xml` と `layouts/events/section.calendar.ics`。

## 構成

- `content/_index.md` — トップ（コミュニティ紹介）
- `content/code-of-conduct.md` — 行動規範
- `content/events/` — イベント（1イベント = 1ファイル）
- `layouts/` — 自作の最小テーマ（baseof / home / section / page + event-card partial）
- `assets/css/main.css` — スタイル一式
