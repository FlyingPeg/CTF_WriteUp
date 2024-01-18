# Follina-Write-Up---BlueTeamLabsOnline
## **1. Giới thiệu challenge**
**Ngữ cảnh**

- On a Friday evening when you were in a mood to celebrate your weekend, your team was alerted with a new RCE vulnerability actively being exploited in the wild.You have been tasked with analyzing and researching the sample to collect information for the weekend team.

Đề bài sau đó cung cấp một file zip chứa mã độc để người chơi có thể tải về và phân tích, cùng với 9 câu hỏi liên quan:
- `Question 1` What is the SHA1 hash value of the sample? (Format: SHA1Hash)
- `Question 2` According to VirusTotal, what is the full filetype of the provided sample? (Format: X X X X)
- `Question 3` Extract the URL that is used within the sample and submit it (Format: https://x.domain.tld/path/to/something)
- `Question 4` What is the name of the XML file that is storing the extracted URL? (Format: file.name.ext)
- `Question 5` The extracted URL accesses a HTML file that triggers the vulnerability to execute a malicious payload. According to the HTML processing functions, any files with fewer than <Number> bytes would not invoke the payload. Submit the <Number> (Format: Number of Bytes)
- `Question 6` After execution, the sample will try to kill a process if it is already running. What is the name of this process? (Format: filename.ext)
- `Question 7` You were asked to write a process-based detection rule using Windows Event ID 4688. What would be the ProcessName and ParentProcessname used in this detection rule? [Hint: OSINT time!] (Format: ProcessName, ParentProcessName)
- `Question 8` Submit the MITRE technique ID used by the sample for Execution [Hint: Online sandbox platforms can help!] (Format: TXXXX)
- `Question 9` Submit the CVE associated with the vulnerability that is being exploited (Format: CVE-XXXX-XXXXX)
### Một số thông tin tìm được từ lỗi Follina:
File `.doc` sử dụng tính năng `remote template` của Word để truy xuất tệp HTML từ máy chủ web, cách nhận biết là có dấu "!" ở cuối, sau đó sử dụng cơ chế `ms-msdt MSProtocol URI` để tải mã và thực thi một số lệnh Shell.

Lợi dụng công cụ Microsoft Support Diagnostic Tool (MSDT) - một công cụ cho phép các đại lý hỗ trợ kỹ thuật của Microsoft phân tích dữ liệu chẩn đoán từ xa cho mục đích khắc phục sự cố, tạo ra tiến trình con cmd.exe để thực thi mã độc.

`Lưu ý: sử dụng máy ảo và tắt Windows Defender của máy ảo đó để có thể thực thi và phân tích file.`
## **2. Quá trình**
### **Question 1: What is the SHA1 hash value of the sample? (Format: SHA1Hash)**
Sử dụng hàm `Get-FileHash` có trong Powershell của Windows: ![](/Images/1.jpg)
### **Question 2: According to VirusTotal, what is the full filetype of the provided sample? (Format: X X X X)**
Truy cập vào trang Virustotal và tìm kiếm bằng chuỗi hash mới tìm được: ![](/Images/2.jpg)
### **Question 3: Extract the URL that is used within the sample and submit it (Format: https://x.domain.tld/path/to/something)**
Để có thể phân tích tiếp file này, ta sẽ cần chuyển dạng file sang XML bằng cách thay đổi đuôi `.doc` thành `.zip`, sau đó giải nén, sẽ ra một thư mục chứa các thành phần của file doc này dưới dạng XML. 
Nhờ vào dấu hiệu nhận biết là có dấu "!" ở cuối dòng, ta tìm ra được URL được sử dụng trong mẫu này: ![](/Images/3.jpg)
### **Question 4: What is the name of the XML file that is storing the extracted URL? (Format: file.name.ext)**
File đính URL ở câu 3 chính là đáp án. ![](/Images/4.jpg)
### **Question 5: The extracted URL accesses a HTML file that triggers the vulnerability to execute a malicious payload. According to the HTML processing functions, any files with fewer than <Number> bytes would not invoke the payload. Submit the <Number> (Format: Number of Bytes)**
Tạm thời chưa giải được câu này.
### **Question 6: After execution, the sample will try to kill a process if it is already running. What is the name of this process? (Format: filename.ext)**
Bởi vì tên miền của file này đã bị gỡ bỏ khỏi internet nên ta sẽ dựa vào write up về lỗi Follina của link sau: https://www.huntress.com/blog/microsoft-office-remote-code-execution-follina-msdt-bug

Ta sẽ tìm thấy đáp án: ![](/Images/5.jpg)
### **Question 7: You were asked to write a process-based detection rule using Windows Event ID 4688. What would be the ProcessName and ParentProcessname used in this detection rule? [Hint: OSINT time!] (Format: ProcessName, ParentProcessName)**
Ở một Writeup khác sẽ cung cấp cho ta biết rằng Follina sẽ tạo ra process cha và process con cụ thể. Đường link như sau: https://logrhythm.com/blog/detecting-follina-cve-2022-30190-microsoft-office-zero-day-exploit/

Ta sẽ tìm thấy đáp án: ![](/Images/6.jpg)

### **Question 8: Submit the MITRE technique ID used by the sample for Execution [Hint: Online sandbox platforms can help!] (Format: TXXXX)**
Bởi vì công đoạn thực thi mã của lỗi này là sử dụng `cmd.exe`, ta sẽ tìm kiếm từ khóa trên MITRE ATT&CK liên quan đến `cmd execution`.
Đáp án: ![](/Images/7.jpg)
### **Question 9: Submit the CVE associated with the vulnerability that is being exploited (Format: CVE-XXXX-XXXXX)**
Trên VirusTotal cũng đã có cung cấp CVE của lỗi này. ![](/Images/8.jpg)
