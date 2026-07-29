# Free-Proxylist

Kho lưu trữ danh sách SOCKS5 proxy miễn phí được cập nhật tự động mỗi 30 phút.

---

## Giới thiệu

**Free-Proxylist** là kho lưu trữ danh sách SOCKS5 proxy miễn phí được cập nhật tự động. Hệ thống chạy định kỳ mỗi 30 phút, thu thập và kiểm tra proxy SOCKS5 từ nhiều nguồn, sau đó lưu kết quả vào repository này.

> **Mục đích:** Cung cấp danh sách SOCKS5 proxy hoạt động, sẵn sàng để sử dụng ngay lập tức.

---

## Danh sách file

Hiện tại repository hỗ trợ:

| File | Mô tả | Định dạng | Trạng thái |
| :--- | :--- | :---: | :---: |
| `proxies_alive.txt` | Danh sách SOCKS5 proxy đang hoạt động | `ip:port` | Đã hỗ trợ |
| `proxies_http.txt` | HTTP proxy | `ip:port` | Sẽ cập nhật |
| `proxies_https.txt` | HTTPS proxy | `ip:port` | Sẽ cập nhật |
| `proxies_socks4.txt` | SOCKS4 proxy | `ip:port` | Sẽ cập nhật |
| `proxies.json` | Thông tin chi tiết | `JSON` | Sẽ cập nhật |

---

## Cách sử dụng

### Cách 1: Tải trực tiếp

```bash
# Tải danh sách SOCKS5 proxy
curl -O https://github.com/Hgbao209/Free-Proxylist/raw/refs/heads/main/proxies_alive.txt

# Hoặc dùng wget
wget https://github.com/Hgbao209/Free-Proxylist/raw/refs/heads/main/proxies_alive.txt

```
### Cách 2: Sử dụng trong code
#### Python
```python
import requests

# Lấy danh sách SOCKS5 proxy từ file cục bộ
with open('proxies_alive.txt', 'r') as f:
    proxies = f.read().strip().split('\n')

# Sử dụng proxy
for proxy in proxies:
    print(f'socks5://{proxy}')

```
#### Python (với requests + socks)
```bash
pip instll requests[socks]

```
```python
import random
import requests

# Lấy proxy ngẫu nhiên
proxies_list = open('proxies_alive.txt').read().splitlines()
proxy = random.choice(proxies_list)

# Cấu hình proxy
proxies = {
    'http': f'socks5://{proxy}',
    'https': f'socks5://{proxy}'
}

# Thực hiện request
response = requests.get('[https://api.ipify.org](https://api.ipify.org)', proxies=proxies)
print(f'IP của bạn qua proxy: {response.text}')

```
#### Node.js
```bash
npm install axios socks-proxy-agent

```
```javascript
const fs = require('fs');
const axios = require('axios');
const { SocksProxyAgent } = require('socks-proxy-agent');

// Đọc danh sách proxy từ file
const proxies = fs.readFileSync('proxies_alive.txt', 'utf-8').split('\n').filter(Boolean);
const proxy = proxies[0]; // Lấy proxy đầu tiên

// Tạo agent
const agent = new SocksProxyAgent(`socks5://${proxy}`);

// Sử dụng
axios.get('[https://api.ipify.org](https://api.ipify.org)', { httpAgent: agent })
  .then(response => console.log(response.data));

```
#### cURL
```bash
# Lấy proxy đầu tiên
PROXY=$(head -1 proxies_alive.txt)

# Sử dụng với cURL
curl -x socks5://$PROXY [https://api.ipify.org](https://api.ipify.org)

```
## Thống kê
| Thông tin | Chi tiết |
|---|---|
| **Tần suất cập nhật** | Mỗi 30 phút |
| **Số lượng proxy trung bình** | ~200-500 SOCKS5 proxy |
| **Tỷ lệ hoạt động** | ~80% |
| **Quốc gia** | Đa dạng (US, FR, DE, VN, ...) |
| **Giao thức** | SOCKS5 |
| **Tốc độ** | Đã kiểm tra response time |
## Ví dụ sử dụng
### 1. Kiểm tra IP qua proxy
```bash
#!/bin/bash
PROXY=$(head -1 proxies_alive.txt)
curl -x socks5://$PROXY [https://api.ipify.org](https://api.ipify.org)

```
### 2. Tự động xoay proxy trong Python
```python
import itertools
import time
import requests

with open('proxies_alive.txt', 'r') as f:
    proxies = f.read().splitlines()

proxy_cycle = itertools.cycle(proxies)

for i in range(10):
    proxy = next(proxy_cycle)
    try:
        response = requests.get(
            '[https://httpbin.org/ip](https://httpbin.org/ip)',
            proxies={'http': f'socks5://{proxy}', 'https': f'socks5://{proxy}'},
            timeout=5
        )
        print(f'Request {i+1}: {response.json()}')
    except Exception as e:
        print(f'Request {i+1}: Failed - {e}')
    time.sleep(1)

```
### 3. Kiểm tra proxy nhanh (Bash Script)
```bash
for proxy in $(head -10 proxies_alive.txt); do
    timeout 3 curl -x socks5://$proxy -s [https://google.com](https://google.com) > /dev/null
    if [ $? -eq 0 ]; then
        echo "[ALIVE] $proxy"
    else
        echo "[DEAD]  $proxy"
    fi
done

```
### 4. Sử dụng với Firefox / Chrome
> **Hướng dẫn thủ công:**
>  1. Truy cập **Preferences / Settings** -> **Network Settings**
>  2. Chọn **Manual proxy configuration**
>  3. Điền thông tin vào phần **SOCKS Host**: 139.28.240.201 - **Port**: 1082
>  4. Chọn tùy chọn **SOCKS v5**
> 
## Use cases phổ biến
### 1. Web Scraping
```python
from random import choice
import requests

proxies = open('proxies_alive.txt').read().splitlines()
proxy = choice(proxies)

response = requests.get(
    '[https://target-website.com](https://target-website.com)',
    proxies={'http': f'socks5://{proxy}', 'https': f'socks5://{proxy}'}
)

```
### 2. Bypass Geo-restriction
```bash
PROXY=$(head -1 proxies_alive.txt)
curl -x socks5://$PROXY [https://netflix.com](https://netflix.com)

```
### 3. Tăng tính ẩn danh
```python
import requests

proxy = 'socks5://139.28.240.201:1082'
session = requests.Session()
session.proxies = {'http': proxy, 'https': proxy}

```
## Dữ liệu mẫu
```text
139.28.240.201:1082
45.33.22.11:1080
103.152.112.162:1080
103.2.112.163:1080
139.28.240.202:1082
45.33.22.12:1080
103.152.112.164:1080

```
## Lưu ý quan trọng
 * Proxy có thể hết hạn bất cứ lúc nào.
 * Luôn tải/đọc file mới nhất để đảm bảo tỷ lệ sống của proxy.
 * Không sử dụng cho các mục đích vi phạm pháp luật.
 * Tốc độ kết nối có thể thay đổi tùy thuộc vào vị trí địa lý của server proxy.
 * Một số website có cơ chế chặn IP proxy public.
## Roadmap
 * [x] SOCKS5 proxy
 * [ ] HTTP proxy
 * [ ] HTTPS proxy
 * [ ] SOCKS4 proxy
 * [ ] JSON format
 * [ ] Phân loại theo quốc gia
## License
Dự án sử dụng MIT License.
<p align="center">
Maintained by <b>@Hgbao209</b> • Cập nhật mỗi 30 phút • SOCKS5 Proxy
</p>
