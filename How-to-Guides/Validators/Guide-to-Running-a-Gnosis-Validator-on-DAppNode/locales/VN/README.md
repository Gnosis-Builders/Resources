# Hướng dẫn triển khai Gnosis Validator trên DAppNode

Hướng dẫn này sẽ chỉ ra cách chạy Gnosis Validator tại nhà. Vì hướng dẫn này được thực hiện ví dụ trên dịch vụ đám mây, vậy nên bạn có thể thực hiện nó bằng phương pháp tương tự với phần cứng của bạn ở nhà. Ngoài ra, bạn có thể cài đặt DAppNode thông qua [hướng dẫn cài đặt DAppNode bằng ISO](https://docs.dappnode.io/user/quick-start/core/installation/#iso-installation).

### 1. Tạo Docker Droplet trên Cloud Server
Dựa theo [Beacon Chain Node Requirement](https://docs.gnosischain.com/node/consensus-layer-validator#beacon-chain-node-requirements), bạn cần cài đặt một máy tính với SSD do tốc độ chậm của HDD. Cấu hình khuyến nghị để thực hiện theo hướng dẫn này là 32 GB Memory / 600 GB Disk - Ubuntu 22.10 x64

### 2. Kết nối với Droplet
Sao chép địa chỉ ipv4 của bạn. Mở terminal và gõ dòng lệnh sau đây `ssh root@<YOUR_DROPLET_IPV4_ADDRESS>`

### 3. Cài đặt DAppNode
Dựa theo [Cấu hình tối thiểu để chạy node từ tài liệu chính thức](https://docs.gnosischain.com/node/#requirements), node của bạn cần có CPU 4 nhân, 16 GB RAM và NVMe SSD (preferred) hoặc SATA SSD. Cấu hình khuyến nghị để thực hiện theo hướng dẫn này là 32 GB Memory / 600 GB Disk - Ubuntu 22.10 x64.


Get Prerequisites

```sudo wget -O - https://prerequisites.dappnode.io | sudo bash```

Script Installation

```sudo wget -O - https://installer.dappnode.io | sudo bash```

### 4. Khởi tại DAppNode aliases
Khi bạn cài đặt thành công DAppNode, bạn có thể sử dụng dòng lệnh ```dappnode_help``` để xem các dòng lệnh có thể được sử dụng với DAppNode. Nếu không nhận được kết quả sau khi chạy dòng lệnh ```dappnode_help```, bạn cần khỏi tạo aliases với câu lệnh sau

```
alias
alias dappnode_connect='docker exec -ti DAppNodeCore-vpn.dnp.dappnode.eth getAdminCredentials'
alias dappnode_get='docker exec -t DAppNodeCore-vpn.dnp.dappnode.eth vpncli get'
alias dappnode_help='echo -e "\n\tDAppNode commands available:\n\n\tdappnode_help\t\tprints out this message\n\n\tdappnode_wifi\t\tget wifi credentials (SSID and password)\n\n\tdappnode_openvpn\tget Open VPN credentials\n\n\tdappnode_wireguard\tget Wireguard VPN credentials (dappnode_wireguard --help for more info)\n\n\tdappnode_connect\tcheck connectivity methods available in DAppNode\n\n\tdappnode_status\t\tget status of dappnode containers\n\n\tdappnode_start\t\tstart dappnode containers\n\n\tdappnode_stop\t\tstop dappnode containers\n"'
alias dappnode_openvpn='docker exec -i DAppNodeCore-vpn.dnp.dappnode.eth getAdminCredentials'
alias dappnode_openvpn_get='docker exec -i DAppNodeCore-vpn.dnp.dappnode.eth vpncli get'
alias dappnode_start='docker-compose $DNCORE_YMLS up -d && docker start $(docker container ls -a -q -f name=DAppNode*)'
alias dappnode_status='docker-compose $DNCORE_YMLS ps'
alias dappnode_stop='docker-compose $DNCORE_YMLS stop && docker stop $(docker container ls -a -q -f name=DAppNode*)'
alias dappnode_wifi='cat /usr/src/dappnode/DNCORE/docker-compose-wifi.yml | grep "SSID\|WPA_PASSPHRASE"'
alias dappnode_wireguard='docker exec -i DAppNodeCore-api.wireguard.dnp.dappnode.eth getWireguardCredentials'
alias ls='ls --color=auto'
```

### 5. Cài đặt VPN - Wireguard VPN
Để kết nối với DAppNode, chúng ta có rất nhiều phương pháp như sử dụng local proxy, wifi hotspot, VPN Connections, và CLI. Trong hướng dẫn này chúng ta triển khai Gnosis Validator trên dịch vụ Cloud sử dụng DAppNode, vì vậy nên sử dụng phương pháp kết nối qua VPN là phương pháp tối ưu và được sử dụng rộng rãi. OpenVPN và Wireguard là 2 phần mềm phù hợp. Tuy nhiên, trong hướng dẫn này, chúng ta sẽ thực hiện với Wireguard do tiện lợi hơn với cài dặt qua dòng lệnh trên server và ổn định hơn.

Để cài đặt Wireguard trên Ubuntu, bạn có thể thực hiện những dòng lệnh sau:
```
sudo apt install wireguard
```

Trong trường hợp DAppNode không xuất hiện thông tin để kết nối, bạn cần sử dụng dòng lệnh sau ```dappnode_connect``` để đọc được thông tin.

Bạn sẽ nhận được các thông tin tương tự phía dưới
```
Connect to DAppNode through Wireguard using the following credentials:
Preparing remote text Wireguard credentials; use CTRL + C to stop

[Interface]
Address = <YourInterfaceAddress>
PrivateKey = <YourInterfacePrivatekey>
ListenPort = 51820
DNS = <YourInterfaceDNS>

[Peer]
PublicKey = <YourPeerPublicKey>
Endpoint = <YourPeerEndpoint>
AllowedIPs = <YourPeerAllowedIPs>
```

### 6. Kết nối DAppNode với VPN
Dựa theo [Wireguard installation guide](https://docs.dappnode.io/user-guide/ui/access/vpn/#linux) để cài đặt Wireguard trên máy tính của bạn, bạn cần thiết lập wg0 configure file ```sudo nano /etc/wireguard/wg0.conf``` và sao chép thông tin thiết lập ở mục 4 vào file thiết lập này. 
Sau đó, bạn chỉ cần khởi chạy Wireguard thông qua dòng lệnh: ```sudo wg-quick up wg0```. Bạn sẽ có được kết quả như phía dưới đây.

```
[#] ip link add wg0 type wireguard
[#] wg setconf wg0 /dev/fd/63
[#] ip -4 address add 10.24.0.2 dev wg0
[#] ip link set mtu 1412 up dev wg0
[#] resolvconf -a tun.wg0 -m 0 -x
[#] ip -4 route add 10.20.0.0/24 dev wg0
[#] ip -4 route add 172.33.0.0/16 dev wg0
```

Ngoài ra, khi chạy ```sudo wg```, bạn sẽ thấy phần sau trong terminal

```
interface: wg0
  publickey: <public_key>
  privatekey: (hidden)
  listening port: 51820
```

Sau đó bạn hãy truy cập website http://my.dappnode trên trình duyệt của bạn.

![image](https://user-images.githubusercontent.com/23649434/201589528-7b7edab0-f7f7-48fa-a656-f55b416cd505.png)

Bạn có thể tham khảo những bước cấu hình DAppNode đầu tiên tại đây [Initial Configurations for the DAppNode](https://docs.dappnode.io/first-steps#)

### 7. Cài đặt Gnosis Validator trên DAppNode
Bây giờ chúng tôi sẽ định cấu hình và thiết lập DAppNode trên máy chủ đám mây của bạn để chúng tôi có thể bắt đầu xác thực cho Gnosis Chain. Bạn có thể làm theo các bước bên dưới để thiết lập (các) trình xác thực của mình. Tìm hiểu cách thiết lập DAppNode qua tài liệu trực tuyến này [https://docs.gnosischain.com/node/tools/dappnode](https://docs.gnosischain.com/node/tools/dappnode)

