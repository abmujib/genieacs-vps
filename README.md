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
wget -O add_vpn_user.sh https://raw.githubusercontent.com/hwdsl2/setup-ipsec-vpn/master/extras/add_vpn_user.sh
bash add_vpn_user.sh 'username' 'password'
```


**INSTALL GENIEACS**
```
wget https://raw.githubusercontent.com/beryindo/genieacs/refs/heads/main/genie.sh
chmod +x genie.sh
./genie.sh
```

Selanjutnya silahkan buka URL genie acs IP:3000
Lanjutkan dengan update Config, Provisioning dan Virtual Parameter

```
mkdir /root/db
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
