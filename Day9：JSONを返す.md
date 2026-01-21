

🟦 Day9｜JSONを返す（APIの感覚をつかむ）


今日やったこと

今日は Laravelで「画面ではなくデータを返す」体験 をした。
routes/web.php にルートを書き、ブラウザからアクセスして JSONが返ること を実際に確認した。


Route::get('/', function () {
    return [
        'ok' => true,
        'message' => 'day9 JSON ok',
    ];
});


ブラウザでは
{"ok":true,"message":"day9 JSON ok"}
と表示され、LaravelがJSONを返している ことを確認できた。



学習したこと（重要な理解）

APIとは「URLにアクセスするとデータ（JSON）が返る仕組み」

APIかどうかは
api.php かどうか / 書き方 ではなく、
「画面ではなくJSONを返しているか」 で決まる

PHPの 配列 を return すると、
Laravelが 自動でJSONに変換 してくれる

response()->json() は
API設計・本番向けの書き方 であり、Day9では必須ではない

Day9は APIを設計する日ではなく、APIを体験する日



なぜ web.php を使ったのか（設計の意図）

Day9の目的は 「APIの感覚をつかむ」こと

余計な前提（/api、設計、ルール）を増やさず
最短で「JSONが返る」体験に集中するため

api.php は Day10以降の本番ステップ で使う

👉 ロードマップどおりの正解ルートは web.php



今日の核心（1行）

「URLにアクセスしたら、画面ではなく“意味を持つデータ（JSON）”が返る。これがAPI」



今日出てきた疑問とその答え

Q. 配列とJSONは違う？
→ 違うが、Laravelが 配列 → JSON を自動でやってくれる

Q. response()->json() を使わないとAPIじゃない？
→ いいえ。返ってくるものがJSONならAPI

Q. api.php が無いのはおかしい？
→ おかしくない。最小構成のLaravelでは最初から無いことがある

Q. ロードマップどおりはどっち？
→ Day9は web.php、Day10で api.php



今日の到達点

あなたはすでに：

Laravelが動く

URLにアクセスできる

JSONを返せる

APIの最小形を理解している

という状態です。