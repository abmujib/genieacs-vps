# genieacs
genieacs

INSTALL L2TP untuk TUNNEL GENIEACS
uname -r
cek pastikan tidak ada mengandung kata cloud

wget https://raw.githubusercontent.com/beryindo/genieacs/refs/heads/main/vpnsetup.sh
chmod +x vpnsetup.sh
./vpnsetup.sh
wget -O add_vpn_user.sh https://raw.githubusercontent.com/hwdsl2/setup-ipsec-vpn/master/extras/add_vpn_user.sh
bash add_vpn_user.sh 'username' 'password'

INTALL GENIEACS
wget https://raw.githubusercontent.com/beryindo/genieacs/refs/heads/main/genie.sh
chmod +x genie.sh
./genie.sh
