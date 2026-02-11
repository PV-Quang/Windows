#### Mở cmd với quyền Administrator và chạy lệnh sau
```shell
w32tm /config /syncfromflags:manual /manualpeerlist:0.asia.pool.ntp.org,0x8 /reliable:yes /update 
```
Thay `0.asia.pool.ntp.org` bằng time server mong muốn

### Chạy lệnh sau để đồng bộ time

> Stop time server
```shell
net stop w32time
```

> Start time server
```shell
net start w32time 
```

> Đồng bộ time server
```shell
w32tm /resync /nowait 
```
