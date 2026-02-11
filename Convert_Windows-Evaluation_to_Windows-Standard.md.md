Windows Server có 2 edition là Standard và Datacenter. Mỗi Edition sẽ có 2 nhóm:
- 1 là Evaluation, cái mà mình vẫn tự hiểu là Trial nhưng bản chất là bản free 180 ngày thiếu tính năng. Ở bản này không chấp nhận activation key nào cả. Đặc điểm là thông tin của OS sẽ có thêm đuôi Evaluation. Có ISO riêng down miễn phí để cài bản này.
- 1 là Retail/Volume, là bản đầy đủ nhưng có trial, thường là 30 ~ 60 ngày nhưng full tính năng. Ở bản này cho phép nhập key để active như bình thường. Đặc điểm là thông tin của OS không có đuôi gì cả. Có ISO riêng và cần account để download về cài. Có nguồn ISO trên mạng

Bản ISO hay cài đặt KHÔNG PHẢI LÀ BẢN TRIAL. Và phải hiểu chính xác là bản Evaluation (free 180 ngày). Muốn dùng phải convert. Bản này ISO public trên trang của Microsoft

### WINDOWS SERVER 2019

> Chạy lệnh sau để convert
```shell
DISM /online /Set-Edition:ServerStandard /ProductKey:N69G4-B89J2-4G8F4-WWYCC-J464C /AcceptEula
```
> Sau khi chạy lệnh trên Windows sẽ restart, sau khi start lên chạy tiếp các lệnh sau
```shell
slmgr /skms kms.digiboy.ir
slmgr /ato
```

### WINDOWS SERVER 2022
> Chạy lệnh sau để convert
```shell
dism /online /set-edition:ServerStandard /productkey:VDYBN-27WPP-V4HQT-9VMD4-VMK7H /accepteula
```
> Sau khi chạy lệnh trên Windows sẽ restart, sau khi start lên chạy tiếp các lệnh sau
```shell
slmgr /skms kms.digiboy.ir
slmgr /ato
```

### WINDOWS SERVER 2025
> Chạy lệnh sau để convert
```shell
dism /online /set-edition:ServerStandard /productkey:TVRH6-WHNXV-R9WG3-9XRFY-MY832 /accepteula
```
> Sau khi chạy lệnh trên Windows sẽ restart, sau khi start lên chạy tiếp các lệnh sau
```shell
slmgr /skms kms.digiboy.ir
slmgr /ato
```
