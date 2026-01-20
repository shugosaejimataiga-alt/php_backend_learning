🟦 Day5 振り返りまとめ（人生ToDoアプリ／バックエンド理解）
今日やったことの結論（最重要）

同じことを、立場ごとに「3つの言葉」で言っているだけだと理解した。

やっている処理は常に同じで、
「追加・取得・更新・削除」しかしていない。

それを

DB

バックエンド設計

API
で 別の表現で書いているだけ。



① データベースの言葉（SQL）

これは DBの中で直接行われる命令。

追加：INSERT

取得：SELECT

更新：UPDATE

削除：DELETE

👉 データベースに対する純粋な操作名
👉 人間向けではなく、DB向けの命令



② バックエンドの考え方（CRUD）

これは 設計のための言葉。

Create（作る） → INSERT

Read（読む） → SELECT

Update（直す） → UPDATE

Delete（消す） → DELETE

👉 「このアプリは何ができればいいか？」を整理するための概念
👉 CRUD = 機能一覧表

人生ToDoアプリは結局、
夢と行動のCRUDをするだけのシステムだと分かった。



③ APIの形（URL + HTTPメソッド）

これは Reactが使うお願い口。

GET /api/dreams → Read

POST /api/dreams → Create

PUT /api/dreams/{id} → Update

DELETE /api/dreams/{id} → Delete

👉 Reactは「DB」も「SQL」も知らない
👉 URL + メソッドでお願いするだけ



重要な対応関係（一本の線で理解）
React（お願い）
 ↓
API（窓口）
 ↓
Laravel（処理）
 ↓
DB（SQL）


例：

GET /api/dreams
 → Laravelが受け取る
 → SELECT * FROM dreams
 → 結果をJSONで返す

👉 全部つながっている
👉 別々の技術ではない



Laravel → React の正体（最後のピース）

Laravelは 画面を返さない。

昔：サーバーがHTMLを返す

今回：サーバーは データだけ返す

👉 Laravel → React は
JSONという形で「データ」を返すだけ

Reactはそのデータを使って

カードを並べる
色を変える
チェックを付ける

つまり
画面はReactが作る。



JSONとは何か
JavaScriptがそのまま扱えるデータ形式。

例：

[
  { "id": 1, "title": "海外で働く", "done": false },
  { "id": 2, "title": "家族を支える", "done": true }
]

Laravelは
❌ 指示しない
⭕ データを返すだけ



今日コードを書かなかった理由

Day5は 理解の日。

Laravelの書き方を覚える → ❌
SQLを書く → ❌
何を作るアプリかを言語化する → ⭕

ここを飛ばすと、

なぜPOSTなのか分からない
なぜこのURLが必要なのか分からない
UPDATEをいつ使うのか分からない

👉 暗記地獄になる



今日の最大の理解

1つの操作を
React → API → Laravel → DB
立場ごとに違う言葉で書いているだけ

この理解ができたので、
Day6以降は「作業」になる。



自分の到達確認

CRUDは別々の技術ではない
JSONは画面ではなくデータ
Laravelは保存と返却だけ
Reactが画面を作る