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

Note: Cloud9のサポート言語がPython3になっていることを確認

### GitHub
#### リポジトリ作成
画面右上で「New repository」をクリック
→「Repository」を入力
→練習用なので「Private」を選択
→「Create repository」をクリック

### 秘密鍵作成
#### 演習/事前準備
```
cd /home/ec2-user/environment　※初期状態であればcd不要
ssh-keygen
cat ~/.ssh/id_rsa.pub
```
→Github＞Settings＞New SSH key
→マシン名等が想像しやすいTitleをつけて、Keyに先程のid_rsa.pubの内容をコピー

#### 接続確認
```ssh git@github.com```

### 接続設定
```
git config --global user.name "GitHubのユーザ名"
git config --global user.email "GitHubに登録したメアド"
```

接続先を変えたい場合は```--local```を使用
```
git config --local user.name "GitHubのユーザ名"
git config --local user.email "GitHubに登録したメアド"
```

やり直したい場合は各キーに```--unset```をつけると削除することができる
```
git config --unset user.email
```

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

**データベース**
Sqliteの場合は`db.sqlite3`が直下に作成される

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

## ビュー
ビューはHttpResponse オブジェクト、もしくは Http404 のような例外を返す  
（テンプレートをロードしてコンテキストに値を入れ、テンプレートをレンダリングした結果を HttpResponse オブジェクトで返す）

### 404エラー
モデルとビューのカップリングを回避するため、エラー送出機能では`get_object_or_404()`もしくは`get_list_or_404()`を使用する

## POSTフォーム
### リクエストの値取得
`request.POST['xxxx']`で、リクエストのパラメーターの値を文字列で取得
キーが存在しない場合は`KeyError`が発生

### リダイレクト
- `HttpResponse`ではなく`HttpResponseRedirect`を使用する
- リダイレクト先のURLはハードコードせず`reverse()関数`を使って定義する

### CSRF対策
POST フォームでは {% csrf_token %} テンプレートタグを使用する

## ListView / DetailView
「オブジェクトのリストを表示する」および「あるタイプのオブジェクトの詳細ページを表示する」という二つの概念を抽象化することができる

**デフォルトで使用されるテンプレート**
- DetailView 汎用ビューは`<app name>/<model name>_detail.html`という名前のテンプレート  
- ListView 汎用ビューは`<app name>/<model name>_list.html`という名前のテンプレート
他のテンプレートへレンダリングする場合は`template_name`を指定

## テストコード
アプリケーション直下の`tests.py`に記述

実行コマンド
`python manage.py test polls`

### デバッグ方法
```
$ python manage.py shell

In [1]: from polls.models import Question
In [2]: Question.objects.get(id=1).question_text
Out[2]: "What's up?"
In [3]: exit
```

```
$ python manage.py shell

In [1]: from django.test import Client
In [2]: client = Client()
In [3]: response = client.get('/')
Not Found: /
In [4]: response.status_code
404

In [5]: from django.urls import reverse
In [6]: response = client.get(reverse('polls:index'))
In [7]: response.status_code
200
In [8]: response.context['latest_question_list']
<QuerySet [<Question: What's up?>]>
```

## アサーションメソッド
値のチェックを行いたい場合 
- assertContains()
- assertQuerysetEqual()

結果が空であることを確認する例
`self.assertQuerysetEqual(response.context['column_name'], [])`


## スタイルの適用
アプリケーションの直下に`static`フォルダを作成し、その中に静的ファイルを格納する

格納する際、アプリケーション名でサブディレクトリを作成しなくてはならない
Djangoは名前がマッチした最初のテンプレートを使用するので、異なるアプリケーションで同じ名前のテンプレートが使用される場合に対応するため、名前空間を分けることで区別する


## Adminのカスタマイズ
adminをカスタマイズしたい場合はモデルごとにadminクラスを作成してパラメーターとして渡す

### Djangoの実行ファイルの場所
```
python -c "import django; print(django.__path__)"
```

ここに置いてある`django/contrib/admin/templates/admin/base_site.html`をコピーして利用

### 補足
タイトルを変えるだけならテンプレートをコピーしなくても`django.contrib.admin.AdminSite.site_header属性`を利用すれば変更可能

## 多言語機能(i18n)の修正方法
1. `setting.py`に`LOCALE_PATHS`を追加
2. ビルトインアプリのロケールをアプリケーション直下にコピー（adminの場合は'contrib/admin/locale'）
3. コピーした`.po`ファイルを編集
4. `python manage.py compilemessages -l ja`を実行