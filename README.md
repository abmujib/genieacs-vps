# genieacs

**UNTUK DI LOKAL TIDAK PERLU INSTALL L2TP, BISA LANGSUNG KE PROSES INSTALL GENIEACS**

INSTALL L2TP untuk TUNNEL GENIEACS
```
apt update
apt upgrade
apt install curl
uname -r
```
cek pastikan TIDAK ada mengandung kata cloud

```
wget https://raw.githubusercontent.com/beryindo/genieacs/refs/heads/main/vpnsetup.sh
chmod +x vpnsetup.sh
./vpnsetup.sh
```

```
wget -O add_vpn_user.sh https://raw.githubusercontent.com/hwdsl2/setup-ipsec-vpn/master/extras/add_vpn_user.sh
bash add_vpn_user.sh 'username' 'password'
```
Tambahkan L2TP Client di Mikrotik anda.

Selanjutnya cek di VPS ppp berapa ada terhubung
```
ip route list
```
misal responsenya seperti ini
```
192.168.42.10 dev ppp0 proto kernel scope link src 192.168.42.1
```
Artinya Mikrotik anda terhubung dengan ppp0. Maka tambahkan route di VPS
```
ip route add 10.0.0.0/24 dev ppp0
```
**10.0.0.0/24** adalah ip lokal modem/onu.


=========================================================================

**INSTALL GENIEACS**
```
apt update
apt upgrade
apt install curl
wget https://raw.githubusercontent.com/beryindo/genieacs/refs/heads/main/genie.sh
chmod +x genie.sh
./genie.sh
```

Selanjutnya silahkan buka URL genie acs IP:3000.
Lanjutkan dengan update Config, Provisioning dan Virtual Parameter

```
mkdir /root/db
cd /root/db
wget https://github.com/beryindo/genieacs/raw/refs/heads/main/config.bson
wget https://github.com/beryindo/genieacs/raw/refs/heads/main/config.metadata.json
wget https://github.com/beryindo/genieacs/raw/refs/heads/main/presets.bson
wget https://github.com/beryindo/genieacs/raw/refs/heads/main/presets.metadata.json
wget https://github.com/beryindo/genieacs/raw/refs/heads/main/provisions.bson
wget https://github.com/beryindo/genieacs/raw/refs/heads/main/provisions.metadata.json
wget https://github.com/beryindo/genieacs/raw/refs/heads/main/virtualParameters.bson
wget https://github.com/beryindo/genieacs/raw/refs/heads/main/virtualParameters.metadata.json
mongorestore --db genieacs --drop /root/db
systemctl start genieacs-{cwmp,ui,nbi}
```

UPDATE
add ip route otomatis ketika l2tp terhubung, caranya buat ip static pada tiap user

edit
```
sudo nano /etc/ppp/chap-secrets
```
```
"bery1" l2tpd "12345678" "192.168.42.10"
"bery2" l2tpd "12345678" "192.168.42.11"
"bery3" l2tpd "12345678" "192.168.42.12"
"bery4" l2tpd "12345678" "192.168.42.13"
"bery5" l2tpd "12345678" "192.168.42.14"
"bery6" l2tpd "12345678" "192.168.42.15"
```

formatnya
```
"username" l2tpd "password" "ip local static yang diberikan ke user"
```

tambahkan /etc/ppp/ip-up.d/add-routes
```
sudo nano /etc/ppp/ip-up.d/add-routes
```
masukkan script dibawah ini, rubah ip tr069 modem kalian.
```
#!/bin/bash
# Auto add route berdasarkan IP remote user L2TP
# xl2tpd + pppd environment: $IPREMOTE, $IFNAME, $PEERNAME

case "$IPREMOTE" in
  192.168.42.10)  # bery1
    ip route add 192.168.120.0/22 dev $IFNAME
    ;;
  192.168.42.11)  # bery2
    ip route add 172.16.4.0/22 dev $IFNAME
    ;;
  192.168.42.12)  # bery3
    ip route add 172.16.12.0/22 dev $IFNAME
    ;;
  192.168.42.13)  # bery4
    ip route add 192.168.4.0/22 dev $IFNAME
    ;;
  192.168.42.14)  # bery5
    ip route add 192.168.8.0/22 dev $IFNAME
    ;;
  192.168.42.15)  # bery6
    ip route add 192.168.12.0/22 dev $IFNAME
    ;;
esac

exit 0
```
kasih akses
```
sudo chmod +x /etc/ppp/ip-up.d/add-routes
service xl2tpd restart
```

melihat ipsec ada disini
```
cat /etc/ipsec.secrets
```
