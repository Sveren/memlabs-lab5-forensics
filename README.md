# 🔍 MemLabs Lab 5 "Black Tuesday" — Memory Forensics & Detection Engineering

> Bài tập thực hành CTF Jeopardy (An toàn và an ninh mạng)
> Trường Đại học Công nghệ, ĐHQGHN | GVHD: TS. Nguyễn Đại Thọ

## 📌 Giới thiệu

Dự án tái dựng và ứng phó sự cố an ninh mạng từ bằng chứng bộ nhớ (memory forensics), dựa trên challenge **MemLabs Lab 5 – "Black Tuesday"** (https://github.com/stuxnet999/MemLabs), được đóng khung theo mô hình ứng phó sự cố **NIST SP 800-61**, kết hợp với việc viết các luật phát hiện chủ động (YARA / Sigma / Suricata) nhằm phát hiện lại các kiểu tấn công tương tự trong tương lai.

## 🎯 Mục tiêu

- Điều tra memory dump Windows, xác định toàn bộ 3 flag của challenge
- Tái dựng chuỗi tấn công (kill chain), ánh xạ MITRE ATT&CK
- Chuyển hóa kết quả điều tra thành công cụ phát hiện có thể tái sử dụng

## 🗂️ Cấu trúc thư mục

\`\`\`
.
├── logs/           # Nhật ký làm việc theo từng ngày
├── evidence/       # Output thô từ Volatility, hash bằng chứng, screenshot
├── scripts/        # build_timeline.py — script dựng timeline hợp nhất
├── rules/
│   ├── lab5_iocs.yar              # YARA rule
│   ├── sigma/notepad_masquerading.yml   # Sigma rule
│   └── suricata/lab5_detect.rules    # Suricata rule
└── report/         # Báo cáo đầy đủ (.docx & .pdf)
\`\`\`

## 🛠️ Công cụ sử dụng

Volatility 2.6 · YARA · Sigma (pySigma) · Suricata 6.0.4 · John the Ripper · Python 3 · VirtualBox + Ubuntu 22.04 LTS

## 🚩 Kết quả

| Flag | Giá trị |
|---|---|
| Stage 1 | `flag{!!_w3LL_d0n3_St4g3-1_0f_L4B_5_D0n3_!!}` |
| Stage 2 | `flag{W1th_th1s_$taGe_2_1s_c0mPL3T3!!}` |
| Stage 3 | `bi0s{M3m_l4b5_OVeR_!}` |

## 🔗 Phát hiện nổi bật

- Kỹ thuật **Process Masquerading** (MITRE T1036.005): file `NOTEPAD.EXE` giả mạo đặt tại `C:\Users\SmartNet\Videos\` thay vì `System32`
- Tài khoản `Administrator` không đặt mật khẩu; mật khẩu user `Alissa Simpson` bị crack tức thời (`goodmorningindia`)

## 🛡️ Bộ luật phát hiện tự viết

- **YARA**: `Suspicious_Base64_Filename` (T1027), `Notepad_Masquerading_Location` (T1036.005)
- **Sigma**: phát hiện notepad.exe sai vị trí, đã kiểm chứng chuyển đổi sang Splunk SPL
- **Suricata**: rule minh họa phát hiện traffic tải file khả nghi

## 📄 Báo cáo đầy đủ

Xem chi tiết tại [`report/BaoCao_DoAn_CTF_Forensics.docx`]([./report/BaoCao_DoAn_CTF_Forensics.docx](https://docs.google.com/document/d/1aw_LBLuwP2259MhRPN5J8xplHgY7rkXZZpffw5wJRHg/edit?usp=sharing))

## ⚠️ Lưu ý về học thuật:

Dự án được thực hiện độc lập, có tham khảo có kiểm soát và trích dẫn đầy đủ các writeup công khai (xem Mục Tài liệu tham khảo trong báo cáo). File memory dump gốc **không** được đính kèm trong repo này (dung lượng lớn, không phải tài sản của tác giả repo) — tải tại [MemLabs gốc](https://github.com/stuxnet999/MemLabs).

## 📚 Nguồn tham khảo

- P. A. Kumar, *MemLabs: Memory Forensics Labs*, GitHub, team bi0s
- NIST SP 800-61 Rev.2 — Computer Security Incident Handling Guide
- MITRE ATT&CK — T1036.005, T1027
