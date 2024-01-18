# Follina-Write-Up---BlueTeamLabsOnline
## **1.Giới thiệu challenge**
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

`Lưu ý: sử dụng máy ảo và tắt Windows Defender của máy ảo đó để có thể thực thi và phân tích file.`
## **2.Quá trình**
### **Question 1: What is the SHA1 hash value of the sample? (Format: SHA1Hash)**

Sử dụng hàm `Get-Hash` có trong Powershell của Windows
