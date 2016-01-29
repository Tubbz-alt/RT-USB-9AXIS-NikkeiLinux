# RT-USB-9AXIS-NikkeiLinux

<img src="./image/RT-USB-9AXIS-picture.png" width="240">

## 概要

本リポジトリは『Raspberry Pi で始めるかんたんロボット制作』で用いた
USB出力9軸IMUセンサモジュールのファームウェアの説明とダウンロード用
です.  

USB出力IMUセンサモジュールは株式会社アールティが製造・販売を行って
いるセンサモジュールになります.
このモジュールでは加速度, 角速度, 地磁気それぞれ3軸のデータをUSB経由で取得可能です.  

[USB出力9軸IMUセンサモジュール](http://products.rt-net.jp/iot/usb9axisimu/)

日経Linuxの連載では本センサから角度を出力して, ロボットの制御を行う例を
紹介しております.  

Raspberry Pi Mouseはラズベリーパイを用いて制御可能な移動台車ロボットです.  
詳細は以下を参照してください.

[Raspberry Pi Mouse](http://products.rt-net.jp/micromouse/raspberry-pi-mouse)


## 連載用ファームウェアと出荷時のファームウェアの相違点について

USB出力IMUセンサモジュールにはもともと9軸データを取得し送信し続ける処理を
するサンプルファームが書き込まれています.  

[USB出力9軸IMUセンサモジュールサンプルソフト等](http://www.rt-shop.jp/download/RT-IMU9/)

連載で使われているファームウェアではサンプルのファームウェアとは別のもので
センサを水平においた状態でその周りの角度値(度)を0.1秒毎に出力し続けます.  

<img src="./image/正方向の図.png" width="240">

注意点:

     ・連載用のファームではセンサの電源投入後数秒間センサを完全に静止した状態にする必要が
     あります.  この期間にジャイロセンサのキャリブレーションを行っています.  
     キャリブレーションが終了するとモジュール上のLEDが点滅します.  
     ・出力される角度はMEMSジャイロセンサの特性からドリフトしていきます.  


## ファームウェアの書き込み方法
連載用のファームウェアはfirmwareフォルダ内
のzipファイル内の
「NikkeiLinux_yaw_firm.bin」になります.
zipファイルを適当な場所に解凍してファームウェア
のbinファイルを取り出してください.  

以下, ファームウェアの書き込み方法について説明します.  

### Windows環境

1:	9軸センサモジュール上のタクトスイッチを押したままUSBケーブルを接続.このとき, モジュール上のLEDが弱く点灯します.   
2:	タクトスイッチから手を離します．   
3:	ブートローダーの起動まで待機(CRP DISABLEDという新しいDiskとして認識されます.)   
4:	もともとのfirmware.binを削除   
5:	新しい.binファイルをコピー   

<img src="./image/ファーム書き込みwin.png" width="320">

### Linux環境

1:    センサモジュール上のタクトスイッチを押したままUSBケーブルを接続. このとき, モジュール上のLEDが弱く点灯します.

2:    タクトスイッチから手を離します．

3:    mountコマンドでマウント名を調べる.(CRP DISABLEDという名前)    

       /dev/sdbのような名前のことが多いです.

4:    mtoolsというコマンドをインストールする.    

       sudo apt-get install mtools 
 
5:    sudo mdel –i マウントされている場所 ::/firmware.bin

       例 マウントされている場所が /dev/sdb だった場合      sudo mdel -i /dev/sdb ::/firmware.bin

6:    sudo mcopy –i マウントされている場所 新しいファイルの絶対path ::/

       例 マウントされている場所が /dev/sdbでダウンロードしてきたファームウェアのパスが
       /home/hogehoge/NikkeiLinux_yaw_firm.binの場合    
       sudo mcopy -i /dev/sdb /home/hogehoge/NikkeiLinux_yaw_firm.bin ::/

## ラズベリーパイでのセンサ値の出力読み取りについて
連載用のファームウェアの出力をラズベリーパイで
確かめてみます.  

ラズベリーパイにセンサを接続します.
(USBケーブルはmicroB端子のものです.)
9軸センサモジュールをラズベリーパイに接続した直後は
センサモジュールを完全に静止させてください.  
最初の数秒間でセンサのキャリブレーションを行っています.

       sudo cat /dev/ttyACM0

と打ち込みます.  すると画面上に角度が出力されます.  
画面への出力を止める際にはCtrl + Cを押してください.  

