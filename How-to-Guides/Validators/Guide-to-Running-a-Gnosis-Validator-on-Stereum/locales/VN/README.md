# Hướng dẫn triển khai Gnosis Validator trên Stereum

## Mục lục

<details>
  <summary> <a href="#about-stereum">Stereum</a></summary>
</details>
<details>
  <summary> <a href="#hardware-requirements">Yêu cầu phần cứng</a></summary>
  <ol>
    <li>
      <a href="#minimum">Tối thiểu</a>
    </li>
    <li>
      <a href="#recommended">Khuyến nghị</a>
    </li>
  </ol>
</details>
<details>
  <summary> <a href="#preparing-the-server">Chuẩn bị máy chủ</a></summary>
  <ol>
    <li>
      <a href="#home-server">Máy chủ tại nhà</a>
    </li>
    <li>
      <a href="#cloud-server">Máy chủ đám mây</a>
    </li>
  </ol>
</details>
<details>
  <summary> <a href="#stereum-launcher">Stereum Launcher</a></summary>
  <ol>
    <li>
      <a href="#step-1-connecting-to-server">Bước 1: Kết nối với máy chủ</a>
    </li>
    <li>
      <a href="#step-2-installation">Bước 2: Cài đặt</a>
    </li>
  </ol>
</details>
<details>
  <summary> <a href="#setup-validator-for-gnosis-chain">Cài đặt trình xác thực cho Gnosis Chain</a></summary>
  <ol>
    <li>
      <a href="#step-1-generate-validator-keys">Bước 1: Tạo khóa cho trình xác thực</a>
    </li>
    <li>
      <a href="#step-2-upload-your-validator-keys-to-stereum">Bước 2: Tải lên khóa của trình xác thực trên Stereum</a>
    </li>
    <li>
      <a href="#step-3-deposit-gno-for-your-validator">Bước 3: Gửi GNO cho trình xác thực</a>
    </li>
  </ol>
</details>
<details>
  <summary> <a href="#monitoring-your-validators">Giám sát trình xác thực</a></summary>
</details>

# About Stereum

Stereum cung cấp một cách nhanh chóng và dễ dàng để những staker đơn lẻ thiết lập các nút trình xác thực của họ trong vòng vài phút. Giao diện người dùng đồ họa (GUI) dễ sử dụng, linh hoạt và miễn phí cho phép người dùng xác thực chỉ bằng một cú nhấp chuột, loại bỏ các thiết lập phức tạp và cho phép nhiều người nhất có thể tham gia ở cấp độ mạng, giữ cho mạng được phân cấp và trung lập.

Để bắt đầu xác thực Gnosis bằng Stereum, người dùng sẽ phải chuẩn bị những thứ sau:

1. Một máy tính (Windows, Mac, Linux) để chạy Stereum Launcher
2. Một máy chủ chạy Linux để lưu trữ nút trên (Đám mây, Máy cục bộ, Máy ảo)

# Hardware Requirements

## Minimum

- 4 Core CPU
- 1TB SSD Storage
- 8GB RAM Memory
- 10Mbit/s Wired Network

## Recommended

- 8 Core CPU
- 2TB SSD Storage
- 16GB RAM Memory
- 25Mbit/s Wired Network

# Preparing the Server

## Home Server

Coming soon!

## Cloud Server

### DigitalOcean

#### Step 1: Create Project on DigitalOcean

<img width="776" alt="Untitled" src="https://user-images.githubusercontent.com/51161692/211513915-4ad906fb-a00b-44bd-96da-5ebe54c180bb.png">

#### Step 2: Create & Setup Droplets

Lựa chọn một máy chủ để chạy Stereum. Theo thiết lập được đề xuất, chúng tôi sẽ lựa chọn máy với vị trí gần nhất (Ví dụ Singapore) với **4 Core CPU**, **32GB RAM,** **900GB SSD Storage & Ubuntu**.

Để xác thực, chúng tôi sẽ chọn tùy chọn Mật khẩu.

<img width="1434" alt="Screenshot_2023-01-10_at_3 00 57_PM" src="https://user-images.githubusercontent.com/51161692/211514326-1f453a95-a5c3-4f48-a85d-058c6b4a4d0d.png">

# Stereum Launcher

Khi máy chủ đã được thiết lập và chạy, bây giờ chúng ta sẽ tải xuống Trình khởi chạy Stereum từ [stereum.net](http://stereum.net) để thiết lập máy chủ. Tải xuống Stereum tùy theo hệ điều hành mà máy của bạn đang chạy (Mac, Windows & Linux).

## Step 1: Connecting to Server

Nhập chi tiết máy chủ của bạn như được cung cấp bởi các nhà cung cấp đám mây đã chọn của bạn. Trong trường hợp này, vì chúng tôi không sử dụng ssh, chúng tôi sẽ tắt USE SSH KEY.

<img width="1052" alt="Untitled 1" src="https://user-images.githubusercontent.com/51161692/211514465-5b995348-a9f7-4f22-8335-2b9e884a7fc9.png">

## Step 2: Installation

Sau khi kết nối với nút của bạn, Stereum cung cấp cho bạn nhiều tùy chọn liên quan đến việc cài đặt nút Gnosis của bạn:

- Cài đặt với 1-Click
    - Cài đặt nút được cấu hình sẵn
- Cài đặt tùy chỉnh
    - Chọn clients tùy chỉnh
- Nhập cấu hình
    - Để di chuyển thiết lập nút, các tệp cấu hình trước đó có thể được nhập để tiết kiệm thời gian

Trong hướng dẫn này, chúng tôi sẽ chọn "Cài đặt với 1-Click" vì nó cung cấp tùy chọn đơn giản nhất để xác thực Gnosis.

<img width="1058" alt="Untitled 2" src="https://user-images.githubusercontent.com/51161692/211514531-d877c1b1-8aba-445b-82bc-f276bd3bdaa5.png">

Khi tùy chọn đã được chọn, hãy chọn Gnosis trong trình đơn thả xuống và chọn Staking. Cuối cùng, chọn Install.

<img width="1053" alt="Untitled 3" src="https://user-images.githubusercontent.com/51161692/211514610-8d5b8180-ebd8-4a4e-843e-44cf1dd35862.png">

Stereum sẽ tự động cài đặt các dịch vụ và ứng dụng khách cần thiết. Trong bước này, bạn có thể đồng bộ hóa ứng dụng khách của mình từ đầu chuỗi.

<img width="1055" alt="Untitled 4" src="https://user-images.githubusercontent.com/51161692/211514666-6e01f5eb-a26c-4843-bf03-02f8d136a226.png">

Bạn có thể dồng bộ node của bạn với đường dẫn đồng bộ. Bạn có thể lựa chọn server https://checkpoint.gnosischain.com/ cung cấp bởi Gnosis hoặc https://checkpoint-sync-gnosis.dappnode.io/ cung cấp bởi DAppNode. Sau đó, chọn Next

<img width="1055" alt="Untitled 4" src="https://user-images.githubusercontent.com/23649434/211530109-d5ed75e3-be81-4695-87f5-1ddb0d6a4442.png">

Nhấp vào Next sau khi xác minh các dịch vụ mà Stereum đang cài đặt.

<img width="1051" alt="Untitled 5" src="https://user-images.githubusercontent.com/51161692/211514724-6f5a4b07-4d3c-4385-8eca-06fa3170b080.png">

Nhấn Install để bắt đầu trình cài đặt.

<img width="1053" alt="Untitled 6" src="https://user-images.githubusercontent.com/51161692/211514760-a4a6a1e2-5669-4410-9791-f7c1cc7a2393.png">

Quá trình cài đặt sẽ tự động bắt đầu. Sau đó, bạn cần đợi quá trình cài đặt hoàn tất trước khi áp dụng các thay đổi tiếp theo.

<img width="1054" alt="Untitled 7" src="https://user-images.githubusercontent.com/51161692/211514861-4dff24f6-c357-457d-a7cf-122147c48833.png">

# Setup Validator for Gnosis Chain

Bây giờ bạn đã thiết lập và định cấu hình Stereum trên máy của bạn, bạn có thể bắt đầu xác thực cho Gnosis.

## Step 1: Generate Validator Keys

Để bắt đầu xác thực Gnosis, bạn sẽ cần tạo khóa riêng của trình xác thực để ký các hoạt động trên chuỗi. Thực hiện theo hướng dẫn này [here](https://docs.gnosischain.com/node/guide/validator/generate-keys/wagyu) để xem cách bạn có thể tạo khóa trình xác thực bằng Wagyu Key Gen.

## Step 2: Upload your Validator Keys to Stereum

Chúng tôi sẽ tải lên tệp keystone được tạo ở bước trước lên Stereum. Ở menu trên cùng bên trái, chọn STAKING. Bạn có thể nhấp hoặc kéo tệp keystore.json của mình vào cửa sổ để tải các tệp kho khóa của mình.

![Untitled 8](https://user-images.githubusercontent.com/51161692/211514963-99055c7d-71d3-4956-b031-caa5b87c906c.png)

Khi bạn đã tải lên tệp keystore.json, nhấn ENTER PASSWORD & IMPORT để nhập khóa của bạn.

![Untitled 9](https://user-images.githubusercontent.com/51161692/211515032-2e51112d-37c4-41d6-ad96-21e0e39a2704.png)

Sau khi hoàn tất, các khóa của bạn sẽ bắt đầu tải.

![Untitled 10](https://user-images.githubusercontent.com/51161692/211515065-0f518e48-7a9a-4cf7-b96e-33e5999ae20a.png)

Chúc mừng! Kho khóa của bạn đã được tải trên nút Stereum của bạn.

![Untitled 11](https://user-images.githubusercontent.com/51161692/211515114-7ed5318e-349a-4be3-a839-14f72908fb66.png)

## Step 3: Deposit GNO for your validator

> :warning: Chú ý: Đảm bảo nút Stereum được đồng bố hóa hoàn toàn trước khi gửi GNO của bạn. Nếu không, trình xác nhận của bạn sẽ ngoại tuyến và bạn sẽ bị phạt vì không xử lý quá trình xác thực. Quá trình đồng bộ hóa sẽ mất khoảng 1-4 ngày.

Để kiểm tra trạng thái đồng bộ hóa của nút, ở menu trên cùng bên trái, chọn Control và bạn sẽ có thể xem trạng thái đồng bộ hóa cho cả ứng dụng khách thực thi và ứng dụng đồng thuận của mình. Đảm bảo rằng cả hai đều được đồng bộ hóa hoàn toàn trước khi tiếp tục.

![Untitled 12](https://user-images.githubusercontent.com/51161692/211515197-032f6ec6-1cfd-4daa-88ea-0a2d2b51fa77.png)

Khi các khóa trình xác thực đã được tạo, bạn sẽ cần gửi một khoản tiền **1 GNO for each validator**. Thực hiện theo hướng dẫn cập nhật này [here](https://docs.gnosischain.com/node/guide/validator/deposit) để xem cách bạn có thể gửi tiền cho trình xác thực của mình.

# Monitoring your Validators

Nếu bạn đã chọn Cài đặt 1-click khi bắt đầu, dịch vụ Grafana sẽ được cài đặt cùng với nút Stereum. Để truy cập bảng điều khiển Grafana nhằm giám sát trình xác thực của bạn, hãy chọn biểu tượng Grafana nằm ở phía bên phải của thanh menu. Khi cửa sổ bật lên xuất hiện, hãy mở Bảng điều khiển Grafana trong trình duyệt của bạn.

![Untitled 13](https://user-images.githubusercontent.com/51161692/211515244-0ad72939-cd97-458a-989a-85ee79b60b7c.png)

Chọn Dashboard trên menu nằm ở bên trái và bên dưới phần chung, bạn sẽ thấy danh sách các bảng điều khiển được định cấu hình trước cho nút của mình.

![Untitled 14](https://user-images.githubusercontent.com/51161692/211515285-4d3735c0-d39e-4a88-946b-90fb3e82575b.png)

![Untitled 15](https://user-images.githubusercontent.com/51161692/211515322-b47d374f-b713-4473-b044-3d68144e997f.png)
