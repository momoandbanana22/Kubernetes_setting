https://www.ooub.net/archives/log632

Lubuntu 20.04でCtrlキーとCapsLockキーを入れ替える

2020/6/4 2020/6/5 設定

Lubuntuのバージョン毎にウィンドウマネージャーが異なるためか、CtrlキーとCapsLockキーの入れ替えもまちまちなようです。

試行錯誤の結果、Lubuntu 20.04では以下の方法で入れ替えることができました。

■やり方
/etc/default/keyboardを開き、以下のように修正する。

[修正前]

XKBOPTIONS=""

[修正後]

XKBOPTIONS="ctrl:swapcaps"

マシンを再起動する。
参考サイト

LinuxでCtrlキーとCapsロックキーを入れ替える方法 (色々なウィンドウマネージャでのやり方を解説)
