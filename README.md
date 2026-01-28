# BÁO GIÁ DỰ ÁN GRS - HỆ THỐNG QUẢN LÝ TIỆN ÍCH GIS

**Kính gửi:** Anh Dương Văn Tuấn  
**Từ:** Lê Thái Anh - Founding Engineer | DNA TEAM
**Ngày:** 28/01/2026

---

## � MỤC LỤC - NHẢY NHANH

<div style="background: #f5f5f5; padding: 20px; border-radius: 8px; margin: 20px 0;">

### 🎯 Phần chính
- [⚠️ 3 Vấn đề anh sẽ gặp](#3-van-de)
- [💡 So sánh công nghệ](#so-sanh-cong-nghe)
- [📊 Bảng báo giá](#bang-bao-gia)
- [🎁 2 Gói lựa chọn](#2-goi-lua-chon)
- [💰 Tại sao không đắt](#tai-sao-khong-dat)
- [🤝 Cam kết & Timeline](#cam-ket)


</div>

---

<div id="3-van-de"></div>

## 🎯 3 VẤN ĐỀ ANH SẼ GẶP (NẾU LÀM SAI)

[⬆️ Về mục lục](#-mục-lục---nhảy-nhanh)

### Vấn đề 1: "Sao hóa đơn Google Maps tháng này 10 triệu?" ⚠️ QUAN TRỌNG NHẤT

**Giống như:** Quán cà phê dùng cốc giấy 500đ/cái. 100 khách × 20 cốc/ngày = 3 triệu/tháng. Mua cốc sứ 10 triệu (1 lần) → Tiết kiệm 36 triệu/năm.

**Thực tế GIS:**
- 100 users × 20 lần/ngày × 30 ngày = 60,000 map loads/tháng
- Google Maps API: $7/1000 loads = **10 triệu/tháng = 120 triệu/năm**

**Giải pháp:** Custom Map Engine + kvrock inverted index
- ✅ Zero API cost (OpenStreetMap free)
- ✅ Tìm kiếm nhanh gấp 100x MySQL
- ✅ Tối ưu cho đường ống (không phải giao thông)
- ✅ Offline support (field workers không sợ mất sóng)

**Trade-off:** Phải viết Polyline Algorithm, Clustering, Coordinate Transform (5-6 ngày).

**Chi phí:** 10tr (1 lần) vs 120tr/năm (mãi mãi).

---

### Vấn đề 2: "Sao data của Partner A lại hiện ở Partner B?"

**Giống như:** Kho hàng 3 khách thuê, chỉ dán nhãn A-B-C. Nhân viên quên check → Lấy nhầm hàng → Khách A kiện → Mất 50 triệu bồi thường.

**Thực tế GIS:**
- Anh có 3 đối tác: A, B, C
- Developer làm đơn giản: Chỉ thêm `partner_id` vào query
- Một chỗ quên check → **Data leak**

**Hậu quả:** Mất uy tín, kiện tụng, mất khách hàng.

**Giải pháp:** Multi-tenancy Architecture
- Middleware check tenant ở **mọi query**
- Không thể leak data (như xây tường ngăn kho)
- Phức tạp hơn, nhưng an toàn tuyệt đối

**Chi phí:** 3-4 ngày làm đúng vs 2-3 tuần sửa sau + mất khách.

---

### Vấn đề 3: "Sao 8h sáng hệ thống lag?"

**Giống như:** Quán phở 1 nồi, 100 khách 8h sáng. Khách cuối đợi 1.5 giờ → Bỏ đi. Chuẩn bị 5 nồi → Đợi 20 phút → Khách hài lòng.

**Thực tế GIS:**
- 100 users cùng login 8h sáng
- Mỗi người load: Map (2MB) + Data (500 rows) + WebSocket
- **Không tối ưu:** Server crash hoặc đợi 10-15s/user

**Giải pháp:**
- Preact (nhẹ hơn React 3KB)
- Code Splitting (load từng phần cần thiết)
- Virtual Scrolling (hiển thị 20 rows thay vì 500)
- Service Worker Cache (lưu map sẵn)

**Chi phí:** 3-4 ngày làm đúng vs Users complain + mất khách.

---

## � TẠI SAO KHÔNG THỂ DÙNG WORDPRESS HOẶC THUÊ TEAM MICROSERVICE?

### Câu chuyện 4: "Sao không dùng WordPress plugin?"

**Giống như:** Nhà hàng buffet mua đồ ăn sẵn siêu thị - nhanh, rẻ nhưng khách ăn chay/dị ứng không có gì ăn. Thuê đầu bếp riêng → Custom theo khách.

**WordPress plugin:**
- ❌ Không có Multi-tenancy (data leak)
- ❌ Phụ thuộc Google Maps (120tr/năm)
- ❌ Không tối ưu 100 users đồng thời
- ❌ Muốn custom → Chờ plugin update (hoặc không bao giờ)

**Custom system:**
- ✅ Multi-tenancy đúng chuẩn
- ✅ Custom Map Engine (zero cost)
- ✅ Tối ưu cho use case anh
- ✅ Thêm feature → Code ngay

---

<div id="tai-sao-khong-microservice"></div>

### Câu chuyện 5: "Sao không thuê team làm Microservice?"

**Giống như:** Đi HN → Hải Phòng (100km). Thuê 5 xe máy (mỗi xe chở 1 thứ) = 3 triệu, phức tạp, xe chậm → Mất đồ. Thuê 1 xe ô tô = 1 triệu, tất cả trong 1 xe, đơn giản.

**Microservice phù hợp:**
- 1 triệu users (Facebook, Google)
- Scale từng phần riêng
- Team 20-30 người

**Anh chỉ có 100 users:**
- Microservice = Overkill (giết gà dao mổ trâu)
- Chi phí cao (5 servers vs 1)
- Phức tạp không cần thiết

**Monolith (em làm):**
- ✅ 100-1000 users đủ
- ✅ 1 server rẻ
- ✅ Dễ maintain, không cần team lớn

---

<div id="kvrock-la-gi"></div>

### Câu chuyện 6: "kvrock + inverted index là gì? Tại sao quan trọng?"

**Giống như:** Thư viện 10,000 cuốn. Tìm "đường ống nước" - Lật từng cuốn = 2 giờ. Có mục lục → Tra ngay = 5 giây.

**kvrock inverted index:**
- ✅ Nhanh gấp 100x MySQL (2-3s → 0.1s)
- ✅ Fuzzy search (gõ "duong ong nuoc" vẫn ra)
- ✅ Tối ưu GIS data (tọa độ, metadata)

**Impact:**
- 10,000 điểm trên map
- MySQL: Query 2-3s → Lag
- kvrock: Query 0.1s → Mượt

**Chi phí:**
- MySQL: Server mạnh (t3.large) = 80$/tháng
- kvrock: Server nhỏ (t3.medium) = 40$/tháng
- **Tiết kiệm:** 12 triệu/năm

---

<div id="tai-sao-khong-kubernetes"></div>

### Câu chuyện 7: "Tại sao không cần Nomad/Kubernetes?"

**Giống như:** Quán cà phê 20 bàn thuê hệ thống khách sạn 5 sao (receptionist, concierge, housekeeping) = 50 triệu/tháng. Thuê 1 nhân viên = 8 triệu/tháng, đủ.

**Kubernetes phù hợp:**
- 1000 servers
- Auto-scale
- Team DevOps 5-10 người

**Anh chỉ có 1 server:**
- Kubernetes = Overkill
- Chi phí cao (thuê DevOps)
- Phức tạp không cần

**Monolith + kvrock:**
- ✅ 1 server đủ 100-1000 users
- ✅ Không cần DevOps team
- ✅ Chi phí thấp (40$/tháng)

---

<div id="so-sanh-cong-nghe"></div>

## 💡 TÓM LẠI: SỨC MẠNH THẦM LẶNG

[⬆️ Về mục lục](#-mục-lục---nhảy-nhanh)

Anh không cần **1 triệu users/giây**. Anh chỉ cần **100 users mượt mà**.

**Em không dùng công nghệ "sang chảnh" để flex.** Em dùng công nghệ **"vừa đủ, đúng chỗ"**:

| Công nghệ | Khi nào dùng? | Anh có cần không? | Ví dụ đời sống |
|:----------|:--------------|:------------------|:---------------|
| **WordPress** | Blog, landing page đơn giản | ❌ Không (cần Multi-tenancy) | Như mua đồ ăn sẵn siêu thị - nhanh nhưng không custom được |
| **Microservice** | 1 triệu users, team 20+ người | ❌ Không (chỉ 100 users) | Như thuê 5 xe máy đi Hải Phòng - tốn kém và phức tạp |
| **Kubernetes** | 1000 servers, auto-scale | ❌ Không (chỉ 1 server) | Như thuê hệ thống khách sạn 5 sao cho quán cà phê 20 bàn |
| **Monolith + kvrock** | 100-1000 users, 1 server, nhanh | ✅ **Đúng** | Như thuê 1 xe ô tô - vừa đủ, hiệu quả, tiết kiệm |

**Kết quả:**
- ✅ Nhanh (kvrock inverted index - như có mục lục thư viện)
- ✅ Rẻ (1 server thay vì 5 - như 1 xe thay vì 5 xe)
- ✅ Đơn giản (không cần team lớn - như 1 tài xế thay vì 5)
- ✅ Scale được (100 → 1000 users không cần refactor)

**Đây là "moat" của em.** Không phải ai cũng biết khi nào dùng công nghệ nào. Em biết.

---

Khi anh trả 25 triệu, anh không mua code. **Anh mua:**

| Giá trị | Mô tả | Ví dụ đời sống | Giá trị thực |
|:--------|:------|:---------------|:-------------|
| **1. Peace of mind** | Data không bao giờ leak | Như két sắt ngân hàng - an toàn tuyệt đối | Vô giá (tránh kiện tụng) |
| **2. Cost savings** | Tiết kiệm 120tr/năm Google Maps | Như mua cốc sứ thay vì cốc giấy | 120tr/năm |
| **3. Scalability** | Thêm 10 đối tác không cần refactor | Như xây nhà 3 tầng từ đầu, không phải đập xây lại | 50tr (chi phí refactor) |
| **4. Performance** | 100 users đồng thời không lag | Như quán phở có 5 nồi, không để khách đợi | Doanh thu tăng 50% |
| **5. Ownership** | Full source code, không phụ thuộc vendor | Như mua nhà thay vì thuê - sở hữu mãi mãi | Vô giá |

**Tổng giá trị thực:** 120tr/năm + 50tr + tăng doanh thu = **Hơn 200 triệu**

**Anh trả:** 25 triệu

**ROI:** 800% (chỉ tính năm đầu)

**Đây là Value-Based Pricing.** Em không tính theo giờ. Em tính theo **giá trị anh nhận được**.

---

<div id="bang-bao-gia"></div>

## 📊 BẢNG BÁO GIÁ MODULAR

[⬆️ Về mục lục](#-mục-lục---nhảy-nhanh)

Bây giờ em chia nhỏ để anh thấy rõ từng phần:

| # | Module | Giải quyết vấn đề gì? | Giá trị | Nếu không làm? | Ví dụ đời sống |
|:--|:-------|:----------------------|:--------|:---------------|:---------------|
| **1** | **Multi-tenancy Core** | Data không leak giữa partners | **8tr** | Kiện tụng, mất khách | Như xây tường ngăn giữa các phòng - không ai nhìn thấy đồ của nhau |
| **2** | **Custom Map Engine**<br>*Powered by kvrock inverted index* | Thoát Google Maps API<br>Tìm kiếm nhanh gấp 100x | **10tr** | Tốn 120tr/năm | Như có mục lục thư viện - tìm sách trong 5 giây thay vì 2 giờ |
| **3** | **Optimized Frontend** | 100 users không lag | **7tr** | Users complain, chậm | Như mở rộng cửa hàng - 100 khách vào cùng lúc không chen lấn |
| **4** | **Cloud Infrastructure** | Uptime 99.9%, bảo mật | **5tr** | Downtime, mất data | Như thuê bảo vệ 24/7 - cửa hàng luôn mở, không bị trộm |
| **5** | **Live Tracking + AI Agentic**<br>*Test tại twitch.tv/thaianh_python* | Alert tức thì, giảm training | **5tr** | Phát hiện sự cố chậm | Như có trợ lý thông minh - nhân viên hỏi gì cũng trả lời ngay |
| | **TỔNG** | | **35tr** | | |

---

<div id="2-goi-lua-chon"></div>

## 🎁 2 GÓI LỰA CHỌN

[⬆️ Về mục lục](#-mục-lục---nhảy-nhanh)

### Gói A: Foundation - **25 triệu** *(Có thể thương lượng)*
*(Giảm 10tr từ giá gốc 35tr - Vì anh là đối tác lâu dài)*

**Bao gồm:** Module 1, 2, 3, 4  
**Giải quyết:** 3 vấn đề lớn (Data leak, Google Maps cost, Performance)  
**Support:** 1 tháng

**Phù hợp:** Anh cần hệ thống ổn định, không đau đầu, tiết kiệm chi phí.

---

### Gói B: Advanced - **30 triệu** *(Có thể thương lượng)*
*(Giảm 5tr từ giá gốc 35tr)*

**Bao gồm:** Gói A + Module 5 + AI Assistant

**Thêm:**
- Live Tracking (WebSocket real-time)
- Email/Webhook Alerts
- **AI Agentic Bot** - Natural Language Query
  - *Ví dụ:* "Hiển thị ống nước Quận 1 áp suất thấp"
  - **Powered by DNA Platform's LLM engine**

**🎥 Test AI bot:** twitch.tv/thaianh_python → Gõ `!bot` trong chat

---

**Tại sao đáng 5tr thêm?**

**Giống như:** Nhà hàng training 10 nhân viên. Truyền thống = 40 giờ = 8 triệu. Có AI Assistant → Nhân viên tự hỏi → 12 giờ = 2.4 triệu. **Tiết kiệm 5.6 triệu.**

**Trong GIS:**
- Nhân viên hỏi: *"Làm sao tìm ống nước Quận 1?"*
- Bot trả lời: *"Vào menu Map → Filter → Chọn Quận 1"*
- Nhân viên hỏi: *"Sao ống này màu đỏ?"*
- Bot trả lời: *"Màu đỏ = áp suất thấp < 2 bar. Cần kiểm tra ngay."*

**Lợi ích:**
- Giảm 70% thời gian training (40 giờ → 12 giờ)
- Nhân viên ca đêm có AI support (anh không cần trực)
- Giảm số lần gọi điện hỏi anh
- **Payback:** Ngay lần training đầu tiên

**Support:** 3 tháng

**Phù hợp:** Nhiều nhân viên mới, có nhân viên ca đêm, muốn giảm training time.

---

<div id="tai-sao-khong-dat"></div>

## 💰 TẠI SAO 25 TRIỆU KHÔNG PHẢI "ĐẮT"?

[⬆️ Về mục lục](#-mục-lục---nhảy-nhanh)

### So sánh thực tế:

| Phương án | Giá | Rủi ro | Chi phí hàng năm | Ví dụ đời sống |
|:----------|:----|:-------|:-----------------|:---------------|
| **Agency** | 50-80tr | Không có source code | +20-30tr maintenance | Như thuê nhà - trả tiền mãi, không bao giờ sở hữu |
| **Freelancer rẻ** | 10-15tr | Data leak, lag, không support | +120tr Google Maps | Như mua xe cũ giá rẻ - sửa hoài, tốn xăng, cuối cùng tốn hơn xe mới |
| **Em làm** | **25tr** | ✅ Không | ✅ Tiết kiệm 120tr/năm | Như mua nhà trả góp - ban đầu hơi cao nhưng về lâu dài tiết kiệm |

**ROI:** Payback trong 2-3 tháng (chỉ tính Google Maps savings)

**Tính toán cụ thể:**
- Chi phí ban đầu: 25tr
- Tiết kiệm Google Maps: 10tr/tháng
- **Hoàn vốn sau:** 2.5 tháng
- **Lợi nhuận năm 1:** 120tr - 25tr = **95tr**

---

<div id="cam-ket"></div>

## 🤝 CAM KẾT CỦA EM

[⬆️ Về mục lục](#-mục-lục---nhảy-nhanh)

### Thanh toán:
- **Cọc 50%** khi bắt đầu → Thuê server, setup dev
- **50% còn lại** → Thương lượng sau (có thể chia nhỏ theo milestone hoặc khi bàn giao)

### Bàn giao:
- ⏱️ **6 tuần** (Phase 1: 3 tuần Core, Phase 2: 3 tuần Polish & Deploy)
  - *Tại sao 6 tuần?* Em còn maintain DNA Platform + Digital Twin Bot + working. Em không muốn rush và deliver chất lượng kém.
- 📦 **Full source code** (GitHub private)
- 📚 **Document** + Video demo
- 🛠️ **Support** 6 month

### Timeline chi tiết:

| Tuần | Công việc | Deliverable | Ví dụ đời sống |
|:-----|:----------|:------------|:---------------|
| **1-2** | Backend Core + Multi-tenancy + Database | API hoàn chỉnh, data an toàn | Như xây móng nhà - không thấy nhưng quan trọng nhất |
| **3-4** | GIS Map Engine + Custom Rendering | Map hiển thị, tìm kiếm nhanh | Như lắp cửa sổ - bắt đầu thấy hình dáng ngôi nhà |
| **5** | Frontend UI + Integration | Giao diện hoàn chỉnh, mượt mà | Như sơn tường, lắp đèn - nhà đẹp, sẵn sàng ở |
| **6** | Testing + Bug fixes + Deployment | Hệ thống live, sẵn sàng dùng | Như dọn dẹp, bàn giao chìa khóa |


### Đảm bảo:
- ✅ Làm trực tiếp 60%, teamsize 3 person (không outsource)
- ✅ Code quality cao (em đã build IaC SDK, DNA Platform)
- ✅ Support sau bàn giao (không bỏ rơi)

---

<div id="tai-sao-chon-em"></div>

## 🚀 TẠI SAO CHỌN TEAM?

[⬆️ Về mục lục](#-mục-lục---nhảy-nhanh)

**không phải developer thuê ngoài.** I and my teams là **Founding Engineers** với mindset:

1. **Hiểu business** - Không chỉ code, mà giải quyết vấn đề
2. **Cost-conscious** - Tối ưu chi phí cho anh (Custom Map thay Google)
3. **Long-term thinking** - Build để scale, không refactor sau
4. **Proven track record** - IaC SDK, DNA Platform, Digital Twin Bot

**Tech stack:** Python (Litestar), Preact, AWS, kvrock, LLM agents  
**Experience:** 7+ năm Backend/System Architecture

---

**Trân trọng,**  
**Lê Thái Anh and My Team Members**
