# Hướng dẫn host trang Cập nhật trên GitHub Pages

Thư mục này (`update_site/`) chứa nội dung cần đưa lên GitHub Pages để app
"Quản lý Quỹ Xã hội" có thể tự kiểm tra phiên bản mới. Thư mục này **không**
được đóng gói vào file .exe.

## Bước 1 — Tạo repo GitHub mới

1. Đăng nhập github.com → bấm **New repository**.
2. Đặt tên, ví dụ: `QuanLyQuyXH-update`.
3. Chọn **Public** (bắt buộc để GitHub Pages dùng được miễn phí).
4. Tạo repo (không cần thêm README/license khi tạo).

## Bước 2 — Đẩy nội dung `update_site/` lên repo

Mở terminal tại thư mục `update_site/` rồi chạy (thay `<username>` và `<repo>`):

```bash
git init
git add .
git commit -m "Khoi tao trang cap nhat"
git branch -M main
git remote add origin https://github.com/luandiepueh/<QuanLyQuyXH>.git
git push -u origin main
```

## Bước 3 — Bật GitHub Pages

1. Vào repo trên github.com → **Settings** → **Pages**.
2. Mục **Source**: chọn branch `main`, thư mục `/ (root)`.
3. Lưu lại. Sau ít phút, trang sẽ chạy tại:
   `https://<username>.github.io/<repo>/`
   và file manifest tại:
   `https://<username>.github.io/<repo>/version.json`

## Bước 4 — Cập nhật URL trong code

Mở `update_checker.py` (thư mục gốc dự án), sửa dòng:

```python
UPDATE_MANIFEST_URL = "https://<TODO-username>.github.io/<TODO-repo>/version.json"
```

thành URL `version.json` thật ở Bước 3. Build lại exe (`pyinstaller DTL_QuyXH.spec`).

## Bước 5 — Quy trình ra bản cập nhật mới (ví dụ F1.0.2)

1. Sửa `APP_VERSION = "F1.0.1"` thành `"F1.0.2"` trong `update_checker.py`.
2. Build exe mới: `pyinstaller DTL_QuyXH.spec` → ra `dist/QuanLyQuyXH.exe`.
3. Nén file `QuanLyQuyXH.exe` thành `QuanLyQuyXH_F1.0.2.zip`.
4. Trên github.com vào repo → **Releases** → **Draft a new release**:
   - Tag: `F1.0.2`
   - Đính kèm file `QuanLyQuyXH_F1.0.2.zip`
   - Publish release.
5. Sửa `update_site/version.json`:
   ```json
   {
     "version": "F1.0.2",
     "release_date": "2026-xx-xx",
     "download_url": "https://github.com/<username>/<repo>/releases/download/F1.0.2/QuanLyQuyXH_F1.0.2.zip",
     "changelog": ["Mô tả thay đổi trong bản F1.0.2"]
   }
   ```
6. Commit + push thư mục `update_site/` (chỉ cần `version.json` thay đổi):
   ```bash
   git add version.json
   git commit -m "Cap nhat phien ban F1.0.2"
   git push
   ```

Sau vài phút, app cũ (F1.0.1) bấm "Kiểm tra cập nhật" sẽ thấy bản F1.0.2 và
có thể tải về cài đặt tự động.
