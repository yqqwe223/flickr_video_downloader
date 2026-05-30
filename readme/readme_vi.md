# Flickr Video Downloader 📥

> Công cụ web nhẹ nhàng, tôn trọng quyền riêng tư, giúp bạn lưu video Flickr công khai về máy một cách đơn giản

## 👋 Vì sao mình tạo ra project này?

Thú thật nhé: đôi khi lướt Flickr, bạn bắt gặp một video thật sự hữu ích — hướng dẫn chụp ảnh, clip hậu trường, hay thậm chí là nội dung chính bạn đã tải lên và giờ muốn sao lưu. Nhưng tải nó về một cách đơn giản? Không phải lúc nào cũng dễ dàng.

Vậy nên mình viết công cụ này trước hết là cho chính mình dùng, rồi nghĩ: "Tại sao không chia sẻ cho anh em cùng dùng nhỉ?". Không tính năng thừa thãi, không theo dõi người dùng, không bắt đăng ký tài khoản. Chỉ cần dán link video Flickr công khai, bấm "Phân tích", và nếu video có thể truy cập, các tùy chọn tải về sẽ hiện ra. Vậy thôi.

Mọi xử lý đều diễn ra ở phía server: mình không ghi lại bạn tải gì, không lưu lịch sử, không thu thập dữ liệu cá nhân. Quyền riêng tư của bạn là của bạn.

## ✨ Công cụ này làm được gì thật sự?

- **Hỗ trợ các link Flickr phổ biến**: Album công khai, trang video người dùng, link chia sẻ trực tiếp — miễn là không ở chế độ riêng tư hoặc bảo vệ bằng mật khẩu
- **Hiển thị các chất lượng có sẵn**: Khi Flickr cung cấp nhiều độ phân giải (Original, High, Standard), bạn có thể chọn phiên bản muốn tải
- **Không cần đăng nhập**: Chỉ xử lý nội dung công khai; không bao giờ yêu cầu thông tin tài khoản Flickr của bạn
- **Giao diện sạch, tương thích mọi thiết bị**: Hiển thị tốt trên điện thoại, máy tính bảng và desktop mà không cần framework frontend nặng nề
- **Giới hạn tốc độ cơ bản**: Tự động hạn chế số request từ mỗi IP để tránh lạm dụng, giữ dịch vụ ổn định cho mọi người
- **Xử lý không chặn giao diện**: Ngay cả khi đang phân tích video dài, tab trình duyệt của bạn vẫn hoạt động bình thường, không bị đơ

## 🛠 Công nghệ đằng sau

| Lớp | Công nghệ sử dụng |
|-----|------------------|
| Backend | Python 3.11, Django 4.2 LTS |
| Parsing | httpx, lxml, regex để trích xuất metadata |
| Frontend | HTML5 ngữ nghĩa, CSS3 tối giản, JavaScript thuần |
| Triển khai | Gunicorn + Nginx, hỗ trợ Docker |
| Tiện ích | python-dotenv, django-ratelimit, whitenoise |

Không thư viện AI. Không gọi API bên ngoài "gửi báo cáo về nhà". Chỉ những request HTTP chuẩn mực và bộ phân tích HTML được viết cẩn thận — loại code mà bạn thực sự có thể đọc, hiểu và sửa đổi mà không đau đầu.

## 🚀 Chạy trên máy cá nhân

### Bạn cần chuẩn bị
- Python 3.10 trở lên
- pip + venv (hoặc virtualenv)
- Hiểu biết cơ bản về cấu trúc project Django

### Thiết lập môi trường phát triển

```bash
# Clone repository
git clone https://github.com/ten-nguoi-dung/flickr-downloader-vi.git
cd flickr-downloader-vi

# Tạo và kích hoạt virtual environment
python -m venv .venv
source .venv/bin/activate  # Linux/macOS
# hoặc .venv\Scripts\activate  # Windows

# Cài đặt dependencies
pip install -r requirements.txt

# Cấu hình biến môi trường
cp .env.example .env
# Sửa file .env với giá trị của bạn (SECRET_KEY, DEBUG, v.v.)

# Chạy migration và khởi động server phát triển
python manage.py migrate
python manage.py runserver
```

Sau đó mở `http://127.0.0.1:8000` trong trình duyệt.

### Lưu ý khi triển khai thực tế

- Đặt `DEBUG=False` và cấu hình đúng `ALLOWED_HOSTS` với domain của bạn
- Chạy đằng sau Nginx bằng Gunicorn (hoặc uWSGI nếu bạn thích)
- Bật HTTPS ở cấp độ proxy
- Thu thập file tĩnh: `python manage.py collectstatic`
- Nếu lưu lượng truy cập tăng, cân nhắc thêm Redis để cache

Ví dụ lệnh Gunicorn:
```bash
gunicorn config.wsgi:application \
  --bind 127.0.0.1:8000 \
  --workers 2 \
  --timeout 90
```

Nếu bạn tăng số worker, hãy theo dõi mức sử dụng bộ nhớ — việc phân tích video có thể hơi nặng một chút.

## 📋 Hướng dẫn sử dụng

1. Tìm một trang Flickr công khai có video (album, trang người dùng hoặc link được chia sẻ)
2. Sao chép URL và dán vào ô nhập liệu của công cụ
3. Nhấn "Phân tích" — backend sẽ trích xuất các stream video có sẵn
4. Nếu thành công, các nút tải xuống với nhãn độ phân giải sẽ xuất hiện
5. Chọn tùy chọn bạn muốn; file sẽ được tải về thông qua trình duyệt

> Lưu ý: Chỉ video có thể truy cập công khai mới hoạt động. Album riêng tư, nội dung chỉ dành cho bạn bè, video bảo vệ bằng mật khẩu hoặc nội dung bị giới hạn theo khu vực sẽ trả về lỗi. Đây là thiết kế có chủ đích — công cụ tôn trọng cài đặt quyền riêng tư của Flickr.

## ⚠️ Vui lòng đọc kỹ phần này

Công cụ này được phát triển **chỉ cho mục đích cá nhân và phi thương mại**. Ví dụ về cách dùng phù hợp:
- Sao lưu video mà chính bạn đã tải lên Flickr
- Lưu nội dung giáo dục hoặc tham khảo được chia sẻ công khai để học offline
- Nghiên cứu hoặc mục đích hỗ trợ tiếp cận trong khuôn khổ fair use

**Bạn có trách nhiệm**:
- Tuân thủ [Điều khoản Dịch vụ của Flickr](https://www.flickr.com/help/terms)
- Tôn trọng bản quyền và giấy phép Creative Commons của người sáng tạo nội dung
- Tuân theo luật pháp hiện hành tại khu vực của bạn về việc sao chép nội dung số

Mình không giám sát hoạt động tải xuống và không chịu trách nhiệm về việc sử dụng sai mục đích. Vui lòng không dùng công cụ này cho:
- Scraping hàng loạt hoặc thu thập nội dung tự động
- Phân phối lại tài liệu có bản quyền mà không có sự cho phép
- Vượt qua cài đặt quyền riêng tư hoặc kiểm soát truy cập
- Dịch vụ thương mại hoặc lưu trữ lại mà không có ủy quyền rõ ràng

Nếu bạn không chắc trường hợp sử dụng của mình có ổn không, thì khả năng cao là không ổn. Khi nghi ngờ, hãy hỏi người sáng tạo nội dung trước.

## 🤝 Muốn đóng góp không?

Tìm thấy lỗi? Nghĩ rằng bộ phân tích có thể hoạt động tốt hơn? Có ý tưởng cải thiện giao diện? Mọi đóng góp đều được hoan nghênh — không rào cản.

### Cách đóng góp
1. Fork repository và tạo branch tính năng (`git checkout -b fix/giao-dien-mobile`)
2. Thực hiện thay đổi bằng các commit nhỏ, logic, với message rõ ràng
3. Test trên máy local — đảm bảo các tính năng hiện có vẫn hoạt động bình thường
4. Mở Pull Request kèm mô tả ngắn gọn về những gì đã thay đổi và lý do

### Phong cách code
- Backend: Tuân thủ PEP 8, thêm type hints ở những nơi giúp code dễ đọc hơn
- Frontend: Giữ JavaScript ở mức tối thiểu; ưu tiên progressive enhancement thay vì framework nặng
- Commit: Dùng prefix theo quy ước (`feat:`, `fix:`, `docs:`, `chore:`, v.v.)

### Báo cáo lỗi
Khi báo bug, vui lòng cung cấp:
- URL Flickr liên quan (nếu có thể chia sẻ)
- Tên + phiên bản trình duyệt, hệ điều hành, loại thiết bị
- Hướng dẫn từng bước để tái hiện vấn đề
- Hành vi mong đợi so với hành vi thực tế

Ảnh chụp màn hình hoặc log console cũng rất hữu ích, đặc biệt với các vấn đề frontend.

## 🔧 Tùy chọn cấu hình

| Biến | Mục đích | Ví dụ |
|------|----------|---------|
| `DEBUG` | Bật/tắt chế độ debug của Django | `False` |
| `SECRET_KEY` | Khóa bảo mật Django | `chuoi-ngau-nhien-an-toan-cua-ban` |
| `MAX_VIDEO_SIZE_MB` | Từ chối file lớn hơn X MB | `500` |
| `RATE_LIMIT_PER_MIN` | Số request tối đa mỗi IP mỗi phút | `10` |
| `ALLOWED_HOSTS` | Các domain được phép (cách nhau bằng dấu phẩy) | `.tenmien-cua-ban.vn` |

Tất cả cài đặt được tải qua `python-dotenv`; không có thông tin nhạy cảm nào được hardcode trong source. Trong môi trường production, hãy xoay vòng `SECRET_KEY` định kỳ.

## 📄 Giấy phép

Giấy phép MIT — xem file [LICENSE](./LICENSE) để đọc toàn văn.  
Bạn được tự do sử dụng, sửa đổi và phân phối phần mềm này, miễn là giữ nguyên thông báo bản quyền gốc.

## 📬 Liên hệ & Hỗ trợ

- Báo lỗi & đề xuất tính năng: Dùng tab Issues trên GitHub
- Câu hỏi chung: support@twittervideodownloaderx.com
- Lỗ hổng bảo mật: Vui lòng gửi email trực tiếp trước khi công khai

Mình cố gắng phản hồi các issue trong vòng vài ngày. Nếu đã lâu hơn mà chưa nhận được trả lời, đừng ngại nhắn lại nhé — đôi khi có thứ bị sót lại.

---

*Project này không liên kết, không được xác nhận và không có mối quan hệ nào với Flickr / SmugMug, Inc. Tất cả nhãn hiệu, logo và quyền nội dung thuộc về chủ sở hữu tương ứng của chúng.*

*Cập nhật lần cuối: Tháng 5 năm  | Phiên bản 1.2.0*

*Demo trực tiếp: https://twittervideodownloaderx.com/flickr_downloader_vi*

*Được viết bởi một con người, dành cho con người. Không có trí tuệ nhân tạo nào tham gia vào việc viết README này hay mã nguồn của project.*