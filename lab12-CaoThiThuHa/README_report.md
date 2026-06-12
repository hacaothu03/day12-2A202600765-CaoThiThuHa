# Báo Cáo: Tích Hợp và Triển Khai Bài Tập Lab 12

## 1. Giới thiệu chung
Trong bài thực hành Lab 12, mục tiêu là đóng gói và triển khai (deploy) sản phẩm **Coffeeholic** (từ bài mini hackathon Day 6) thông qua công cụ Docker. 

Báo cáo này tóm tắt toàn bộ các bước đã được thực hiện để tái cấu trúc source code, chuẩn bị dữ liệu và đóng gói Docker để sẵn sàng deploy lên các nền tảng serverless như Railway hoặc Render.

## 2. Các Công Việc Đã Thực Hiện

### 2.1. Cấu Trúc Lại Codebase
- Đã tạo thư mục `lab12-CaoThiThuHa` trong repository `day12-2A202600765-CaoThiThuHa`.
- Clone toàn bộ mã nguồn của dự án (cả frontend lẫn backend) từ bài tập cũ (Day 6) sang thư mục làm việc mới.
- Khởi tạo cấu trúc dữ liệu cho runtime (tạo thư mục `codebase/data` và copy dữ liệu `cafes.json` từ fixtures vào) để ứng dụng có thể lấy được dữ liệu ngay khi vừa chạy lên.

### 2.2. Tích hợp Backend và Frontend về 1 Server
Ban đầu ứng dụng chạy Frontend (trên cổng `5500`) và Backend (trên cổng `8000`) tách biệt. Để tiết kiệm tài nguyên khi deploy và chỉ cần sử dụng 1 container duy nhất:
- Sửa file `codebase/app/main.py` của **FastAPI**.
- Bổ sung cấu hình `StaticFiles` để backend kiêm luôn vai trò là một web server phục vụ file tĩnh cho thư mục `frontend`.
- Mount file `codebase/index.html` vào đường dẫn gốc (`/`) của API.
- Tự động thay thế Base URL trong file `app.js` để kết nối vào API tương ứng qua URL tương đối thay vì `127.0.0.1`.

### 2.3. Đóng gói ứng dụng với Docker (Dockerization)
Viết file `Dockerfile` nhằm đóng gói toàn bộ dự án vào một container chạy `python:3.11-slim`:
- Khai báo hệ điều hành base, cài đặt các extension cần thiết bằng `apt-get build-essential`.
- Khai báo `pip install` để tự động tải các gói trong `requirements.txt` (FastAPI, PyTorch, Transformers, v.v.).
- Khai báo môi trường `PYTHONPATH` và cổng `PORT=8000`.
- Chạy lệnh kích hoạt Uvicorn ở cuối file.
- Viết file `.dockerignore` nhằm loại bỏ những thư mục và file rác như `.git`, `__pycache__`, hay `.venv` ra khỏi build context.

### 2.4. Xử Lý Các Lỗi Phát Sinh (Bug Fixes)
- **Lỗi path trong codebase**: Đã sửa lại đường dẫn `project_dir` cho tương thích khi code được import sâu từ `app/main.py`.
- **Lỗi timeout khi build Docker**: Do thư viện `torch` có dung lượng cực kì lớn (hơn 800MB), quá trình build thường xuyên bị lỗi đứt mạng (`ReadTimeoutError`). Đã fix triệt để bằng cách gắn tham số `--default-timeout=1000` vào câu lệnh `pip install` trong Dockerfile.
- **Xóa conflict submodule**: Gỡ bỏ thư mục `.git` bị ẩn bên trong `lab12-CaoThiThuHa` (do copy đè từ dự án cũ) để tránh việc Git không track được code.

### 2.5. Commit & Khuyến nghị Deploy
- Đã lưu trữ toàn bộ lịch sử thay đổi và push lên repository gốc `hacaothu03/day12-2A202600765-CaoThiThuHa`.
- **Khuyến nghị**: Vì Railway hiện nay có thể yêu cầu gắn thẻ visa hoặc trả phí, ứng dụng này được khuyến nghị có thể đem deploy trực tiếp lên [Render.com](https://render.com) (chỉ cần chọn "Deploy from Github repo", cấp Root Directory là `/lab12-CaoThiThuHa`, chọn Docker environment là sẽ chạy thành công 100% miễn phí).

---
*Báo cáo được tạo tự động nhằm tổng kết các công việc kĩ thuật đã hoàn thiện.*
