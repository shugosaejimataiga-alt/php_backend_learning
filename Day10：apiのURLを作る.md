
🟦 Day10：/api/dreams というURLを作る
→ 夢一覧APIの“形”を体験する



今日の学習目標

API用のURLが1つ作れる

ブラウザでアクセスしてJSONが返るのを確認できる

👉 まだDB・配列・本格処理はやりません。



なぜこの学習が必要か

人生ToDoアプリでは、Reactがこう聞いてきます。

「今保存されている“夢一覧”をください」

それを受け取る窓口が
/api/dreams です。



どこで使われるのか（超重要）

将来こうなります。

React
  ↓  GET /api/dreams
Laravel
  ↓
DB

今日は この入口（URL）だけ を作ります。

ーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーー

🟦 routes/api.php に書いたコードの解説まとめ

今日書いたコード（完成形）

<?php

use Illuminate\Support\Facades\Route;

Route::get('/dreams', function () {
    return [
        "message" => "dream list api",
    ];
});


① <?php

PHPの開始宣言
Laravelは内部的にすべてPHPで動いている
👉 これが無いと PHPとして解釈されない


② use Illuminate\Support\Facades\Route;

何をしているか
Route というクラスを使いますと宣言している

なぜ必要か
PHPでは、使うクラスは 必ず import が必要

書かないと
Route が見つかりません エラーになる

👉 Laravel以前にPHPの基本ルール


③ Route::get('/dreams', function () { ... });

全体の意味（1文）
「GET通信で /dreams に来たら、この処理を実行する」


分解して読む

Route::get

GETメソッド用のルート
「データをください」という通信

👉 夢一覧を取得するのに最適



'/dreams'

URLの中身
api.php 側では /api は書かない

👉 /api は prefix('api') が付けてくれる



function () { ... }

このURLが呼ばれた時に実行される処理
今回はテスト用なので 無名関数



④ return [ "message" => "dream list api" ];


何をしているか

PHPの 配列を返している


なぜJSONになる？

Laravelは
配列を返すと
自動で JSON に変換

👉 return しているのは 画面ではなくデータ



実際に返ってきたもの
{
  "message": "dream list api"
}



**今日のコードの本質（超重要）**

URL

HTTPメソッド（GET）

返すデータ（配列 → JSON）

👉 これが APIの最小単位



1文まとめ（思い出し用）

api.php では
URLと通信方法を決め、処理を書き、データを返す。



今の /dreams は
バックエンドが将来持つ「夢配列」を扱う予定の名前。

ーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーー

🟦 Day10 振り返りまとめ（人生ToDoアプリ｜Laravelバックエンド）

今日やったこと（事実）

/api/dreams というURLを作成
JSONが返るAPIを実際に動かした
routes/api.php を自分で作成
Laravelに api.php を登録して動かした
エラー（Route が見つからない）を import で解決
RouteServiceProvider のコードを読んで理解した



今日学習したこと（核心）

① APIとは何か（体験として理解）

URLにアクセスするとJSONが返る
画面（HTML）ではなく データを返すのがAPI

/api/dreams は
👉「夢一覧をください」という専用のお願い口



② ルーティングとは何か

URLと処理を結びつける仕組み
Laravelではそれを Route が担当する


Route::get('/dreams', function () {
    return [...];
});


=
「このURLが来たら、この処理をする」



🟨 重要：RouteServiceProvider の理解（今日の山場）

RouteServiceProvider の役割

Laravelに「どのルートファイルを、どんなルールで読むか」を教える場所
web用とapi用を 分けて登録している



boot(): void の意味


public function boot(): void


Laravel起動時に 自動で呼ばれる
目的は 設定
値を返す必要がないため void

👉
「初期設定専用の関数」



$this->routes(function () { ... })

Laravelが ルート登録フェーズに入ったときに実行される
自分で呼ぶ関数ではない
中身は ルートの登録ルール



web.php 側の登録

Route::middleware('web')
    ->group(base_path('routes/web.php'));


意味：
web用の最低限のルール（ブラウザ前提）を付けて
routes/web.php に書かれたルートをまとめて読む



api.php 側の登録（最重要）

Route::middleware('api')
    ->prefix('api')
    ->group(base_path('routes/api.php'));


意味を分解すると：

middleware('api')
API用の軽いルール（JSON前提）

prefix('api')
URLの先頭に /api を自動で付ける

group(...)
そのルールをまとめて適用

base_path('routes/api.php')
Laravelプロジェクトのルートを基準にした実ファイルへの道案内

👉
api.php に /dreams と書いても
実際のURLが /api/dreams になる理由がここ。



base_path() の理解

Laravelプロジェクトの一番上（根っこ）を基準にしたパス指定
実行場所に依存しない安全な指定方法



今日つまずいた所・疑問点

api.php を作っただけでは動かなかった
👉 Laravelに登録が必要だった

Route が見つからないエラー
👉 PHPではクラスの import が必要だと再確認



今日の最重要理解（1文）

APIかどうかはファイル名ではなく、「URL＋JSONを返す設計」で決まる



今日の到達点

Laravelの「使い方」ではなく
「仕組み（ルーティング）」を理解した

/api/dreams という
本番につながる入口を自分で作れた



