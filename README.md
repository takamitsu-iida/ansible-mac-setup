# Macを新規セットアップする

制御ノードから別のmacを設定するためのものなので、自分自身に対して実行するときは慎重に。
特にpython周りを変更すると実行中のansibleに影響が出てしまうので要注意。

## 新規マシンをSSHでログインできるようにする

1. システム環境設定
1. 共有
1. リモートログイン　をチェック

## アップルストアにGUIでサインインしておく

ログイン済みにしておかないとmas-cliが動かないため。

## Xcodeの使用許諾

XcodeをAppleStoreからダウンロードしてインストールする。
一度起動して、使用許諾に同意しておく。

## command line tools for xcode

Xcodeを起動してコマンドラインツールをインストールする。

```bash
Xcode->Open Developer Tool->More Developer Tools
```

ブラウザで使用許諾に同意してダウンロードしてインストールする。

## Xcodeのヘッダファイルのインストール(10.14のみ？)

Xcode10からは標準の場所にしかヘッダファイルを置いてくれないので、手動でヘッダファイルをインストールする。
これは一度だけやればよいので、先に済ませておく。
パスはバージョンによって変更すること。

```bash
sudo installer -pkg /Library/Developer/CommandLineTools/Packages/macOS_SDK_headers_for_macOS_10.14.pkg -target /
```

## Homebrewのインストール

homebrewをansibleでインストールするのは大変なので、手動でインストールする。
homebrewはインタラクティブにインストールするのが基本なので、無人インストールは困難。

公式サイトにある通り、コマンドをコピペしてインストールする。

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

## Google日本語入力

brew caskでもインストールできるようだが、Google日本語入力も手動でインストールした方がよい。

## コピー元macの設定の読み出し

```bash
defaults read > /var/tmp/defaults.txt
```

たとえばドックの設定はこう。ドックを左に置いているのでorientation = leftになっていることがわかる。

```text
    "com.apple.dock" =     {
        autohide = 0;
        "last-messagetrace-stamp" = "571630131.362445";
        loc = "ja_JP";
        magnification = 0;
        "mod-count" = 468;
        orientation = left;
```

## mas-cli

```bash
brew install mas
```

mas-cliを使うとアップルストアからコマンドでインストールできるようになる。
事前にアプリIDを調べる必要がある。

```bash
mas search Xcode
```

実行例。XcodeのIDは497799835であることがわかる。

```bash
iidamac2:/ iida$ mas search Xcode
   497799835  Xcode                                                                (10.1)
  1163893338  App School for Xcode and  iOS 10 Development Free                    (1.0)
  1183412116  Swiftify for Xcode                                                   (4.6)
  1179007212  Code School for Xcode Free -Learn How to Make Apps                   (1.1.3)
  1083165894  Course for Xcode 7 Lite                                              (1.0)
  1168397789  Alignment for Xcode                                                  (1.1.2)
```

## Visual Studio Codeの拡張機能のインストール

インストールされている拡張機能の一覧はコマンドで知ることができる。

```bash
/Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin/code --list-extensions
```

拡張機能のインストールもコマンドで行う。

```bash
/Applications/Visual\ Studio\ Code.app/Contents/Resources/app/bin/code --install-extension "{{ item }}"
```

## nodebrew

nodebrewはhomebrewで導入する。

現在の環境に何がインストールされているかは以下のコマンドで知ることができる。

```bash
npm ls -g --depth=0
```

ansibleにnpmモジュールがあるのでそれを使えばいいだけ。
