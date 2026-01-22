

Day12｜今日やったこと（事実）

Laravelで GET /api/dreams というURLを作った
ブラウザでURLを直接開き、JSONが返ることを確認した
DBは使わず、配列（$dreams）をデータ源として扱った
Reactはまだ書かず、API単体で動くかを確認した



今日学習したこと（理解）

① APIとは何か

URLにアクセスするとJSONを返す仕組み
HTMLを返したら画面、JSONを返したらAPI


② return $dreams; が動く理由

Laravelは 配列が返されたら自動でJSONに変換する
そのため response()->json() を書かなくても表示される
ただし 実務では明示するのが普通


③ バックエンドとReactの切り分け方

URL直アクセスでダメ → Laravel
URL直アクセスでOK → React
これが最初にやる障害切り分け


④ 今作ったものはAPIか？

YES

JSONを返すURLがあればAPI

DB・Controllerは必須ではない



今日出た疑問と答え

Q. React側かLaravel側か、どう判断する？

👉 URLを直接開いて判断する


Q. return response()->json() を書かなくてもいいの？

👉 Laravelが自動でやってくれている


Q. これはAPI実装と言える？

👉 最低限のAPIとして成立している



今日の核心（1行）

「APIは単体で正しく動く部品。まずURL直で確かめる」



次につながる理解

今日作った /api/dreams は
次に POSTで夢を追加する土台になる

DBに変わっても 構造は一切変わらない