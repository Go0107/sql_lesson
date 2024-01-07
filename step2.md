# データベースの構築

```sql
CREATE DATABASE internet_tv;
```
## データベースへの移動

```sql
USE internet_tv;
```

# テーブルの構築

```sql
CREATE TABLE channel
(id           BIGINT(20)   AUTO_INCREMENT,
 name         VARCHAR(100) NOT NULL,
 PRIMARY KEY (id));

CREATE TABLE genre
(id         BIGINT(20)   AUTO_INCREMENT,
 name       VARCHAR(100) NOT NULL,
 PRIMARY KEY (id));

CREATE TABLE broadcast_time
(id         BIGINT(20) AUTO_INCREMENT,
 start_time TIME       NOT NULL,
 end_time   TIME       NOT NULL,
 PRIMARY KEY (id));

CREATE TABLE program
(id                BIGINT(20)   AUTO_INCREMENT,
 title             VARCHAR(100) NOT NULL,
 detail            VARCHAR(100) ,
 genre_id          BIGINT(20)   NOT NULL,
 channel_id        BIGINT(20)   NOT NULL,
 release_day       DATE         ,
 broadcast_time_id BIGINT(20)   ,
 FOREIGN KEY (genre_id) REFERENCES genre(id),
 FOREIGN KEY (channel_id) REFERENCES channel(id),
 FOREIGN KEY (broadcast_time_id) REFERENCES broadcast_time(id),
 PRIMARY KEY (id));

CREATE TABLE season
(id              BIGINT(20) AUTO_INCREMENT,
 program_id      BIGINT(20) NOT NULL,
 season_number   BIGINT(20) NOT NULL,
 FOREIGN KEY (program_id) REFERENCES program(id),
 PRIMARY KEY (id));

CREATE TABLE episode
(id             BIGINT(20)   AUTO_INCREMENT,
 season_id      BIGINT(20)   NOT NULL,
 episode_number BIGINT(20)   NOT NULL,
 detail         VARCHAR(200) ,
 release_day    DATE         ,
 FOREIGN KEY (season_id) REFERENCES season(id),
 PRIMARY KEY (id));

CREATE TABLE user
(id    BIGINT(20)   AUTO_INCREMENT,
 name  VARCHAR(100) NOT NULL,
 email VARCHAR(100) NOT NULL,
 PRIMARY KEY (id));

CREATE TABLE views
(id         BIGINT(20) AUTO_INCREMENT,
 user_id    BIGINT(20) NOT NULL,
 program_id BIGINT(20) ,
 episode_id BIGINT(20) ,
 FOREIGN KEY (user_id) REFERENCES user(id),
 FOREIGN KEY (program_id) REFERENCES program(id),
 FOREIGN KEY (episode_id) REFERENCES episode(id),
 PRIMARY KEY (id));
```
# サンプルデータの挿入

```sql

-- チャンネル
INSERT INTO channel (name) VALUES
('Channel A'),
('Channel B'),
('Channel C'),
('Channel D'),
('Channel E');

-- ジャンル
INSERT INTO genre (name) VALUES
('Drama'),
('Comedy'),
('Documentary'),
('Science Fiction'),
('Reality Show');

-- 放送時間
INSERT INTO broadcast_time (start_time, end_time) VALUES
('18:00:00', '19:00:00'),
('20:30:00', '21:30:00'),
('22:00:00', '23:00:00'),
('15:00:00', '16:00:00'),
('19:30:00', '20:30:00');

-- プログラム
INSERT INTO program (title, detail, genre_id, channel_id, release_day, broadcast_time_id) VALUES
('Program 1', 'Exciting drama', 1, 1, '2024-01-10', 1),
('Program 2', 'Hilarious comedy', 2, 2, '2024-01-12', 2),
('Program 3', 'Informative documentary', 3, 3, '2024-01-15', 3),
('Program 4', 'Adventurous sci-fi series', 4, 4, '2024-01-20', 4),
('Program 5', 'Reality show with surprises', 5, 5, '2024-01-22', 5);

-- シーズン
INSERT INTO season (program_id, season_number) VALUES
(1, 1),
(1, 2),
(2, 1),
(3, 1),
(4, 1);

-- エピソード
INSERT INTO episode (season_id, episode_number, detail, release_day) VALUES
(1, 1, 'Episode 1 of Season 1', '2024-01-10'),
(1, 2, 'Episode 2 of Season 1', '2024-01-17'),
(2, 1, 'First episode of the series', '2024-01-12'),
(3, 1, 'Introduction to the documentary', '2024-01-15'),
(4, 1, 'Exciting pilot episode', '2024-01-20');

-- ユーザー
INSERT INTO user (name, email) VALUES
('John Doe', 'john.doe@example.com'),
('Jane Smith', 'jane.smith@example.com'),
('Bob Johnson', 'bob.johnson@example.com'),
('Alice Williams', 'alice.williams@example.com'),
('Charlie Brown', 'charlie.brown@example.com');

-- 視聴履歴
INSERT INTO views (user_id, program_id, episode_id) VALUES
(1, 1, 1),
(2, 2, NULL),
(3, 1, 2),
(4, 3, NULL),
(5, 4, 1);

```

## episode_titleを入れ忘れたので追加

```sql
-- episodeテーブルに新しい列 episode_title を追加
ALTER TABLE episode ADD COLUMN episode_title VARCHAR(100) NOT NULL;

-- episodeテーブルのサンプルデータ追加
UPDATE episode SET episode_title = 'Exciting Episode 1' WHERE id = 1;
UPDATE episode SET episode_title = 'Thrilling Episode 2' WHERE id = 2;
UPDATE episode SET episode_title = 'Funny First Episode' WHERE id = 3;
UPDATE episode SET episode_title = 'Introduction Episode' WHERE id = 4;
UPDATE episode SET episode_title = 'Pilot Adventure' WHERE id = 5;

```