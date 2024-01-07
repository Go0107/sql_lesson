# ステップ3

## 1
よく見られているエピソードを知りたいです。エピソード視聴数トップ3のエピソードタイトルと視聴数を取得してください

```sql
    SELECT episode_title, COUNT(*)
      FROM episode
INNER JOIN views
        ON episode.id = views.episode_id
  GROUP BY views.episode_id
  ORDER BY COUNT(*) DESC
     LIMIT 3;
``` 

## 2
よく見られているエピソードの番組情報やシーズン情報も合わせて知りたいです。エピソード視聴数トップ3の番組タイトル、シーズン数、エピソード数、エピソードタイトル、視聴数を取得してください

:::note warn
テーブルの構造が悪く、番組とシーズンとエピソードの紐づけがうまくいかなかった
:::

```sql

```

## 3
本日の番組表を表示するために、本日、どのチャンネルの、何時から、何の番組が放送されるのかを知りたいです。本日放送される全ての番組に対して、チャンネル名、放送開始時刻(日付+時間)、放送終了時刻、シーズン数、エピソード数、エピソードタイトル、エピソード詳細を取得してください。なお、番組の開始時刻が本日のものを本日方法される番組とみなすものとします

:::note warn
日付の表現がわからなかったので、時刻のみ表示.
テーブルの構造が悪く、番組とシーズンとエピソードの紐づけがうまくいかなかったので、とりあえず番組タイトルと番組詳細を表示
:::

```sql
SELECT c.name, b.start_time, b.end_time, p.title, p.detail
FROM program AS p
INNER JOIN channel AS c ON p.channel_id = c.id
INNER JOIN broadcast_time AS b ON p.broadcast_time_id = b.id
ORDER BY b.start_time ASC;
```