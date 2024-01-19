# Follina Writeup BlueTeamLabsOnline
## **1. Giới thiệu challenge**
Đề bài cung cấp một file zip chứa mã độc để người chơi có thể tải về và phân tích, cùng với 9 câu hỏi liên quan:
- `Câu hỏi 1` Giá trị SHA1 của mẫu này là gì?
- `Câu hỏi 2` Theo VirusTotal, filetype đầy đủ của mẫu này là gì? 
- `Câu hỏi 3` Tìm URL được sử dụng trong mẫu này trong việc thực thi.
- `Câu hỏi 4` Tên của file chứa đoạn URL của câu hỏi 3 là gì?
- `Câu hỏi 5` URL được trích xuất truy cập vào tệp HTML kích hoạt lỗ hổng để thực thi payload độc hại. Theo các chức năng xử lý HTML, bất kỳ tệp nào có ít hơn <số> byte sẽ không gọi payload. Hãy tìm ra số byte này
- `Câu hỏi 6` Sau khi thực thi thì mẫu này sẽ cố gắng xóa một tiến trình nếu nó đang chạy. Tên của tiến trình là gì?
- `Câu hỏi 7` Chúng ta được giao công việc viết một quy tắc dựa trên tiến trình (process-based) sử dụng Windows Event ID 4688 nhằm phát hiện loại lỗ hổng này. Vậy ProcessName và ParentProcessName được sử dụng trong quy tắc này là gì?
- `Câu hỏi 8` Tìm ra ID kĩ thuật MITRE được sử dụng trong mẫu nhằm thực thi mã.
- `Câu hỏi 9` CVE của lỗ hổng này là gì?
### Một số thông tin tìm được từ lỗi Follina:
File `.doc` sử dụng tính năng `remote template` của Word để truy xuất tệp HTML từ máy chủ web, cách nhận biết là có dấu "!" ở cuối, sau đó sử dụng cơ chế `ms-msdt MSProtocol URI` để tải mã và thực thi một số lệnh Shell.

Lợi dụng công cụ Microsoft Support Diagnostic Tool (MSDT) - một công cụ cho phép các đại lý hỗ trợ kỹ thuật của Microsoft phân tích dữ liệu chẩn đoán từ xa cho mục đích khắc phục sự cố, tạo ra tiến trình con cmd.exe để thực thi mã độc.

`Lưu ý: sử dụng máy ảo và tắt Windows Defender của máy ảo đó để có thể thực thi và phân tích file.`
## **2. Quá trình**
### **Câu hỏi 1: Giá trị SHA1 của mẫu này là gì?**
Sử dụng hàm `Get-FileHash` có trong Powershell của Windows: ![](/Images/1.jpg)
### **Câu hỏi 2: Theo VirusTotal, filetype đầy đủ của mẫu này là gì?**
Truy cập vào trang Virustotal và tìm kiếm bằng chuỗi hash mới tìm được: ![](/Images/2.jpg)
### **Câu hỏi 3: Tìm URL được sử dụng trong mẫu này trong việc thực thi.**
Để có thể phân tích tiếp file này, ta sẽ cần chuyển dạng file sang XML bằng cách thay đổi đuôi `.doc` thành `.zip`, sau đó giải nén, sẽ ra một thư mục chứa các thành phần của file doc này dưới dạng XML. 
Nhờ vào dấu hiệu nhận biết là có dấu "!" ở cuối dòng, ta tìm ra được URL được sử dụng trong mẫu này: ![](/Images/3.jpg)
### **Câu hỏi 4: Tên của file chứa đoạn URL của câu hỏi 3 là gì?**
File đính URL ở câu 3 chính là đáp án. ![](/Images/4.jpg)
### **Câu hỏi 5: URL được trích xuất truy cập vào tệp HTML kích hoạt lỗ hổng để thực thi payload độc hại. Theo các chức năng xử lý HTML, bất kỳ tệp nào có ít hơn <số> byte sẽ không gọi payload. Hãy tìm ra số byte này**
Bởi vì tên miền của file này đã bị gỡ bỏ khỏi internet và không có cách nào truy cập được file `.html` này nên ta sẽ dựa vào write up về lỗi Follina của link sau: hxxps[://]www[.]huntress[.]com/blog/microsoft-office-remote-code-execution-follina-msdt-bug

Đáp án: 

![](/Images/9.jpg)
### **Câu hỏi 6: Sau khi thực thi thì mẫu này sẽ cố gắng xóa một tiến trình nếu nó đang chạy. Tên của tiến trình là gì?**

Cũng ở link writeup trên, ta sẽ tìm thấy đáp án: ![](/Images/5.jpg)
### **Câu hỏi 7: Chúng ta được giao công việc viết một quy tắc dựa trên tiến trình (process-based) sử dụng Windows Event ID 4688 nhằm phát hiện loại lỗ hổng này. Vậy ProcessName và ParentProcessName được sử dụng trong quy tắc này là gì?**
Ở một Writeup khác sẽ cung cấp cho ta biết rằng Follina sẽ tạo ra process cha và process con cụ thể. Đường link như sau: hxxps[://]logrhythm[.]com/blog/detecting-follina-cve-2022-30190-microsoft-office-zero-day-exploit/

Ta sẽ tìm thấy đáp án: ![](/Images/6.jpg)

### **Câu hỏi 8: Tìm ra ID kĩ thuật MITRE được sử dụng trong mẫu nhằm thực thi mã.**
Bởi vì công đoạn thực thi mã của lỗi này là sử dụng `cmd.exe`, ta sẽ tìm kiếm từ khóa trên MITRE ATT&CK liên quan đến `cmd execution`.

Đáp án: 

![](/Images/7.jpg)
### **Câu hỏi 9: CVE của lỗ hổng này là gì?**
Trên VirusTotal cũng đã có cung cấp CVE của lỗi này. ![](/Images/8.jpg)
## **3. Tài liệu tham khảo**
- hxxps[://]www[.]huntress[.]com/blog/microsoft-office-remote-code-execution-follina-msdt-bug
- hxxps[://]logrhythm[.]com/blog/detecting-follina-cve-2022-30190-microsoft-office-zero-day-exploit/
