地址：http://10.214.20.213/wiki/HowtoVPN   (作者：pluskid)

#!/bin/bash
#provide vpn connect
> connect()
{
> > COUNT=0
> > sudo chmod 666 /var/run/l2tp-control
> > echo "c zju" > /var/run/l2tp-control
> > while ! ifconfig | grep -s 'ppp0' > /dev/null; do
> > > sleep 2
> > > COUNT=`expr $COUNT + 2`
> > > echo "等待VPN连接建立，已等待 $COUNT 秒。"
> > > if ifconfig | grep -s 'ppp0' > /dev/null;then
> > > > echo "VPN连接建立完成。"

> > > fi
> > > > if [$COUNT -gt 20 ](.md); then
> > > > > echo "**VPN连接出错**"
> > > > > echo "等待 $COUNT 秒，还是无法建立VPN连接，退出。"
> > > > > break

> > > > fi

> > done
}
if [$# = 1 ](.md); then
> > if [$1 = -r ](.md); then
> > > sudo etc/init.d/l2tpd restart
      1. waiting for l2tpd to startup
> > > while ! test -f /var/run/l2tp-control; do
> > > > sleep 1

> > > done
> > > connect
> > > ROUTE=$(ifconfig | grep 点对点 | awk '{print $3}' | cut -d':' -f2)
> > > sudo ip route add default via $ROUTE dev ppp0

> > fi
else
> > if ! ifconfig | grep -s 'ppp0' > /dev/null; then
> > > connect
> > > IP=$(ip route | grep '^default' | cut -d" " -f3)
> > > ROUTE=$(ifconfig | grep 点对点 | awk '{print $3}' | cut -d':' -f2)
> > > sudo ip route add 10.0.0.0/8 via $IP dev eth0
> > > sudo ip route add 222.205.0.0/16 via $IP dev eth0
> > > sudo ip route add 210.32.0.0/16 via $IP dev eth0
> > > sudo ip route del default
> > > sudo ip route add default via $ROUTE dev ppp0

> > else
> > > echo 'VPN连接已存在。'

> > fi
fi