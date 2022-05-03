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

