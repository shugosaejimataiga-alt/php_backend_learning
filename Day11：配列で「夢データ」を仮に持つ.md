

今日やったこと（事実）

./vendor/bin/sail up を実行し、
Webサーバー（Nginx）と PHP（Laravel）を起動した

routes/api.php に
Route::get('/dreams', ...) を書いた

ブラウザで
http://localhost/api/dreams にアクセスした

JSON形式の配列データが返ってくることを確認した



今日学習したこと（理解の要点）

1️⃣ APIとは何か

ReactとLaravelの出入り口
APIは「画面」ではなく データを返す窓口
誰が来てもよい（Reactでもブラウザでも同じ）


2️⃣ ルート（Route）とは何か

「このHTTP通信が来たら、この処理を実行する」というルール
ファイルでも場所でもない
HTTPメソッド × パス の組み合わせで決まる

例：

Route::get('/dreams', ...)

＝
「GET かつ /dreams が来たらこの処理」



3️⃣ /api が付く理由

Laravel起動時に、次の設定が読み込まれる。

Route::middleware('api')
    ->prefix('api')
    ->group(base_path('routes/api.php'));


意味：

routes/api.php に書いたルートは

すべて /api が先頭についた状態で登録される

これは 通信時ではなく、起動時の登録処理



⭐ 通信の過程（最重要・文章で完全版）

./vendor/bin/sail up を実行すると、Docker内で Webサーバー（Nginx） と PHP（Laravel） が起動し、HTTPリクエストを受け取れる状態で待機します。
その状態でブラウザから http://localhost/api/dreams にアクセスすると、ブラウザは GET /api/dreams というHTTPリクエストを Nginx に送信します。
Nginxはそのリクエストを受け取り、「これはPHPで処理する通信だ」と判断して、内容をそのまま Laravel に渡します。
Laravelは受け取った HTTPメソッド（GET） と パス（/api/dreams） を使って、起動時に登録しておいたルーティングルールと照合します。
起動時の設定により、routes/api.php に書いた Route::get('/dreams', ...) は内部的に GET /api/dreams として登録されているため、通信とルートが一致します。
その結果、対応する function が実行され、PHPで配列が作られます。
Laravelはその配列を自動的に JSON形式 に変換し、HTTPレスポンスとしてNginxに返します。
Nginxはそのレスポンスをそのままブラウザに返し、ブラウザは受け取ったJSONを画面に表示します。



今日、特に大事だと気づいたこと（疑問→理解）

/api/dreams は
どこかに存在するものではなく、通信時に照合される文字列

prefix('api') は
返すときに付くのではなく、ルート登録時に付いている

ルーティングは
HTTPメソッドとパスの完全一致がすべて