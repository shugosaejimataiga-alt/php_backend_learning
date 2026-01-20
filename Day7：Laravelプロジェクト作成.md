📘 Day7 完全振り返りまとめ

Laravel（Docker + WSL）環境構築 編



1️⃣ 今日のゴール（最初に決めていたこと）

Laravel を Docker環境で動かす

ブラウザで Laravel Welcome画面を見る

「自分が何を操作しているか」を 概念レベルで理解する

👉 すべて達成済み



2️⃣ 今日あなたが実際にやった行動（時系列）

① Docker + Linux の前提理解

あなたの質問：
Docker Desktop用Linuxと
人が作業するLinux（Ubuntu / WSL）は何が違うのか？

結論：
Docker Desktop用Linux
→ Dockerが裏で使う・触らない

Ubuntu / WSL
→ あなたが操作するLinux

👉 ターミナルで触っているのは Ubuntu / WSL


② WSL（Ubuntu）が入っていないことに気づいた

実行：

wsl -l -v


表示：

docker-desktop  Running


意味：

Ubuntuが まだ存在していなかった
Docker用Linuxしかなかった


③ Ubuntu（WSL）をインストール

実行：

wsl --install -d Ubuntu


意味：

人が作業するLinux環境を追加


途中でやったこと：

ユーザー名作成
パスワード設定


④ 「Linuxはどこにあるの？」問題の解決

あなたの疑問：

Linuxってフォルダなの？
エクスプローラーの Linux は触らない？
じゃあどこで触るの？

答え：

Linuxは「フォルダ」ではなく OS（環境）
ターミナルで操作する
エクスプローラーの Linux は 内部領域（触らない）



3️⃣ 作業場所の決定

あなたの判断：

C:\Users は散らかっている
C:\直下に開発用フォルダを作りたい

👉 実務的に正解

実行したコマンド
cd /mnt/c
mkdir dev
cd dev


意味：

/mnt/c = WindowsのCドライブ

dev = 開発用フォルダ

C:\dev が作られた



4️⃣ Laravel（Docker対応）プロジェクト作成
実行したコマンド
curl -s https://laravel.build/life-todo-api | bash


意味を分解すると：

curl
→ インターネットからデータ取得

https://laravel.build/...
→ Laravel公式の「Docker付き雛形生成」

| bash
→ 取得した内容を コマンドとして実行


結果：

life-todo-api フォルダ作成

Laravel + Docker + Sail が一式生成



5️⃣ Dockerが起動していなかった問題

表示：

Docker is not running.


意味：

Docker Desktop（Windowsアプリ）が起動していない


対応：

Docker Desktop を起動

それでもダメ → WSL連携の問題と判明


解決策（重要）

実行：

wsl --shutdown


意味：

WSLを完全停止 → 再起動

DockerとWSLの接続をリセット

👉 これで接続成功



6️⃣ Laravelプロジェクトへ移動 & 起動

cd life-todo-api
./vendor/bin/sail up


意味：

sail = Laravel専用Docker操作ツール

up = コンテナ起動


表示された英語ログの意味

例：

exporting layers
→ Dockerが完成品を組み立て中

status_code=200
→ サービス正常稼働

👉 長くても正常



7️⃣ ブラウザでエラーが出た理由

表示されたエラー（要点）：

table 'laravel.sessions' doesn't exist


意味：

DBは動いている
でも テーブルがまだ作られていない

👉 初期セットアップ不足なだけ



8️⃣ DB初期化（migrate）

新しいUbuntuをもう1つ開いて実行

./vendor/bin/sail artisan migrate


意味：

artisan = Laravel管理コマンド

migrate = DBテーブル作成


Laravelでは、次の流れになっています。

1,database/migrations フォルダに
「こういうテーブルを作れ」という設計図（PHPファイル）がある
2,artisan migrate を実行
3,Laravelがその設計図を読み取る
4,MySQLに対して CREATE TABLE ... を自動で実行
5,テーブルが実際に作成される


結果：

create_users_table DONE
create_cache_table DONE
create_jobs_table DONE

👉 DB完成



9️⃣ Laravel Welcome画面表示

ブラウザ：

http://localhost


表示：

Let's get started
Laravel has an incredibly rich ecosystem

👉 Day7 完全成功



🔟 正しい終了方法

実行：

Ctrl + C


表示されたログ（SIGTERMなど）：

すべて正常
データ保存済み
きれいなシャットダウン



🔁 次回の再開方法（メモ）

cd /mnt/c/dev/life-todo-api
./vendor/bin/sail up

→ ブラウザで http://localhost



🧠 今日得た「本質的理解」

Linuxは「場所」ではなく「環境」
あなたは Ubuntu（WSL）で操作
Docker用Linuxは 裏方
Laravelは
React → API → DB の土台になる


ーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーーー

🧠 Windows → Linux（WSL・Docker）の構造まとめ【完全版】

**まず大前提（ここが一番大事）**

あなたのパソコンは常に Windows が主役です。
Linux は Windows の「中」で動いています。

ただし Linux は1つではなく、役割の違う2種類があります。



**全体構造（感覚的な階層）**

イメージとしては、こう理解するとズレません。

一番外側：Windows（あなたのPC）

その上で同時に動いているものが2つある

あなたが操作する Linux
Docker が裏で使う Linux

重要なのは、
「Linuxの中にLinuxがある」のではなく、
Windowsの上でLinuxが“並列”に動いている
という点です。



1.あなたが触る Linux
👉 Ubuntu / WSL

これは あなた専用の作業環境です。

どうやって手に入れたか

wsl --install -d Ubuntu

つまり 自分でダウンロード・インストールした


どこで触るか

ターミナル（Ubuntuの黒い画面）

ここで：

cd=移動するコマンド

mkdir=フォルダを作るコマンド

curl=インターネットからデータを取得するコマンド

./vendor/bin/sail = laravel専用の起動コマンド

などを打った


重要な理解

Ubuntu（WSL）は フォルダではない

OS（環境）

だから：

エクスプローラーで「Ubuntuの場所」を探す必要はない
触る場所は ターミナルのみ



2.触らなくていい Linux
👉 Docker Desktop 用 Linux（docker-desktop）

これは Docker専用の裏方Linuxです。


どうやって手に入ったか

Docker Desktop をインストールした瞬間に自動で作られた


自分で：

ダウンロードしていない
インストール操作もしていない


つまり：

Docker Desktopを入れたら、勝手についてきた


何をしているか

コンテナ（Laravel / MySQL / Redis など）を動かす
ネットワーク管理
ボリューム管理


なぜ触らないのか

人が作業する前提ではない
必要最低限の構成
触ると壊れる可能性がある


あなたが間違って入ったことがある：

docker-desktop:/tmp/...

👉 ここは作業場ではない



**「表」「裏」という感覚で言うなら**

表（あなたが見る・触る）

Windows

Ubuntu / WSL

ターミナル

Cドライブ（/mnt/c）


裏（勝手に動く・触らない）

Docker Desktop用Linux

Docker内部のネットワーク

各コンテナの中身

👉 あなたは「表」だけ理解して操作すればいい



**Windows と Linux の「ファイルの関係」**

Windows側

C:\dev\life-todo-api

エクスプローラーで見える

VS Code で編集できる


Linux（Ubuntu）側

同じ場所を：

/mnt/c/dev/life-todo-api

として見ている


つまり：

Ubuntuは、WindowsのCドライブを
外付けディスクのように扱っている


だから：

Linux上で作ったファイルが
Windowsでも見える


**重要な誤解ポイントの整理**

❌ 間違った考え

Linuxはフォルダである

エクスプローラーの「Linux」を触る

Docker用LinuxとUbuntuを行き来している


⭕ 正しい理解

Linuxは 環境（OS）

触るのは Ubuntuのターミナル

Docker用Linuxは 完全に裏方



**今日のあなたの最終理解を一文で言うと**

Windowsの上で
Ubuntu（WSL）と Docker用Linux が並列で動いていて、
自分は Ubuntu のターミナルから操作し、
Docker用Linuxは裏で勝手に動いている。

これは 実務レベルで完全に正しい理解です。