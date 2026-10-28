# Hướng dẫn kích hoạt Windows Server Standard thông qua KMS Server

Tài liệu hướng dẫn cách cấu hình **KMS Client Key**, chỉ định **KMS Server** và thực hiện **Windows Activation** trên Windows Server Standard. Các thao tác được thực hiện trực tiếp trong Windows, không cần cài đặt thêm phần mềm.

**Phiên bản áp dụng:** Windows Server 2016, 2019, 2022 và 2025 **Standard Edition**.

> **Phạm vi:** Hướng dẫn dành cho máy đã cài **Windows Server Standard** nhưng chưa kích hoạt, hoặc cần cấu hình lại phương thức kích hoạt KMS. Nếu máy vẫn là **Windows Server Evaluation**, cần chuyển đổi Edition sang bản Standard trước; các lệnh bên dưới không thực hiện việc chuyển đổi Edition. Đế chuyển đổi từ Evaluation sang Standard xem hướng dẫn tại https://github.com/PV-Quang/Windows/blob/main/Convert_Windows-Evaluation_to_Windows-Standard.md

## 1. Chuẩn bị

- Đăng nhập Windows Server bằng tài khoản có quyền **Administrator**.
- Mở **Windows PowerShell** hoặc **Command Prompt** bằng quyền **Run as administrator**.
- Máy chủ cần có kết nối mạng đến KMS Server sẽ sử dụng.

Ví dụ dưới đây là màn hình **Activation** trên Windows Server 2022 Standard khi hệ điều hành chưa được kích hoạt.

![Windows Server 2022 Standard đang ở trạng thái chưa kích hoạt](Windows_Server_Standard_KMS_Huong_Dan_assets/01-windows-server-chua-kich-hoat.png)

*Hình 1. Windows Server Standard chưa được kích hoạt.*

## 2. Các bước thực hiện

### Bước 1. Cài đặt KMS Client Key

**KMS Client Key** (còn gọi là **Generic Volume License Key – GVLK**) giúp Windows Server sử dụng phương thức kích hoạt thông qua KMS. Đây là khóa dùng để cấu hình KMS Client, **không phải Product Key cấp bản quyền riêng cho máy chủ**.

Nếu Windows Server chưa được cấu hình KMS Client Key, hãy chạy **một** lệnh tương ứng với phiên bản đang sử dụng. Không chạy đồng thời các key của nhiều phiên bản.

**Windows Server 2025 Standard**

```powershell
slmgr /ipk TVRH6-WHNXV-R9WG3-9XRFY-MY832
```

**Windows Server 2022 Standard**

```powershell
slmgr /ipk VDYBN-27WPP-V4HQT-9VMD4-VMK7H
```

**Windows Server 2019 Standard**

```powershell
slmgr /ipk N69G4-B89J2-4G8F4-WWYCC-J464C
```

**Windows Server 2016 Standard**

```powershell
slmgr /ipk WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY
```

Trong đó, `slmgr /ipk` là lệnh cài đặt Product Key cho Windows. Sau khi cài thành công, máy chủ được cấu hình với KMS Client Key tương ứng. **Việc cài key không đồng nghĩa với việc Windows đã được kích hoạt thành công**.

Nếu máy đã có **đúng KMS Client Key** cho phiên bản Windows Server đang dùng thì có thể bỏ qua bước này và chuyển sang **Bước 2**. Việc màn hình Activation hiển thị Product ID hoặc Product Key không tự nó xác nhận máy đã có đúng KMS Client Key.

![Ví dụ màn hình Activation hiển thị thông tin Product ID và Product Key](Windows_Server_Standard_KMS_Huong_Dan_assets/02-man-hinh-product-key.png)

*Hình 2. Ví dụ thông tin Product ID và Product Key trên màn hình Activation.*

### Bước 2. Cấu hình KMS Server

Sau khi cài đặt KMS Client Key, cần chỉ định địa chỉ **KMS Server** để Windows biết nơi gửi yêu cầu kích hoạt.

Trong ví dụ này, sử dụng địa chỉ KMS Server sau:

```powershell
slmgr /skms kms.digiboy.ir
```

Ý nghĩa: `slmgr /skms` thiết lập địa chỉ KMS Server. Trong ví dụ này, `kms.digiboy.ir` là **KMS Server công cộng của bên thứ ba**, không phải máy chủ do Microsoft hoặc Cloud Provider của bạn xác nhận quản lý.

Nếu tổ chức có **KMS Server được cấp phép và quản lý hợp lệ**, nên thay địa chỉ trong lệnh bằng KMS Server của tổ chức.

![Ví dụ cấu hình KMS Server trên Windows Server 2025 Standard](Windows_Server_Standard_KMS_Huong_Dan_assets/03-cau-hinh-kms-server.png)

*Hình 3. Windows hiển thị thông báo sau khi cấu hình địa chỉ KMS Server.*

### Bước 3. Kích hoạt Windows

Sau khi cấu hình KMS Server, thực hiện lệnh sau để Windows gửi yêu cầu kích hoạt:

```powershell
slmgr /ato
```

Lệnh `slmgr /ato` yêu cầu Windows tiến hành **Activation** thông qua phương thức cấp phép đang được cấu hình. Nếu KMS Server phản hồi và chấp nhận yêu cầu, Windows sẽ được ghi nhận trạng thái kích hoạt.

Trên giao diện **Settings → Activation** (đường dẫn hiển thị có thể khác nhau giữa các phiên bản Windows Server), trạng thái có thể chuyển từ **Not active** sang **Active** như ví dụ bên dưới.

![Ví dụ Windows Server 2025 Standard hiển thị trạng thái Active](Windows_Server_Standard_KMS_Huong_Dan_assets/04-windows-server-da-kich-hoat.png)

*Hình 4. Ví dụ Windows Server hiển thị trạng thái Active sau khi kích hoạt.*

## 3. Lưu ý khi sử dụng

- Ba lệnh trong tài liệu có chức năng riêng: **`/ipk` cài KMS Client Key**, **`/skms` chỉ định KMS Server**, và **`/ato` gửi yêu cầu kích hoạt**. Chúng không phải lệnh chuyển Windows Server Evaluation sang Standard.
- Các KMS Client Key liệt kê ở Bước 1 là **GVLK**, không phải giấy phép Windows Server. Trạng thái **Active** không tự chứng minh tổ chức có quyền sử dụng phần mềm hợp lệ.
- `kms.digiboy.ir` là dịch vụ KMS công cộng bên thứ ba. Không nên áp dụng dịch vụ này cho **VM production hoặc VM cung cấp cho khách hàng** vì không kiểm soát được tính sẵn sàng, độ tin cậy và việc tuân thủ điều khoản cấp phép. Với môi trường Cloud Provider, cần dùng phương án cấp phép Windows Server hợp lệ.

