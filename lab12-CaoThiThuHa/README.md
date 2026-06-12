## Thông tin nhóm

| MSSV | Họ và tên |
| --- | --- |
| 2A202600765 | Cao Thị Thu Hà |
| 2A202600709 | Hà Trung Kiên |
| 2A202600873 | Nguyễn Lâm Phương Thảo |
| 2A202600689 | Nguyễn Bình Huy |
# Coffeeholic

Coffeeholic là prototype gợi ý quán cafe theo phong cách không gian bằng hình ảnh. Người dùng chọn các quán mẫu đúng "vibe", backend dùng embedding ảnh và `Cosine Similarity` để trả ra các quán có không gian gần nhất.

## Cấu trúc repo

- `README.md`: tài liệu tổng quan của dự án.
- `spec/`: toàn bộ tài liệu đặc tả, slide và hình minh họa.
- `codebase/`: toàn bộ mã nguồn backend, frontend, scripts, tests và dữ liệu runtime cục bộ.

## Cách chạy dự án

Chạy trong thư mục `codebase/`.

### 1. Cài môi trường

```powershell
cd codebase
python -m venv .venv
.venv\Scripts\Activate.ps1
pip install -r requirements.txt
Copy-Item .env.example .env
```

### 2. Chuẩn bị dữ liệu

Nếu chưa có `codebase/data/cafes.json`, chạy:

```powershell
python scripts/supabase_cafes.py
python scripts/preprocess_cafes.py
python scripts/validate_data.py
```

### 3. Chạy backend

```powershell
uvicorn app.main:app --reload
```

Backend mặc định chạy tại `http://127.0.0.1:8000`.

### 4. Chạy frontend

Mở terminal mới, vẫn ở trong `codebase/`, rồi chạy:

```powershell
python -m http.server 5500
```

Sau đó mở:

```text
http://127.0.0.1:5500
```

Frontend mặc định gọi API ở `http://127.0.0.1:8000`.

## Tài liệu trong `spec/`

- `spec/spec_A2_coffeeholic.md`: đặc tả sản phẩm chính
- `spec/slide_Coffeeholic.pdf`: slide demo
- `spec/product_canvas_coffeeholicc.png`: AI Product Canvas
- `spec/coffeeholic_path.png`: sơ đồ luồng trải nghiệm
