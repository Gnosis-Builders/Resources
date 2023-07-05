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
Thông qua Admin UI, bạn vui lòng truy cập DAppStore và tìm kiếm Gnosis Beacon Chain Prysm.

![image](https://user-images.githubusercontent.com/23649434/201592661-9111180f-3ab3-49d8-a1ca-c67fa53e5cb2.png)

Ảnh phía dưới thể hiện cấu hình chi tiết

![image](https://user-images.githubusercontent.com/23649434/201593138-663c57bc-5351-41f7-a786-817e4e1a8bcb.png)

Bạn cần cài đặt Nethermind Xdai(Gnosis chain) để duy trì trình thực thi.

Sau khi cài đặt thành công, trạng thái đồng bộ hóa sẽ được hiển thị trên bảng điều khiển.

### 8. Tạo Khóa
Hướng dẫn này có 2 phương pháp để tạo khóa.

1. Phương pháp thứ nhất là sử dụng Command Line Tool.
Bạn có thể tham khảo [hướng dẫn chính thức từ Gnosis Chain để tạo khóa thông qua Command Line Tool tại đây](https://docs.gnosischain.com/node/guide/validator/generate-keys/cli/)

2. Phương pháp thứ 2 là sử dụng Gnosis Wagyu Key Gen.

Cài đặt Gnosis Wagyu Key Gen thông qua [link](https://github.com/alexpeterson91/Gnosis-Wagyu-Key-Gen/releases). Sau đó, mở Gnosis Wagyu Key Gen và hoàn thành các bước Create Secret Recovery Phrase, Configure Validator Keys như ảnh phía dưới.

Thực hiện từng bước, chúng ta sẽ có
![image](https://user-images.githubusercontent.com/23649434/201819925-3e318c83-798f-4397-b860-71f857898804.png)

Sau đó, bạn có thể tạo Validator Key như hình bên dưới
![image](https://user-images.githubusercontent.com/23649434/201820812-5119f61f-c096-4b8d-b4d4-aec960ae7f6f.png)


### 9. Thiết lập Keystores trên Web3signer Gnosis package
Truy cập [http://ui.web3signer-gnosis.dappnode/](http://ui.web3signer-gnosis.dappnode/). Sau đó, bạn cần tải lên keystores từ file bạn đã tạo ở bước phía trước.
Cuối cùng, bạn có validator public key như hình ảnh phía dưới và bạn có thể kiểm tra validator public key thông qua website của Gnosischain. https://beacon.gnosischain.com/validator/<your_validator_publickey>

![image](https://user-images.githubusercontent.com/23649434/201821914-47f9279a-91c2-4dc1-9c86-49dbff4cba78.png)

### 10. Triển khai validator của bạn
Khi bạn đã có validator public key, bạn cần triển khai validator hoạt động bằng cách gửi 1GNO trên 1 validator. Truy cập website [https://deposit.gnosischain.com/](https://deposit.gnosischain.com/) và kết nối với ví của bạn. Sau đó, bạn có thể tải lên deposit_data*.json đã tạo ở bước số 7.

![image](https://user-images.githubusercontent.com/23649434/201823454-dd479504-bcc6-4aa2-8ba3-35df0ad4834f.png)

Sau đó, bạn thực hiện `Deposit`. Cuối cùng, bạn đã tạo Gnosis Validator thành công. Mọi thông tin chi tiết có thể tham khao thêm thông qua tài liệu chính thức của Gnosis Chain [https://docs.gnosischain.com/](https://docs.gnosischain.com/).

