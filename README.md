Hardware
This is the hardware configuration required to set up an Avail node:

Component	Minimum	Recommended
RAM	4GB	8GB
CPU (amd64/x86 architecture)	2 core	4 core
Storage (SSD)	20-40 GB	200-300 GB

# Hướng dẫn Cài đặt và Chạy Avail Node

****Bước 1: Cài đặt Rust và các phụ thuộc cần thiết********

**Cập nhật danh sách gói:**

sudo apt-get update

**Cài đặt các công cụ cơ bản và các phần mềm cần thiết:**

sudo apt install build-essential
sudo apt install --assume-yes git clang curl libssl-dev protobuf-compiler

**Cài đặt Rust bằng cách sử dụng script chính thức:**

curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source ~/.cargo/env

**Chọn phiên bản Rust mặc định là stable:**

rustup default stable

**Cập nhật Rust và thêm target wasm32-unknown-unknown cho phiên bản nightly:**

rustup update
rustup update nightly
rustup target add wasm32-unknown-unknown --toolchain nightly


**Bước 2: Chạy Avail Node**

git clone https://github.com/availproject/avail.git
cd avail
Tạo các thư mục output và data:
mkdir -p output
mkdir -p data

**Kiểm tra mã nguồn Avail ở phiên bản cụ thể (ví dụ: v1.7.2):**

git checkout v1.7.2
Biên dịch và chạy Avail Node với các tham số cụ thể:

cargo run --locked --release -- --chain kate -d ./output
Trên màn hình sẽ hiển thị thông tin về việc khởi chạy Avail Node.

**Bước 3: Tạo systemd service**

**Tạo một tệp dịch vụ systemd cho Avail Node:**

touch /etc/systemd/system/availd.service

**Chỉnh sửa các thông số trong tệp dịch vụ, bao gồm tên của Validator (VD: "nobita") và tùy chọn khởi động lại (nếu cần):**
**
[Unit] 
Description=Avail Validator
After=network.target
StartLimitIntervalSec=0
[Service] 
User=root 
ExecStart= /root/avail/target/release/data-avail --base-path `pwd`/data --chain kate --name "nobita"
Restart=always 
RestartSec=120
[Install] 
WantedBy=multi-user.target**


Lưu và đóng trình soạn thảo văn bản.

**Bước 4: Kích hoạt khởi động tự động khi khởi động hệ thống**

Kích hoạt dịch vụ để tự động khởi động khi hệ thống khởi động lại:

systemctl enable availd.service


Start it manually with:

systemctl start availd.service

You can check that it's working with:

systemctl status availd.service

You can tail the logs with journalctllike so:

journalctl -f -u availd

**Check your node on https://telemetry.avail.tools/**


