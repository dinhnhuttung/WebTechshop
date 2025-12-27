# WebTechshop (Techshop Plus with DocuSmart)

Dự án website thương mại điện tử sản phẩm công nghệ (Tech Shop) xây dựng bằng **Python + Flask + Jinja2 + SQLite**.  
Ngoài các chức năng mua hàng/giỏ hàng/đặt hàng, dự án tích hợp mô-đun **DocuSmart** hỗ trợ xử lý tài liệu: **OCR**, **trích xuất nội dung**, **phân loại**, **tóm tắt** và **trợ lý** (có sử dụng OpenAI API).

---

## Tính năng

### 1) Khách hàng
- Trang chủ, giới thiệu, liên hệ
- Danh sách sản phẩm, xem chi tiết sản phẩm
- Giỏ hàng
- Checkout/đặt hàng
- Theo dõi đơn hàng (tracking)

### 2) Quản trị (Admin)
- Đăng nhập admin
- Quản lý sản phẩm (CRUD)
- Quản lý đơn hàng + xem chi tiết đơn
- Quản lý banner
- Thống kê (stats)

### 3) DocuSmart / AI
- OCR và trích xuất nội dung tài liệu (PDF/ảnh)
- Phân loại tài liệu (AI Classifier)
- Tóm tắt tài liệu (AI Summarizer)
- Trợ lý (Assistant) / trang DocuSmart

> Gợi ý: các trang giao diện nằm trong `templates/` gồm:
`index, product_detail, cart, checkout, track, admin_login, admin_products, admin_orders, admin_banners, admin_stats, ai_classifier, ai_summarizer, assistant, docusmart...`

---

## Công nghệ sử dụng

- **Backend:** Python, Flask
- **Template Engine:** Jinja2
- **Database:** SQLite (`shop.db`)
- **ORM:** Flask-SQLAlchemy / SQLAlchemy
- **AI:** OpenAI SDK (cần API key)
- **Xử lý tài liệu & OCR:** pdf2image, pytesseract, OpenCV (cv2)
- **Dữ liệu/Excel:** pandas, openpyxl
- **Frontend:** HTML/CSS/JS (trong `static/`)

---

## Cấu trúc thư mục

```
techshop_plus_with_docusmart/
  app.py
  requirements.txt
  instance/
  shop.db
  seed_demo_shopdb.sql
  docusmart_core/
    __init__.py
    ocr_engine.py
    extractor.py
    document_processor.py
    classifier.py
  templates/
    *.html
  static/
    style.css
    app.js
    images/...
  tgdd_seed/ , tgdd_seed_100/
  scripts:
    crawl_tgdd_to_excel.py
    auto_import_bst_to_db.py
    import_db_btss.py
    reset_admin.py
    generate_demo_images.py
```

---

## Yêu cầu môi trường

- Python **3.10+** (khuyến nghị 3.11)
- pip
- (Khuyến nghị) tạo virtualenv riêng

### Nếu dùng tính năng OCR/PDF
Bạn cần cài thêm:

1) **Tesseract OCR**
- Windows: cài Tesseract và thêm vào PATH (hoặc cấu hình đường dẫn trong code nếu dự án có yêu cầu)
- macOS: `brew install tesseract`
- Ubuntu: `sudo apt install tesseract-ocr`

2) **Poppler** (để `pdf2image` chuyển PDF → ảnh)
- Windows: cài Poppler và thêm PATH
- macOS: `brew install poppler`
- Ubuntu: `sudo apt install poppler-utils`

---

## Cài đặt & chạy dự án

### 1) Giải nén & vào thư mục dự án
> Trong file rar có thể chứa `.venv/` — khuyến nghị **không dùng lại venv đóng gói sẵn**, hãy tạo mới để tránh lỗi môi trường.

```bash
cd techshop_plus_with_docusmart/techshop_plus_with_docusmart
```

### 2) Tạo môi trường ảo (khuyến nghị)
```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate
```

### 3) Cài dependencies
```bash
pip install -r requirements.txt
```

### 4) Cấu hình biến môi trường (OpenAI)
Tạo file `.env` (nếu dự án có đọc) hoặc export biến môi trường:

**Windows (PowerShell):**
```powershell
setx OPENAI_API_KEY "YOUR_API_KEY"
```

**macOS/Linux:**
```bash
export OPENAI_API_KEY="YOUR_API_KEY"
```

> Nếu không dùng tính năng AI, bạn có thể không cần API key (nhưng các trang AI sẽ không hoạt động).

### 5) Database
Dự án đã có sẵn SQLite:
- `shop.db` (database mẫu)

Nếu bạn muốn seed lại:
- file `seed_demo_shopdb.sql` (dùng khi cần khởi tạo dữ liệu demo)

> Tùy code trong `app.py`, database có thể nằm trong `instance/` hoặc cùng cấp.  
Nếu gặp lỗi “không tìm thấy DB”, hãy kiểm tra đường dẫn DB trong `app.py`.

### 6) Chạy project
```bash
python app.py
```

Mặc định Flask thường chạy tại:
- http://127.0.0.1:5000

---

## Tài khoản admin

Dự án có script:
- `reset_admin.py` — dùng để tạo/đặt lại tài khoản admin.

Bạn chạy:
```bash
python reset_admin.py
```

Sau đó đăng nhập tại trang Admin (thường là `/admin/login` hoặc link tương ứng trên giao diện).

> Vì không nên đoán mật khẩu mặc định, hãy xem output của script hoặc mở file `reset_admin.py` để biết chính xác thông tin tài khoản.

---

## Các chức năng & đường dẫn (tham khảo)

Tùy route định nghĩa trong `app.py`, nhưng thường sẽ có:

### User
- `/` : Trang chủ
- `/product/<id>` : Chi tiết sản phẩm
- `/cart` : Giỏ hàng
- `/checkout` : Thanh toán/đặt hàng
- `/track` : Theo dõi đơn hàng

### Admin
- `/admin/login`
- `/admin/products`
- `/admin/orders`
- `/admin/banners`
- `/admin/stats`

### DocuSmart / AI
- `/ai/summarizer`
- `/ai/classifier`
- `/assistant`
- `/docusmart`

> Nếu route khác, bạn chỉ cần search từ khóa `@app.route` trong `app.py`.

---

## Script tiện ích (nếu cần)

- `crawl_tgdd_to_excel.py`  
  Crawl dữ liệu sản phẩm (tgdd) ra Excel.
- `auto_import_bst_to_db.py` / `import_db_btss.py`  
  Import dữ liệu vào DB.
- `generate_demo_images.py`  
  Tạo dữ liệu/hình ảnh demo (nếu dùng).

---

## Troubleshooting

### 1) Lỗi OCR (pytesseract / tesseract not found)
- Cài Tesseract và đảm bảo `tesseract` nằm trong PATH.
- Trên Windows, có thể cần set đường dẫn trong code:
  ```py
  pytesseract.pytesseract.tesseract_cmd = r"C:\Program Files\Tesseract-OCR\tesseract.exe"
  ```

### 2) Lỗi pdf2image (poppler not installed)
- Cài Poppler và thêm PATH.
- Trên Windows, có thể phải truyền `poppler_path` khi convert PDF.

### 3) Lỗi OpenAI API key
- Đảm bảo đã set `OPENAI_API_KEY`
- Kiểm tra mạng, giới hạn quota, model name (nếu có cấu hình model).

---

## License
Dự án phục vụ học tập/đồ án. Bạn có thể bổ sung LICENSE nếu cần.
