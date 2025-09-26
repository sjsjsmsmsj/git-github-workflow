# Git Workflow - Hướng dẫn chuẩn khi làm việc với Git

## 1. Khi bạn là **chủ dự án**

### **Bước 1: Tạo dự án và kết nối GitHub**
Có 2 cách khởi tạo repository:

#### **Cách 1: Tạo repo trên GitHub trước**
1. Tạo repository trên GitHub.
2. Clone về máy:
   ```bash
   git clone <url_repo>
   ```

#### **Cách 2: Có sẵn dự án local, push lên GitHub**
1. Tạo Git repository:
   ```bash
   git init
   ```
2. Kết nối với GitHub:
   ```bash
   git remote add origin <url_repo>
   ```
3. Push lên GitHub:
   ```bash
   git push -u origin main
   ```

---

### **Bước 2: Làm việc trên branch riêng (Không push trực tiếp lên main/master)**

> Ví dụ: muốn thêm navbar, tạo branch tên `add_navbar`.

```bash
git checkout -b add_navbar
```

> Hoặc tương đương:
```bash
git branch -b add_navbar
```

---

### **Bước 3: Đẩy code lên GitHub**
1. **Thêm tất cả thay đổi:**
   ```bash
   git add .
   ```
2. **Kiểm tra trạng thái:**
   ```bash
   git status
   ```
3. **Commit với thông điệp rõ ràng:**
   ```bash
   git commit -m "Thêm tính năng navbar"
   ```
4. **Push lên GitHub:**
   ```bash
   git push origin add_navbar
   ```

---

### **Bước 4: Tạo Pull Request (PR)**
- Mở GitHub, chọn tab **Pull Requests** > **Create Pull Request**.
- Mục đích: yêu cầu merge vào `main`/`master` và để team review code.

---

### **Bước 5: Review & Update commit**

> Trong team thường chỉ **1 commit trên mỗi Pull Request**.

Nếu bị review yêu cầu sửa code:
1. Sửa code.
2. Update commit cuối cùng (ghi đè commit cũ):
   ```bash
   git add .
   git commit --amend -m "Sửa lỗi navbar và update UI"
   ```
3. Push với `--force`:
   ```bash
   git push origin add_navbar -f
   ```

---

### **Bước 6: Merge & dọn dẹp**
1. Sau khi review **OK**, merge code vào `main/master`.
2. Xóa branch cũ:
   ```bash
   git branch -D add_navbar
   ```
3. Checkout về nhánh `main/master`:
   ```bash
   git checkout main
   ```
4. Cập nhật code mới nhất từ GitHub:
   ```bash
   git pull origin main
   ```

---

## 2. Khi bạn là **nhân viên** (làm việc với dự án công ty)

### **Bước 1: Fork và setup dự án**
1. **Fork dự án công ty** về GitHub cá nhân.
2. Clone về máy:
   ```bash
   git clone <url_repo_cua_ban>
   ```
3. Thêm remote của công ty để cập nhật code gốc:
   ```bash
   git remote add company <url_repo_cong_ty>
   ```
4. Kiểm tra các remote:
   ```bash
   git remote -v
   ```

| Thành phần     | Ý nghĩa                                   |
|----------------|------------------------------------------|
| `git remote`   | Liệt kê các remote hiện tại              |
| `-v`           | Hiện chi tiết URL và quyền fetch/push    |

---

### **Bước 2: Tạo nhánh để phát triển tính năng**
```bash
git checkout -b add_navbar
```

---

### **Bước 3: Commit và push code**
1. Thêm tất cả thay đổi:
   ```bash
   git add .
   ```
2. Kiểm tra trạng thái:
   ```bash
   git status
   ```
3. Commit:
   ```bash
   git commit -m "Thêm tính năng navbar"
   ```
4. Push lên **repo của bạn** (fork):
   ```bash
   git push origin add_navbar
   ```

> **Lưu ý:** Luôn kiểm tra đúng remote trước khi push:
> ```bash
> git remote -v
> ```

---

### **Bước 4: Tạo Pull Request lên repo công ty**
- Truy cập repository của công ty.
- Tạo **Pull Request** từ branch của bạn sang branch `main`/`master` của công ty.

---

### **Bước 5: Update commit nếu bị yêu cầu sửa code**
1. Sửa code theo review.
2. Ghi đè commit cuối cùng:
   ```bash
   git add .
   git commit --amend -m "Sửa lỗi navbar và tối ưu code"
   ```
3. Force push:
   ```bash
   git push origin add_navbar -f
   ```

---

### **Bước 6: Đồng bộ với dự án công ty**
1. Checkout về `main`:
   ```bash
   git checkout main
   ```
2. Lấy code mới nhất từ repo công ty:
   ```bash
   git pull company main
   ```
3. Xóa branch cũ:
   ```bash
   git branch -D add_navbar
   ```

---

## **Git Workflow Chuẩn**
```
main
│
├── feature/add_navbar
│
├── feature/fix_bug_login
│
└── feature/update_ui
```

- `main` → nhánh chính, chứa code đã ổn định.
- `feature/*` → mỗi tính năng/mỗi task tạo một nhánh riêng.

---

## **Tóm tắt lệnh Git quan trọng**
| Lệnh | Mục đích |
|------|----------|
| `git init` | Khởi tạo repo local |
| `git clone <url>` | Clone repo từ GitHub |
| `git checkout -b <branch>` | Tạo và chuyển sang branch mới |
| `git add .` | Thêm tất cả thay đổi |
| `git status` | Kiểm tra trạng thái file |
| `git commit -m "msg"` | Commit với message |
| `git push origin <branch>` | Push code lên branch |
| `git pull origin main` | Lấy code mới từ GitHub |
| `git branch -D <branch>` | Xóa branch local |
| `git remote -v` | Kiểm tra remote URL |
| `git commit --amend` | Sửa commit cuối cùng |
| `git push -f` | Force push ghi đè lịch sử |

---

## **Best Practices**
- Không commit trực tiếp vào `main` hoặc `master`.
- Mỗi tính năng/bugfix → 1 branch riêng.
- Commit message ngắn gọn, rõ ràng, theo chuẩn:
  - `feat: thêm navbar`
  - `fix: sửa bug login`
  - `refactor: tối ưu code UI`
- Luôn đồng bộ với repo công ty trước khi bắt đầu task mới.
- Review kỹ trước khi merge để tránh lỗi production.
