# genieacs

**UNTUK DI LOKAL TIDAK PERLU INSTALL L2TP, BISA LANGSUNG KE PROSES INSTALL GENIEACS**

INSTALL L2TP untuk TUNNEL GENIEACS
```
uname -r
```
cek pastikan TIDAK ada mengandung kata cloud

```
wget https://raw.githubusercontent.com/beryindo/genieacs/refs/heads/main/vpnsetup.sh
chmod +x vpnsetup.sh
./vpnsetup.sh
```

```
apt update
apt upgrade
apt install curl
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
