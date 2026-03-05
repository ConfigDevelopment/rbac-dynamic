# 🔐 RBAC Dynamic – Phân quyền động

Hệ thống **RBAC (Role-Based Access Control) động** cho phép quản lý quyền người dùng một cách linh hoạt, có thể mở rộng theo **Module → Subsystem → Function → Command** mà không cần hard-code trong code.  
Quyền được lưu trong DB, admin có thể thêm mới hoặc chỉnh sửa trực tiếp mà không phải build lại ứng dụng.  

---

## 📑 Mục lục
- [Giới thiệu](#-giới-thiệu)
- [Cơ chế phân quyền động](#-cơ-chế-phân-quyền-động)
- [Ví dụ luồng phân quyền](#-ví-dụ-luồng-phân-quyền)
- [Ưu điểm](#-ưu-điểm)

---

## 📖 Giới thiệu

Trong hệ thống lớn (ERP, HRM, CRM…), số lượng chức năng và hành động rất nhiều.  
RBAC động giúp:
- Tách rời phần **định nghĩa quyền** khỏi source code.  
- Cho phép thêm mới **Function, Command, Permission** mà không cần build lại.  
- Hỗ trợ **đa chi nhánh** (Branch).  
- Vẫn tương thích với **ASP.NET Identity**.  

---

## ⚙️ Cơ chế phân quyền động

1. **Xác thực (Authentication)**  
   Người dùng đăng nhập qua `User (IdentityUser<Guid>)`.
   Thuật toán signature RS256
      Dùng 2 key
         Private key → ký
         Public key → verify
      Lệnh gen privatekey 
         openssl genrsa -out private.pem 2048
      Lệnh gen publickey
         openssl rsa -in private.pem -pubout -out public.pem
     Lệnh mở thư mục lấy key
         explorer .
3. **Phân quyền (Authorization)**  
   Khi người dùng gọi API hoặc truy cập UI:
   - Hệ thống lấy danh sách **Permission** từ:
     - Quyền trực tiếp (`UserPermission`)  
     - Quyền theo nhóm (`UserPermissionGroup`)  
   - Kiểm tra xem user có quyền với **Function + Command** trong **Branch** hay không.  

4. **Quản trị động**  
   - Có thể thêm mới `Subsystem`, `Module`, `Function`, `Command` mà không cần sửa code.  
   - Admin chỉ cần định nghĩa `Permission` và gán cho `PermissionGroup` hoặc `User`.  

---

## 📌 Ví dụ luồng phân quyền

- **Subsystem**: Kế toán  
- **Module**: Quản lý hóa đơn  
- **Function**: Danh sách hóa đơn (`FunctionCode = InvoiceList`)  
- **Command**: View, Create, Delete  
- **PermissionGroup**: "Nhân viên kế toán" có quyền `View`, `Create`  
- **User A** thuộc chi nhánh Hà Nội, nằm trong nhóm "Nhân viên kế toán"  

👉 Khi User A đăng nhập tại chi nhánh Hà Nội:  
- ✅ Có thể **xem** và **tạo mới** hóa đơn  
- ❌ Không thể **xóa** hóa đơn  

---

## 🚀 Ưu điểm

- **Linh hoạt**: Không cần hard-code quyền.  
- **Quản trị động**: Admin quản lý trực tiếp từ DB/UI.  
- **Đa chi nhánh**: Hỗ trợ quyền tách biệt theo chi nhánh.  
- **Tích hợp sẵn Identity**: Sử dụng ASP.NET Identity cho xác thực, RBAC cho phân quyền chi tiết.  

---
