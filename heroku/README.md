# Heroku上でLINE BOTを動かしてみよう

## 必要なファイル
- main.py (メインのソースコード)
- Procfile (heroku上でのプログラムの実行方法を記載)
- requirements.txt (必要なライブラリのバージョンを指定)
- runtime.txt (Pythonのバージョン指定)

## 準備
1. Herokuアカウント登録
    https://signup.heroku.com/login
1. Heroku CLIインストール  
    - macの場合はターミナル上で下記コマンドを実行
      ```sh
      brew tap heroku/brew && brew install heroku
      ```
    - それ以外のOSの場合は公式ドキュメントを参考にインストール  
      https://devcenter.heroku.com/articles/heroku-cli#standalone

## 実装
1. local環境で動作確認済みの`2-returnMessage.ipynb`をPython形式でダウンロードする  
    1. jupyter notebookで`2-returnMessage.ipynb`を開き **File**タブをクリックする
    1. **Download as** から **Python (.py)** を選択し、保存する  
      -> `2-returnMessage.py`がダウンロードされる
1. `2-returnMessage.py` がダウンロードフォルダに保存されているので、それをここのディレクトリに持ってくる
1. `2-returnMessage.py` を `main.py` という名前に変換する
1. 好きなエディターもしくはjupyter notebookで`main.py`を開き末尾の実行コードを下記のように書き換えて保存する

    **before**
    ```py3
    if __name__ == "__main__":
      app.run()
    ```
 
    **after**
    ```py3
    if __name__ == "__main__":
      port = int(os.getenv("PORT", 5000))
      app.run(host="0.0.0.0", port=port)
    ```

1. ターミナル上でHerokuにログインし、新規アプリケーション作成する
    ```sh
    $ heroku login
    $ heroku create heroku-line-bot (好きなアプリケーション名)
    ```
    
1. Heroku環境変数の設定をする
    ```sh
    $ heroku config:set LINE_CHANNEL_SECRET="<チャネルシークレット>"
    $ heroku config:set LINE_CHANNEL_ACCESS_TOKEN="<アクセストークン>"
    ```
 
1. 追加・変更したファイルをコミットしてデプロイする
    ```sh
    $ git init (以降このコマンドは実行不要)
    $ git add .
    $ git commit -m "new commit"
    $ git push heroku master
    ```

1. Webhook登録する
    LINE DevelopersのMessaging API設定画面で、Webhook URLにデプロイしたアプリケーションのURL「https://heroku-line-bot.herokuapp.com/callback 」を登録する。  
    **アプリケーション名を変更したらURLも変わるので要注意**


## ログ調査
- もしうまくいかなかったら、下記コマンドでログを確認できる
    ```sh
    $ heroku logs --tail
    ```

## アプリケーション削除
```sh
$ heroku apps:destroy --app heroku-line-bot --confirm heroku-line-bot
``` 
