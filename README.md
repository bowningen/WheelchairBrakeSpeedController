<div id="top"></div>

# WheelChairBreakeSpeedController


## 目次

1. [概要](#概要)
2. [プロジェクト名](#プロジェクト名)
3. [詳細](#詳細)
4. [動作環境](#動作環境)
5. [実行方法](#実行方法)
6. [設定項目の一覧](#設定項目の一覧)
7. [トラブルシューティング](#トラブルシューティング)
8. [諸注意](#諸注意)
9. [更新履歴](#更新履歴)


## 概要

このプログラムはLinux版Python3系向けです

Arduino2台とシリアル通信を行い、片方に接続しておいたホール素子(あるいは他のセンサー)からの電圧が変動する間隔から物体の回転速度を検出し、その速度に応じてもう一台のArduinoに接続したサーボモーターを動かすプログラムです。

応用すればホール素子を用いて様々な物体の速度を計測することにご使用いただくことが可能です。


## 使用技術一覧
<p style="display: inline">
  <img src="https://img.shields.io/badge/-Python-F2C63C.svg?logo=python&style=for-the-badge">
  <img src="https://img.shields.io/badge/-Arduino-00979D.svg?logo=arduino&style=for-the-badge">
</p>


## プロジェクト名

ホール素子を用いた介助式車椅子の自動速度制御装置の開発(奈良高等学校 2024)

<!-- プロジェクトについて -->

## プロジェクトについて(概要)

私たちは、車椅子が斜面を下る際のスピード加速による危険性に着目した。最近は、自動ブレーキ付きの車椅子も存在しているが、後付けをすることができ、より簡易的で安価な資材で作れる自動ブレーキをつけることはできないかと考えた。そしてホール効果を用いて車椅子の速度を測定し、それに応じてかける強さを変えることができる自動ブレーキを試作した。

  <p align="left">
    <br />
    <!-- プロジェクト詳細にBacklogのWikiのリンク -->
    <a href="https://www.e-net.nara.jp/hs/nara/"><strong>詳細がないので高校のホームページ »</strong></a>
    <br />
    <br />
    

## 詳細


お使いの環境にpyserialをインストールして、config.iniのarduino1,arduino2をお使いのArduinoそれぞれ(1はホール素子側、2はモーター側)のシリアルポート番号に設定します(ls /dev/tty*)

その後実行したら何とかなるはずです

多分...


## 動作環境


| OS　　　　　　　　　  | バージョン |
| --------------------- | ---------- |
| Ubuntu                | 22.04.3    |

| 言語・フレームワーク  | バージョン |
| --------------------- | ---------- |
| Python                | 3.10.12    |
| pyserial              | 3.5        |
| Arduino IDE           | 1.8.18     |
| servo                 | 1.2.1      |


## 必須ライブラリ

・pyserial (Python)

・servo (Arduino)


## 開発環境構築

あらかじめお使いのPCにPythonとArduino IDEをインストール、その後、各モジュールの最新版をpip等でインストールしてください。

なお、servoライブラリはArduino IDE側の「ライブラリの管理」からインストールしてください。


## 実行方法


※実行時にエラーが出る場合が多いですが、シリアル通信関連で例外が発生していることがほとんどなので、何度かCtrl+Zで強制終了して再実行を繰り返していると動作するはずです（v1.1.0で修正予定）


## 設定項目の一覧

### ・config.ini
| 変数名                 | 役割                                      | デフォルト値                       |
| ---------------------- | ----------------------------------------- | ---------------------------------- |
| ARDUINO1               | Arduino(センサーを取り付けたもの) に割り当てられているポート | /dev/ttyACM0                       |
| ARDUINO2               | Arduino(モーターを取り付けたもの) に割り当てられているポート | /dev/ttyUSB0                       |
| RESULT                 | 各種情報の出力先(未使用) (※1)                           | result.txt                         |
| HOLE1                  | ホール素子1つ目の監視ピン(未使用)                         | 15                                 |
| HOLE2                  | ホール素子2つ目の監視ピン(未使用)                         | 16                                 |
| SERV1                  | モーターの名前1 (※2)                                   | L                                  |
| SERV2                  | モーターの名前2 (※2)                                   | R                                  |
| gp_outL                | モーター制御信号出力ピン1 (※3)                          |  7                                 |
| gp_outR                | モーター制御信号出力ピン1 (※3)                          | 11                                 |
| TIRE                   | 速度を計測するタイヤ(あるいは円盤)が何インチであるか         | 16                                 |
| MUG_NUM                | 速度を計測するタイヤ(あるいは円盤)につけた磁石の数          | 4                                  |
| VTE_OFF                | 磁石が接近していない状態でのホール電圧                     | 2.28                               |
| VTE_ERROR              | 磁石が接近していない状態での許容される電圧のブレの範囲       | 0.2                                |
| POS_MIN                | ブレーキOFF状態でのモーターの角度(未使用) (※4)            | 80                                 |
| POS_MAX                | フルブレーキ状態でのモーターの角度(未使用) (※4)           | 120                                |
| POS_MINL               | ブレーキOFF状態でのモーター1の角度                        | 90                                 |
| POS_MAXL               | フルブレーキ状態でのモーター1の角度                       | 110                                |
| POS_MINR               | ブレーキOFF状態でのモーター2の角度                        | 95                                 |
| POS_MAXR               | フルブレーキ状態でのモーター2の角度                       | 40                                 |
| minSP                  | ブレーキが作動開始する最小速度                            | 0.4                                |
| maxSP                  | ブレーキが全力でかかる...つまり最大速度                    | 2.0                                |



※1 : 研究時間(と予算は0)が本当になかったので未実装のままです。(v1.1.0で実装予定)


※2 : Raspberry Piを用いて制御する場合に変数に用いる名前として指定します。
ただRaspberry Pi側でのモーターの動作の実装はしておりません(v1.2.0で実装予定)


※3 : Raspberry Piを用いて制御する場合に出力ピンとして指定します。
ただRaspberry Pi側でのモーターの動作の実装はしておりません(v1.2.0で実装予定)


※4 : 左右でモーターの角度を同じにしていた時のものです。
現在は左右で別の角度を指定できるようになったため、未使用となっております。


### ・holeRead_MTOnly.ino
### ・holeRead_HlOnly.ino

setup関数内の指定ピンをお手元の環境に合わせて変更してください。




## トラブルシューティング

### AttributeError: module 'serial' has no attribute 'Serial'
pyserialではなくserialをインストールしている可能性があります。
serialをインストールしている、あるいは両方をインストールしている場合、serialをアンインストールしてください。

### No such file or directory: '/dev/tty****'
指定しているシリアルポートが間違っている可能性があります。
Arduinoを抜き差ししつつ" ls /dev/tty* "を実行して特定を行ったうえで、config.iniに記述してください。

### 2.2.41がfloatに直せない等のエラーが出ている
わかりません。
なぜなのか聞きたいです。
おそらくシリアル通信中にいらないものを受け取っているようです。
まぁこのエラーはそのまま進んだはずですが...

### 実行した直後に〇等のエラーが出て停止する
わかりません。
上のエラーと同様、シリアル通信関連で何か間違ってるみたいです。
何度か実行すれば通るので Ctrl+Z で強制終了させ、もう一度実行してください。

## 諸注意
当プログラムを利用した事によるいかなる損害も一切の責任を負いません。  

自己の責任の上で使用して下さい。  


## 更新履歴
・v1.0.0 : リリース


## 更新予定

・v1.1.0 : 実行時などのいくつかのバグを修正予定。

・v1.2.0 : 実行環境にRaspberry Piを使用した場合に、Raspberry Pi側からのモーターの動作に対応予定。


<p align="right">(<a href="#top">トップへ</a>)</p>
