# How to add validator to DOS Chain with WSL

## Avalanche + Ledger trong WSL (Windows 11) — Hướng dẫn đầy đủ

> Tài liệu này giúp thiết lập và kiểm thử ký giao dịch Avalanche bằng Ledger (Nano S/S+/X hoặc Stax) trong WSL2 (Ubuntu) trên Windows 11. Các bước chỉ đọc/kiểm thử đều không broadcast, an toàn để chạy.

***

### 0) Điều kiện

* Windows 11 + WSL2 (Ubuntu 22.04/24.04 hoặc 24.10).
* Ledger đã cài **Avalanche app** trong Ledger Live.
* Khi ký thật: **đóng Ledger Live** và mọi ví trên Windows.

***

### 1) Cài `usbipd` (Windows) và passthrough Ledger cho WSL

Mở **PowerShell (Run as Administrator)**:

```powershell
# Cài usbipd
winget install usbipd

# Liệt kê USB để tìm Ledger (VID 2c97)
usbipd list

# Bind (chỉ làm lần đầu cho thiết bị đó)
usbipd bind --busid <BUSID>

# Attach vào WSL (mỗi lần cắm lại hoặc sau reboot Windows)
usbipd attach --wsl --busid <BUSID>

# Kiểm tra lại
usbipd list
```

Trong **WSL (Ubuntu)** xác nhận thiết bị đã vào:

```bash
lsusb | grep -i 2c97          # thấy 2c97:xxxx (Stax: 6000, Nano X: 4000)
ls -l /dev/hidraw*            # có hidraw node
```

***

### 2) Bật systemd trong WSL (để udev hoạt động chuẩn)

Trong **WSL (Ubuntu)**:

```bash
sudo tee /etc/wsl.conf >/dev/null <<'INI'
[boot]
systemd=true
INI
```

Trên **Windows (PowerShell)**:

```powershell
wsl --shutdown
```

Mở lại **Ubuntu WSL**, kiểm tra:

```bash
systemctl status systemd-udevd   # phải active (running)
```

***

### 3) Cài Avalanche-CLI trong WSL

```bash
curl -sSfL https://raw.githubusercontent.com/ava-labs/avalanche-cli/main/scripts/install.sh | sh -s

# Đảm bảo ~/bin đã có trong PATH (idempotent đã cấu hình ở ~/.bashrc)
source ~/.bashrc

avalanche --version
```

***

### 4) Udev rules cho Ledger trong WSL

```bash
# Cài rules chính thức từ LedgerHQ
curl -fsSL https://raw.githubusercontent.com/LedgerHQ/udev-rules/master/add_udev_rules.sh | sudo bash

# Đảm bảo user thuộc nhóm plugdev
getent group plugdev || sudo groupadd plugdev
sudo usermod -aG plugdev "$USER"

# Áp rules
sudo udevadm control --reload-rules
sudo udevadm trigger

# Nếu vừa thêm group: đóng WSL rồi mở lại (Windows: wsl --shutdown)
```

Kỳ vọng sau khi re-attach Ledger:

```bash
ls -l /dev/hidraw*
# Ví dụ:
# crw-rw---- 1 root plugdev ... /dev/hidraw0
# crw-rw---- 1 root plugdev ... /dev/hidraw1
```

***

### 5) Kiểm thử an toàn (không ký, không broadcast)

#### 5.1 Kiểm HID mở được (Python, tùy chọn)

```bash
python3 - <<'PY'
import hid, sys
V, P = 0x2c97, 0x6000  # Stax; nếu Nano X: P=0x4000
devs = [d for d in hid.enumerate(V, P)]
print("Found", len(devs), "Ledger interface(s)")
try:
    h = hid.device()
    h.open(V, P)  # chỉ open, không gửi APDU
    print("HID OPEN OK")
    h.close()
except Exception as e:
    print("HID OPEN FAIL:", e); sys.exit(2)
PY
```

#### 5.2 Liệt kê địa chỉ trên Fuji (chỉ đọc)

> Mở khóa Ledger và mở **app Avalanche** trước.

```bash
avalanche key list --ledger 0 --fuji
```

#### 5.3 Liệt kê địa chỉ Mainnet bằng alias (chỉ đọc)

```bash
# Đặt tên (alias) cho index 0 trên Ledger — chỉ tạo bí danh cục bộ, không tạo private key
avalanche key import ledger --name stax0 --ledger 0

# Liệt kê địa chỉ theo tên trên mainnet
avalanche key list --keys stax0 --mainnet
```

***

### 6) (Tùy chọn) Import metadata blockchain vào local

Không bắt buộc. Dùng khi muốn đặt tên và tái sử dụng cấu hình chain gọn hơn. Nếu bản CLI hỗ trợ:

```bash
avalanche blockchain import \
  --rpc https://main.doschain.com \
  --blockchain-id 22v7AG7h6qaVxd4bLvAsSsg2LZ4RCn5iVYgFn7a2Fj1LCuYwjv \
  --name dos-main

avalanche blockchain list
avalanche blockchain describe dos-main
```

Sau đó có thể dùng `dos-main` thay cho `--rpc`/`--blockchain-id` trong các lệnh khác.\
Nếu CLI của bạn chưa có `blockchain import`, bỏ qua bước này (không ảnh hưởng thao tác qua `--rpc`).

***

### 7) Ký Add Validator (khi cần)

Chuẩn bị Ledger: mở khóa PIN, vào **app Avalanche**.

```bash
unset LEDGER_DEVICE
# Dùng trực tiếp RPC:
avalanche blockchain addValidator --rpc https://main.doschain.com

# Hoặc nếu đã import và đặt tên:
# avalanche blockchain addValidator --blockchain-name dos-main
```

Điền: **Mainnet**, BlockchainID (nếu chưa import), NodeID, Weight, Start/End time. Xác nhận ký trên Ledger.

***

### 8) Kiểm tra trạng thái

```bash
avalanche blockchain list
avalanche blockchain describe "TEN_BLOCKCHAIN"
avalanche blockchain validators "TEN_BLOCKCHAIN" --mainnet
avalanche blockchain stats "TEN_BLOCKCHAIN" --mainnet
```

***

### 9) Sau mỗi lần reboot Windows

```powershell
# PowerShell (Run as Administrator)
usbipd attach --wsl --busid <BUSID>
```

Rồi vào WSL xác nhận `lsusb | grep -i 2c97`.

***

### 10) Troubleshooting nhanh

* **WSL không thấy Ledger**
  * Windows: `usbipd list` → `usbipd attach --wsl --busid <BUSID>`
  * WSL: `lsusb | grep -i 2c97`, kiểm tra `/dev/hidraw*`
* **`hidapi: failed to open device`**
  * `systemd-udevd` phải running
  * `/dev/hidraw*` phải `root:plugdev` và `0660`
  * User thuộc `plugdev`
  * Đóng Ledger Live và ví khác trên Windows
  *   Rút cắm lại, rồi:

      ```powershell
      usbipd detach --busid <BUSID>
      usbipd attach --wsl  --busid <BUSID>
      ```
  *   Có thể bật log:

      ```bash
      LEDGER_LOG_LEVEL=debug avalanche blockchain addValidator --rpc https://main.doschain.com
      ```
* **`avalanche` không vào PATH**
  * `source ~/.bashrc`
  * Đảm bảo cuối file chỉ có **một** khối idempotent thêm `~/bin`.

***

### Phụ lục: Mẫu khối PATH idempotent cho `~/.bashrc`

```bash
# ensure ~/bin is in PATH (idempotent)
case ":$PATH:" in
  *":$HOME/bin:"*) ;;
  *) export PATH="$HOME/bin:$PATH" ;;
esac
```

***

**Ghi chú:** Không bắt buộc “import blockchain về local.” Dùng `--rpc` là thao tác được ngay. Import chỉ để đặt tên, lưu metadata cục bộ cho tiện dùng lại.
