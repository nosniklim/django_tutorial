# Django開発入門
https://docs.djangoproject.com/ja/2.2/intro/tutorial01/

## Djangoのインストール確認
`python -m django --version`

## プロジェクト作成
startprojectでプロジェクト作成
`django-admin startproject mysite`

ディレクトリ構成の詳細はチュートリアル参照

## .gitignore追加
djangoってデフォルトで.gitignoreが作成されないので自分で用意
https://zenn.dev/wtkn25/articles/django-gitignore
```
*.log
*.pyc
__pycache__/
db-volumes/
db.sqlite3
```

Note: Cloud9のサポート言語がPython3になっていることを要確認

## 開発用のサーバー起動
`python manage.py runserver`

runserver はデフォルトでは8000ポートで起動するのでCloud9の場合は8080を指定
`python manage.py runserver 8080`

ここまででCloud9でアプリケーションが起動するところまでを確認！！！


## アプリケーション追加
アプリケーションは Python path のどこにでも設置可能

mysite のサブモジュールではなく manage.py のすぐ隣に作る場合
`python manage.py startapp polls`

## URLconf
ルーティングを設定するため polls ディレクトリに urls.py というファイルを追加したら、
ルートのURLconfに polls.urls モジュールの記述を反映

include() 関数は他の URLconf への参照するURLプラグ＆プレイ

## データベース設定
### 設定ファイル
`mysite/settings.py`

migrate コマンドで  mysite/settings.py の INSTALLED_APPS が利用できるようになる
（不要な場合は INSTALLED_APPS を削除してからmigrate）
`python manage.py migrate`

### モデル作成
`django.db.models.Model`のサブクラス
各フィールドは Field クラスのインスタンスとして表現される

Field インスタンスには機械可読なフィールド名と人間可読なフィールド名がある

### モデルを有効化
アプリケーションをプロジェクトに含めるには、構成クラスへの参照を INSTALLED_APPS 設定に追加する必要がある

### モデルの変更からmigrationファイル作成
`python manage.py makemigrations polls`

**マイグレーション**
`python manage.py migrate`

sqlmigrate コマンドで実行されるSQLを確認することができる
`python manage.py sqlmigrate polls 0001`

## 対話シェル
`python manage.py shell`

manage.py を使用することで DJANGO_SETTINGS_MODULE 環境変数を設定
（Django に mysite/settings.py ファイルへの import パスが与えられる）

## Django Admin
### 管理ユーザーを作成
`python manage.py createsuperuser`

とりあえず適当に設定したら runserver して`http://127.0.0.1:8000/admin/`へアクセス
```
admin
admin@example.com
adminadmin
```

## Djangoのルーティング
1. `ROOT_URLCONF`に指定されている Python モジュール`mysite.urls`をロード
2. urlpatterns という変数を順番にパターンを検査
3. `polls/` にマッチした箇所を見つけた後、("polls/") を取り除く
4. 残りの文字列 "34/" を次の処理のために`polls.urls`の URLconf に渡す
5. '<int：question_id>/'でdetail() が呼び出される

山括弧を使用すると、URLの一部が「キャプチャ」され、キーワード引数としてビュー関数に送信される

