---
title: 番組のURL(URL用の名前)を変える
nav_order: 6
summary: 番組の「URL用の名前」の変え方と、変えたときに旧URL・RSS・エピソードのURLがどうなるか(リダイレクトされない)
keywords: [URL, 番組のURL, ポッドキャストのURL, URL用の名前, スラッグ, slug, URL変更, URLを変える, 名前を変える, リダイレクト, 転送, 旧URL, 元のURL, 404, Not Found, RSS, RSS URL, フィードURL, rss.listen.style, itunes:new-feed-url, キャッシュ, Cloudflare, エピソードURL, 詳細設定, ポッドキャスト情報を編集, 設定タブ, 送信, 配信先, Spotify, Apple Podcasts]
checked: 2026-11-26
verified: 一部実機
thirdparty: []
source:
  - 実機で確認(LISTENで配信している番組の「URL用の名前」を変えて、元に戻した。2026-11-26)
  - https://listen.style/document/start_podcast
  - https://listen.style/document/register_listenpodcast
  - https://listen.style/document/moving_podcast
---

# 番組のURL(URL用の名前)を変える

## 要点

- 番組の URL(`https://listen.style/p/〇〇` の `〇〇`)は「**URL用の名前**」。番組を作ったあとでも、「設定」タブの「ポッドキャスト情報を編集」で変えられる。元の名前に戻すこともできた(実機で確認)
- 変えると、**番組ページ・エピソードのページ・RSS の URL がすべて変わる**。**旧 URL から新しい URL へのリダイレクト(転送)はない**(実機で確認)
- RSS の URL も変わるので、Spotify や Apple Podcasts などに配信している番組で変えると、配信先で更新が止まるおそれがある(推測。配信先では試していない)。配信している番組では、変えないほうが安全

## 手順

LISTENで配信している番組の場合:

1. 「ダッシュボード」で番組を選び、「設定」タブを開く
2. 「ポッドキャスト情報を編集」の「**URL用の名前**」(`listen.style/p/` のあとの欄)を新しい名前にする
3. 「送信」を押す

外部の RSS から登録した番組に、この欄があるかどうかは未確認。

## 変えたときに何が起きるか(実機で確認)

2026-11-26 に、LISTENで配信している番組の「URL用の名前」を、ハイフンを減らした名前(例:`my-po-d-cast` → `my-podcast`)に変え、すぐに元の名前へ戻して確かめた。

### 変えたあと

| もの | 旧 URL(変える前の名前) | 新 URL(新しい名前) |
|---|---|---|
| 番組ページ(`listen.style/p/名前`) | 転送されない。タイトルが「Not Found」のページが出る(ステータスは 200) | 表示される |
| エピソードのページ(`listen.style/p/名前/エピソードのID`) | 開けない(404) | 表示される。エピソードの ID の部分は変わらない |
| RSS(`rss.listen.style/p/名前/rss`) | しばらく取れる(下の「キャッシュに注意」) | 取れる。中のリンクも新しい名前に変わる |

- RSS に、配信先に RSS の引っ越しを知らせるタグ(`itunes:new-feed-url`)は入らなかった
- 番組の ID(画像の URL などに出てくる英数字)を使った URL(`listen.style/p/番組のID`)では開けなかった(404)。URL用の名前を変えても使い続けられる URL は、見つからなかった

### 元の名前に戻したあと

- 元の名前で、番組ページ・エピソード・RSS がすべて元どおり使えた
- 一時的に使った名前のほうは、エピソードと RSS が 404 になった。こちらからも転送はない

### キャッシュに注意

- LISTENのページと RSS は Cloudflare を通して配信されていて、古い内容がしばらく残る
  - RSS は `cache-control: public, max-age=86400`(最大24時間)。名前を変えた直後でも、旧 RSS の URL から**変える前の内容**が取れた
  - 番組ページも、名前を戻した数分後に、使わなくなった名前の URL で古いページが表示された
- このため、変えた直後に旧 URL が開けても、**転送されているわけでも、使い続けられるわけでもない**。キャッシュが切れれば開けなくなると思われる(旧 RSS がキャッシュ切れのあとに 404 になるかは、すぐ名前を戻したので未確認)

## エピソードの URL を変える場合(公式の説明)

エピソードの URL は、エピソード編集画面の「詳細設定」で確かめたり変えたりできる。公式のヘルプには次のように書かれている(https://listen.style/document/start_podcast)。

> エピソードURLを変更すると、変更前のURLは利用できなくなります。すでにSNSやWebサイトなどで共有している場合はご注意ください。

番組の URL を変えたときも、これと同じく、変える前の URL は使えなくなった。

## メモ

- 公式のヘルプには、「URL用の名前」は番組を作るときに入れる項目として出てくるだけで、あとから変えられるか、変えたら旧 URL がどうなるかは書かれていない(2026-11-26 に、start_podcast・register_listenpodcast・moving_podcast・publication_range などのページを確認)。上の結果は実機で試したもの
- 名前を変える前に、次のものを洗い出しておくとよい。どれも自分で新しい URL に直す必要がある
  - SNS・ブログ・ウェブサイトに貼った番組やエピソードのリンク
  - Spotify・Apple Podcasts などの配信先に登録した RSS の URL(配信先での直し方はサービスごとに違う。ここは未確認)
  - 自分の RSS を使っているほかのサービス
- 配信先でどうなるか(更新が止まるか、新しい RSS に切りかえられるか)は試していない
- 番組の公開範囲などの設定のしかたは [03-private-podcast.md](03-private-podcast.md)、外部の RSS の番組で取りこみ元の RSS を変える方法(「RSS URLを変更」)は [04-external-podcast.md](04-external-podcast.md)。こちらは LISTEN の番組ページの URL を変えるものではない
