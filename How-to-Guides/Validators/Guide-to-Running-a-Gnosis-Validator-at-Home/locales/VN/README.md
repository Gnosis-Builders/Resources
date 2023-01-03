# Hướng dẫn triển khai Gnosis Validator Node trên Cloud.

Hướng dẫn này sẽ chỉ ra cách chạy Gnosis Validator tại nhà. Vì hướng dẫn này được thực hiện trên dịch vụ đám mấy như là một ví dụ, vậy nên bạn có thể thực hiện nó bằng phương pháp tương tự với phần cứng của bạn ở nhà.

Hướng dẫn này có phiên bản [tiếng Anh](https://github.com/Gnosis-Builders/Resources/tree/main/How-to-Guides/Validators/Guide-to-Running-a-Gnosis-Validator-at-Home), [tiếng Việt](https://github.com/Gnosis-Builders/Resources/tree/main/How-to-Guides/Validators/Guide-to-Running-a-Gnosis-Validator-at-Home/locales/VN).

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

### 4. Cài đặt VPN - Wireguard VPN
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

### 5. Kết nối DAppNode với VPN
Dựa theo [Wireguard installation guide](https://docs.dappnode.io/user-guide/ui/access/vpn/#linux) để cài đặt Wireguard trên máy tính của bạn, bạn cần thiết lập wg0 configure file ```sudo nano /etc/wireguard/wg0.conf``` và sao chép thông tin thiết lập ở mục 4 vào file thiết lập này. 
Sau đó, bạn chỉ cần khởi chạy Wireguard thông qua dòng lệnh: ```sudo wg-quick up wg0```. Bạn sẽ có được kết quả như phía dưới đây.

![image](https://user-images.githubusercontent.com/23649434/201591812-97c4bcb7-5760-485f-a7a3-62d5e8418d46.png)


Sau đó bạn hãy truy cập website http://my.dappnode trên trình duyệt của bạn.

![image](https://user-images.githubusercontent.com/23649434/201589528-7b7edab0-f7f7-48fa-a656-f55b416cd505.png)

Bạn có thể tham khảo những bước cấu hình DAppNode đầu tiên tại đây [Initial Configurations for the DAppNode](https://docs.dappnode.io/first-steps#)

### 6. Cài đặt Gnosis Validator trên DAppNode
Thông qua Admin UI, bạn vui lòng truy cập DAppStore và tìm kiếm Gnosis Beacon Chain Prysm.

![image](https://user-images.githubusercontent.com/23649434/201592661-9111180f-3ab3-49d8-a1ca-c67fa53e5cb2.png)

Ảnh phía dưới thể hiện cấu hình chi tiết

![image](https://user-images.githubusercontent.com/23649434/201593138-663c57bc-5351-41f7-a786-817e4e1a8bcb.png)

Bạn cần cài đặt Nethermind Xdai(Gnosis chain) để duy trì trình thực thi.

Sau khi cài đặt thành công, trạng thái đồng bộ hóa sẽ được hiển thị trên bảng điều khiển.

### 7. Tạo Khóa
Hướng dẫn này có 2 phương pháp để tạo khóa.

1. Phương pháp thứ nhất là sử dụng Docker để tạo keystores. Dựa trên hướng dẫn chính thức [Step 1) Generate Validator Account(s) and Deposit Data](https://docs.gnosischain.com/node/consensus-layer-validator#step-1-generate-validator-accounts-and-deposit-data). Bạn vui lòng thực hiện các dòng lệnh phía sau đây để tạo Keystores

Tải xuống Key Generator

```
cd
sudo docker pull ghcr.io/gnosischain/validator-data-generator:latest
```

Tạo thư mục cho Key Storage
```
mkdir home/<your_username>/vkeys
```

Chạy Generator để tạo Keystores bằng cách thay đổi các giá trị sau trong dòng lệnh `/home/<your_username>/`, NUM, WITHDRAWAL_ADDRESS
```
docker run -it --rm -v /home/<your_username>/validator_keys:/app/validator_keys \
  ghcr.io/gnosischain/validator-data-generator:latest new-mnemonic \
  --num_validators=NUM --mnemonic_language=english \
  --folder=/app/validator_keys --eth1_withdrawal_address=WITHDRAWAL_ADDRESS
```

Sau đó, bạn sẽ có tìm thấy file deposit_data*.json và keystores trong /home/<your_username>/validator_keys

2. Phương pháp thứ 2 là sử dụng Gnosis Wagyu Key Gen.

Cài đặt Gnosis Wagyu Key Gen thông qua [link](https://github.com/alexpeterson91/Gnosis-Wagyu-Key-Gen/releases). Sau đó, mở Gnosis Wagyu Key Gen và hoàn thành các bước Create Secret Recovery Phrase, Configure Validator Keys như ảnh phía dưới.

Thực hiện từng bước, chúng ta sẽ có
![image](https://user-images.githubusercontent.com/23649434/201819925-3e318c83-798f-4397-b860-71f857898804.png)

Sau đó, bạn có thể tạo Validator Key như hình bên dưới
![image](https://user-images.githubusercontent.com/23649434/201820812-5119f61f-c096-4b8d-b4d4-aec960ae7f6f.png)


### 8. Thiết lập Keystores trên Web3signer Gnosis package
Truy cập [http://ui.web3signer-gnosis.dappnode/](http://ui.web3signer-gnosis.dappnode/). Sau đó, bạn cần tải lên keystores từ file bạn đã tạo ở bước phía trước.
Cuối cùng, bạn có validator public key như hình ảnh phía dưới và bạn có thể kiểm tra validator public key thông qua website của Gnosischain. https://beacon.gnosischain.com/validator/<your_validator_publickey>

![image](https://user-images.githubusercontent.com/23649434/201821914-47f9279a-91c2-4dc1-9c86-49dbff4cba78.png)

### 9. Triển khai validator của bạn
Khi bạn đã có validator public key, bạn cần triển khai validator hoạt động bằng cách gửi 1GNO trên 1 validator. Truy cập website [https://deposit.gnosischain.com/](https://deposit.gnosischain.com/) và kết nối với ví của bạn. Sau đó, bạn có thể tải lên deposit_data*.json đã tạo ở bước số 7.

![image](https://user-images.githubusercontent.com/23649434/201823454-dd479504-bcc6-4aa2-8ba3-35df0ad4834f.png)

Sau đó, bạn thực hiện `Deposit`. Cuối cùng, bạn đã tạo Gnosis Validator thành công. Mọi thông tin chi tiết có thể tham khao thêm thông qua tài liệu chính thức của Gnosis Chain [https://docs.gnosischain.com/](https://docs.gnosischain.com/).

