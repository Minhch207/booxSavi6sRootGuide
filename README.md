# Boox Savi 6s Root Guide
INITIAL DISCLAIMER: screwing this up could brick your device, do not attempt unless you thoroughly understand what is happening and why, please read all instructions top to bottom first so you know what to expect, always back up any and all of your important data before trying this\
LƯU Ý BAN ĐẦU: Làm sai thao tác này có thể làm hỏng thiết bị của bạn, đừng thử nếu bạn không hiểu rõ điều gì đang xảy ra và tại sao, vui lòng đọc kỹ tất cả hướng dẫn từ đầu đến cuối trước để biết bạn cần chuẩn bị những gì, luôn sao lưu tất cả dữ liệu quan trọng của bạn trước khi thử thao tác này.\
THANKS: Cảm ơn Oxgoru từ MobileRead.com vì đã cho biết việc root model này khả thi và jdkruzr người đã tạo bài [BooxPalma2RootGuide](https://github.com/jdkruzr/BooxPalma2RootGuide).
# Chuẩn bị
Cách này cho win nên sẽ cần
-  ["EDL" utility](https://www.temblast.com/edl.htm)
-  file [loader](https://www.temblast.com/download/poke6.bin) cho edl sử dụng
-  [Zadig](https://zadig.akeo.ie/) để tải driver cho edl.
-  [platform-tools](https://developer.android.com/tools/releases/platform-tools#downloads) để sử dụng lệnh adb.\

\Sau khi tải đầy đủ về, ta giải nén platform-tools. Mở powershell chạy để xem thiết bị đã nhận chưa, nhớ bật gỡ lỗi usb trên máy. Nếu chạy thành công thì sẽ hiện như này
```
PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> ./adb devices
List of devices attached
DAA2174F        device
```
Sau đó reboot vào `edl mode`. Nếu vào thành công thì sẽ như sau:
```
PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> .\adb.exe reboot edl
PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> .\adb.exe devices
List of devices attached

```
Tiếp, ta cài driver edl bằng [Zadig](https://zadig.akeo.ie/).
Nếu cài driver thành công thì nó sẽ hiện như sau:
```
PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> ./edl.exe
Found EDL 9008
```
# Bắt đầu
Ta lấy file boot_a của máy nếu hiện ok ở dưới cùng thì thành công.
```
PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> .\edl /lpoke6.bin /r /pboot_a boot_a.img
Found EDL 9008
<log value="ERROR: Failed to run the last command -1" />
Configuring... ok
Requesting info on LUN 0... ok
Requesting GPT 0 header... ok, receiving... ok, requesting entries... ok, receiving... ok
Requesting read boot_b.img... ok, reading 100% ok
```
lấy file boot_b 
```
PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> .\edl /r /pboot_b boot_b.img
Found EDL 9008
<log value="ERROR: Failed to run the last command -1" />
Configuring... ok
Requesting info on LUN 0... ok
Requesting GPT 0 header... ok, receiving... ok, requesting entries... ok, receiving... ok
Requesting read boot_b.img... ok, reading 100% ok
```
Sau đó, ta thoát edl mode. Và đẩy hai file boot ta vừa lấy được về máy boox của mình.
```
PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> .\edl /z
Found EDL 9008, requesting reboot... ok

PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> ./adb push boot_a.img /sdcard
>> ./adb push boot_b.img /sdcard
boot_a.img: 1 file pushed, 0 skipped. 298.6 MB/s (100663296 bytes in 0.321s)
boot_b.img: 1 file pushed, 0 skipped. 307.0 MB/s (100663296 bytes in 0.313s)
```
Trên máy boox của mình, ta tải [Magisk](https://github.com/topjohnwu/Magisk) về và patch cả hai file `boot_a` và `boot_b`. Patch xong, ta được hai file .img mới với chuỗi random. Đổi tên file hai file về dạng dễ phân biệt hơn rồi. Lại cắm máy vào pc rồi vào thư mục chứa platform-tools. Ta tiếp tục thực hiện các lệnh:
```
PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> ./adb devices
List of devices attached
DAA2174F        device
#Kiểm tra xem đã kết nối vào chưa như này là ok

PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> ./adb shell getprop ro.boot.slot_suffix
_a
#Cho biết là máy boot từ phân vùng a nên ta sẽ chỉ cần đẩy cái file phân vùng a đã patch bằng Magisk vào

PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> ./adb reboot edl
PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> ./edl /lpoke6.bin /w /pboot_a magisk_patched-30700_boot_a.img
Found EDL 9008, handshaking... version 2
HWID: 0014d0e100000000 (x3), JTAG: 0014d0e1, OEM: 0000, Model: 0000
Hash: d40eee56f3194665-574109a39267724a-e7944134cd53cb76-7e293d3c40497955-bc8a4519ff992b03-1fadc6355015ac87 (x3)
Sending poke6.bin 100% ok, starting... ok, waiting for Firehose... ok
Configuring... ok
Requesting info on LUN 0... ok
Requesting GPT 0 header... ok, receiving... ok, requesting entries... ok, receiving... ok
Requesting write magisk_patched-30700_boot_a.img... ok, writing 100% ok
PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> .\edl /z
Found EDL 9008, requesting reboot... ok
# đã đẩy file boot patch bằng magisk thành công và bây giờ khởi động lại.
PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> ./adb shell su -c id
uid=0(root) gid=0(root) groups=0(root) context=u:r:magisk:s0
# khởi động vào đc rồi dùng lệnh trên, nếu ra như này là có root
```
# Ngoài lề
Hai file `boot_a` và `boot_b` giống nhau hoàn toàn vì thế cả hai file sau khi patch bằng Magisk đều giống nhau
```
PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> dir *.img


    Directory: C:\Users\Minh Chau\Downloads\platform-tools-latest-windows


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----          6/1/2026  12:27 PM      100663296 boot_a.img
-a----          6/1/2026  12:29 PM      100663296 boot_b.img
-a----          6/1/2026  12:38 PM      100663296 magisk_patched-30700_boot_a.img
-a----          6/1/2026  12:40 PM      100663296 magisk_patched-30700_boot_b.img

PS C:\Users\Minh Chau\Downloads\platform-tools-latest-windows> Get-FileHash boot_a.img
>> Get-FileHash boot_b.img
>> Get-FileHash magisk_patched-30700_boot_a.img
>> Get-FileHash magisk_patched-30700_boot_b.imglatest-windows>

Algorithm       Hash                                                                   Path
---------       ----                                                                   ----
SHA256          B78859E716DF28B580E1AF335F28FC31239F8F8C901250FA23E4C0956CEE5B29       C:\Users\Minh Chau\Downloads\platform-tools-latest-windows\boot_a.img
SHA256          B78859E716DF28B580E1AF335F28FC31239F8F8C901250FA23E4C0956CEE5B29       C:\Users\Minh Chau\Downloads\platform-tools-latest-windows\boot_b.img
SHA256          5C94F1BE420E7F852E8D772197D16A1CB170DEDCF8F97EA139C76A0B5F811D44       C:\Users\Minh Chau\Downloads\platform-tools-latest-windows\magisk_patched-30700_boot_a.img
SHA256          5C94F1BE420E7F852E8D772197D16A1CB170DEDCF8F97EA139C76A0B5F811D44       C:\Users\Minh Chau\Downloads\platform-tools-latest-windows\magisk_patched-30700_boot_b.img
```
