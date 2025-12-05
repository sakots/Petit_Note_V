# お絵かき掲示板PHPスクリプト Petit Note V

このスクリプトはさとぴあさんの[Petit_Note](https://github.com/satopian/Petit_Note)をフォークしたものです。

![php](https://img.shields.io/badge/php-7.4-green.svg)
![php](https://img.shields.io/badge/php-8.x-green.svg)

![Last commit](https://img.shields.io/github/last-commit/sakots/Petit_Note_V)
![version](https://img.shields.io/github/v/release/sakots/Petit_Note_V)
![Downloads](https://img.shields.io/github/downloads/sakots/Petit_Note_V/total)
![License](https://img.shields.io/github/license/sakots/Petit_Note_V)

- PaintBBS NEO,tegaki.js,ChickenPaint,Klecksが使えるお絵かき掲示板です。
- ログ形式をsqliteにしました。
- テンプレートをVuejsにしました。

## 動作環境

- PHP7.3以上の環境が必要です。
- PHP7.4,PHP8.4,PHP8.5で動作確認しています。
- PHP8.1-PHP8.5での使用を推奨します。

## Petit Note Vを使った交流サイト

- [お絵かき掲示板交流サイト Petit Note](https://paintbbs.sakura.ne.jp/)

## DEMO

- [Petit Note サンプル掲示板](https://paintbbs.sakura.ne.jp/cgi/neosample/petitnote/)

## ダウンロードと設置

### ダウンロード

- [リリース](https://github.com/sakots/Petit_Note_V/releases/latest)から安定版をダウンロードできます。

### 設置方法

1. 設置するサーバのPHPのバージョンが7.3以上になっている事を確認します。
2. [リリース](https://github.com/sakots/Petit_Note_V/releases/latest)のページの一番下からzipファイルをダウンロードします。
3. `petitnote`フォルダ内の`config.example.php`のコピーを作り、`config.php`に名前を変更します。
4. `petitnote`フォルダ内の`config.php`の管理者パスワードを他の人にはわからないパスワードに変更します。
5. `petitnote`フォルダをアップロードします。
6. サーバ上の`petitnote`ディレクトリにブラウザでアクセスすると設置が完了します。

## 設置しても動作しない場合

パーミッションを手動で設定すると動作しなくなる事があります。

- 設置時にディレクトリやファイルのパーミッションを変更すると正常に動作しない事があります。
- 必要なディレクトリの作成･パーミッションの設定はPHPスクリプトが自動的に行います。

PHPのバージョンがPHP7.3未満の時は500エラーになります。

- PHPのバージョンが7.3未満の時は500エラーになります。
- PHPのバージョンが切り替え可能な場合はPHP7.3以上への変更をお願いします。
- このスクリプトの動作環境はPHP7.3-PHP8.xです。推奨はPHP8.1以上です。

template/ ディレクトリのcssが表示されない時は。

- 設置したサーバによっては、同梱している.htaccessが原因でエラーになる事があります。
- その場合は`petitnote/` ディレクトリ、`petitnote/template/`ディレクトリにある`.htaccess`を削除すると動作するようになります。
- ただし、問題なく動作している場合は削除してはいけません。このファイルはセキュリティリスクを低減させるためのものです。

## それでも動作しない場合は

[設置サポート掲示板](https://paintbbs.sakura.ne.jp/cgi/neosample/support/)をご利用ください。

![image](https://user-images.githubusercontent.com/44894014/134553433-d50e05be-a483-4b94-a575-3cead96b6720.png)

## 履歴

[Petit_note(さとぴあさん)の履歴](./changelog.md)

### 2025/12/05

- PetitNote 1.158.3.1からフォーク
