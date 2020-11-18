# Chromebookでkaggle環境を構築する(ARM編)

## 概要

ARM系のChromebook(Lenovo S330)で、Linux環境にJupyter NotebookとPyTorchを導入し、作成したcsvファイルをKaggleに送信するまでの環境を構築しました。その作業手順を解説します。

## コンセプト

- すべてローカル環境に構築する
- pip以外のライブラリはユーザーエリアに導入する
- 時間がかかるのでスリープを解除して導入作業を放置する
- 書籍『Kaggleスタートブック』のサンプルコードの動作を目標とする

## ハードウェア

今回使用するChromebookは次の機種です。

    機種名：Lenovo Chromebook S330
    モニタ：14.0型フルHD液晶(TN)
    CPU：MediaTek MT8173C
    メモリ：4GB
    ストレージ：64GB(eMMC)

## Linux環境の導入

Linux環境は次の作業でインストールします。

    1. 画面右下で時間表示をクリックする
    2. [設定]をクリックする
    3. [Linux（beta）]をクリックし、[有効にする]をクリックする
    4. 画面の指示に従って設定を進める
    5. 最後にターミナルウィンドウが開くので、Linuxコマンドを入力して動作確認する

## Linuxのバージョン

執筆時点(2020/11/18)での「cat /etc/os-release」の実行結果は次の通りです。

    NAME="Debian GNU/Linux"
    VERSION_ID="10"
    VERSION="10 (buster)"
    VERSION_CODENAME=buster
    ID=debian
    HOME_URL="https://www.debian.org/"
    SUPPORT_URL="https://www.debian.org/support"
    BUG_REPORT_URL="https://bugs.debian.org/"REPORT_URL="https://bugs.debian.org/"

## 準備作業

- スリープ設定を解除
  - 設定→デバイス→電源で充電時とバッテリー駆動時で「画面をオフにする」を設定
- Linuxの有効化
  - 設定→Linux（ベータ版）で「オンにする」をクリック
  - ターミナルを起動して初期設定する

## パッケージの更新

    $ sudo apt-get update
    $ sudo apt-get upgrade -y
    $ sudo apt-get dist-upgrade
    $ sudo apt-get autoclean

## PIP3の導入

    $ sudo apt install -y python3-pip

## ビルド関連の依存ライブラリを導入

    $ sudo apt install liblapack-dev gfortran zlib1g-dev libjpeg-dev
    $ sudo apt install libffi-dev cmake llvm llvm-dev

## pip3で基本ライブラリを導入

    $ pip3 install numpy scipy pandas matplotlib scikit-learn
    $ pip3 install jupyter seaborn optuna lightgbm
    $ pip3 install llvmlite==0.30.0
    $ pip3 install numba==0.39.0
    $ pip3 install pandas_profiling==2.9.0
    
## Kaggle API ツールの導入

    $ pip3 install kaggle
    - KaggleのAccountで「Create New API Token」をクリックしてAPI Tokenをダウンロードする
    - ~/.kaggleにkaggle.jsonをコピーしてパーミッションを600に設定

## Kaggle API でCSVファイルをアップロードする

    $ kaggle competitions submit -c titanic -f ファイル名 -m コメント

## PyTorchの導入

- aptでビルド関連の依存ライブラリを導入
    $ sudo apt install libopenmpi3
- pip3で追加パッケージを導入
    $ pip3 install future gensim
- torch,torchvisionを導入
    - https://mathinf.eu/pytorch/arm64/ からtorch*.whlとtorchvision*.whlをダウンロード
    - $ sudo pip3 install torch*.whl torchvision*.whl

## ソースコード

- 書籍『Kaggleスタートブック』のサンプルコードをjupyter notebookで入力して実行する
- すべてのコードが動作することを確認する
