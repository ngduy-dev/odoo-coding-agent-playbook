# Odoo Coding Agent Playbook: Từ "Solution First" Đến Bộ Ba Công Cụ Skills, MCP và Subagents

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Odoo Version](https://img.shields.io/badge/Odoo-16%20|%2017%20|%2018%20|%2019-714B67.svg)](https://www.odoo.com)
[![Status](https://img.shields.io/badge/Status-Production%20Ready-success.svg)]()

> **Kho tri thức và bộ công cụ mẫu (Starter Kit) dành cho Kỹ sư Phát triển Odoo**  
> Định hình phương pháp cộng tác với thế hệ Tác tử Tự hành (Autonomous Coding Agents: Antigravity, Claude Code, Cursor, Windsurf) dựa trên tư duy **Solution First** và tiêu chuẩn kỹ thuật thị trường Nhật Bản.

---

## 📖 Mục Lục
1. [Bối Cảnh & Thách Thức Kỹ Thuật](#-bối-cảnh--thách-thức-kỹ-thuật)
2. [Khía Cạnh 1: Quy Chuẩn & Tri Thức Kỹ Thuật (Skills)](#-khía-cạnh-1-quy-chuẩn--tri-thức-kỹ-thuật-skills)
3. [Khía Cạnh 2: Tích Hợp Hệ Thống Thực Tế (Bộ Tứ MCP)](#-khía-cạnh-2-tích-hợp-hệ-thống-thực-tế-bộ-tứ-mcp)
4. [Khía Cạnh 3: Tác Tử Phân Vai Chuyên Biệt (Subagents)](#-khía-cạnh-3-tác-tử-phân-vai-chuyên-biệt-subagents)
5. [Cấu Trúc Thư Mục & Hướng Dẫn Sử Dụng](#-cấu-trúc-thư-mục--hướng-dẫn-sử-dụng)
6. [Lời Cảm Ơn & Bản Quyền Mã Nguồn Mở (Credits & License)](#-lời-cảm-ơn--bản-quyền-mã-nguồn-mở)

---

## 🧭 Bối Cảnh & Thách Thức Kỹ Thuật

### 1. Nguyên lý "Solution First"
Trong phát triển ERP, định vị của kỹ sư là **Solution Engineer (Kỹ sư giải pháp)** thay vì chỉ là **Feature Coder (Lập trình viên chức năng)**. Giá trị phần mềm không đo bằng số dòng mã (LOC), mà đo bằng việc thấu hiểu bài toán nghiệp vụ để chọn ra giải pháp bền vững nhất.

### 2. Nguy cơ Nợ kỹ thuật (Technical Debt) từ Coding Agent
Sự trỗi dậy của Coding Agent (được cấp quyền truy cập Terminal, File System) giúp việc sinh mã diễn ra tức thì. Tuy nhiên, nếu thiếu cơ chế kiểm soát:
- **Tái phát minh bánh xe**: Tự lập trình logic mới trong khi Odoo đã có tính năng Standard (ví dụ: tự custom phân loại giá thay vì dùng `product.pricelist`).
- **Vi phạm tiêu chuẩn**: Sử dụng raw SQL (`cr.execute()`), lạm dụng `sudo()` làm hở ranh giới đa công ty, hoặc quên bọc chuỗi hiển thị trong hàm `_()` cho thị trường Nhật (`ja_JP`).

---

## 🧠 Khía Cạnh 1: Quy Chuẩn & Tri Thức Kỹ Thuật (Skills)

Repo này tích hợp sẵn bộ sưu tập `.agent/skills/` chuẩn hóa theo tiêu chuẩn công nghiệp:

* **Pre-implementation Gate**: Áp dụng kỹ thuật *Phỏng vấn đảo chiều (Reverse Interview)* để chất vấn lại spec của khách hàng trước khi sinh mã.
* **`odoo-workflow` (Nguyên tắc: *No citation, no code*)**: Mọi thao tác override hay xpath đều bắt buộc phải trích dẫn chính xác `file:line` trong base Odoo trước khi viết code.
* **`odoo-16.0` đến `odoo-19.0` Reference Packs**: Cung cấp tài liệu tra cứu API theo từng phiên bản Odoo cụ thể.

---

## 🔌 Khía Cạnh 2: Tích Hợp Hệ Thống Thực Tế — Góc Nhìn Về MCP (Model Context Protocol)

Mỗi dự án và mỗi kỹ sư sẽ có một nhu cầu công cụ khác nhau. MCP không phải là một tiêu chuẩn bắt buộc phải cài đặt toàn bộ, mà là một **giao thức mở linh hoạt** giúp tác tử kết nối với các nguồn dữ liệu bên ngoài khi cần thiết.

Dưới đây là **4 MCP thực tế mà tác giả thường xuyên sử dụng và thấy mang lại hiệu quả cao nhất** trong quy trình phát triển Odoo hàng ngày để anh em tham khảo và tùy biến theo nhu cầu riêng:

```
   ┌────────────────────────────────────────────────────────────────────────┐
   │            4 MCP THAM KHẢO HỮU ÍCH TRONG CÔNG VIỆC THỰC TẾ             │
   │                                                                        │
   │  [Sequential Thinking] ──► Hỗ trợ phân tích logic đa bước & rủi ro     │
   │           │                                                            │
   │  [Parallel Search]     ──► Tra cứu tài liệu kỹ thuật Odoo & OCA nhanh  │
   │           │                                                            │
   │  [Toolbox Databases]   ──► Đối soát lược đồ CSDL PostgreSQL cục bộ     │
   │           │                                                            │
   │  [Chrome DevTools]     ──► Hỗ trợ kiểm thử và bắt lỗi giao diện OWL    │
   └────────────────────────────────────────────────────────────────────────┘
```

👉 Xem hướng dẫn tham khảo và cấu hình mẫu tại: [`mcp-configs/README.md`](mcp-configs/README.md).

---

## 👥 Khía Cạnh 3: Tác Tử Phân Vai Chuyên Biệt (Subagents)

Giải quyết hiện tượng *Attention Decay (Suy giảm độ chú ý do phình ngữ cảnh)* thông qua cơ chế phân lập ngữ cảnh (Context Isolation):

1. **Mô hình Phản biện Đối lập (Role-play Debate)**: Hai Subagent (Solution Advocate vs System Critic) tranh luận để sàng lọc giải pháp tối ưu.
2. **Mô hình Coder - Reviewer Độc lập**: Tác tử kiểm tra chất lượng (KCS) độc lập rà soát bảo mật (ACL), hiệu năng (N+1 query) và ngôn ngữ tiếng Nhật trước khi commit.

👉 Xem chi tiết system prompts tại thư mục: [`prompts/README.md`](prompts/README.md).

---

## 📂 Cấu Trúc Thư Mục & Hướng Dẫn Sử Dụng

```text
odoo-coding-agent-playbook/
├── README.md                  # Tài liệu tổng quan & Hướng dẫn chính
├── LICENSE                    # Giấy phép mã nguồn mở MIT
├── .agent/
│   └── skills/                # Toàn bộ gói Skills Odoo 16-19, Workflow, Code-review
│       ├── odoo-workflow/
│       ├── odoo-18.0/
│       ├── code-review/
│       └── ...
├── mcp-configs/               # File cấu hình mẫu cho PostgreSQL, DevTools, Sequential Thinking
│   └── README.md
└── prompts/                   # System Prompts chuẩn hóa cho Subagents
    └── README.md
```

### Cách áp dụng nhanh vào dự án Odoo của bạn:
1. Sao chép thư mục `.agent/skills/` vào thư mục gốc (root) của dự án Odoo.
2. Cấu hình MCP server theo hướng dẫn trong `mcp-configs/README.md`.
3. Khi nhận task, áp dụng prompt từ `prompts/README.md` để khởi động quy trình làm việc chuẩn.

---

## 🙏 Lời Cảm Ơn & Bản Quyền Mã Nguồn Mở (Credits & License)

Dự án này được phát hành dưới giấy phép mã nguồn mở **[MIT License](LICENSE)**.

### Ghi nhận đóng góp (Acknowledgments):
- Chân thành cảm ơn tác giả **[unclecatvn/agent-skills](https://github.com/unclecatvn/agent-skills)** vì đã đóng góp bộ kỹ năng Odoo và quy trình kiểm thử `odoo-workflow` xuất sắc cho cộng đồng mã nguồn mở.
- Chuẩn giao thức **Model Context Protocol (MCP)** do **Anthropic** khởi xướng.
- Đội ngũ kỹ sư tại **TGL Solutions** với tinh thần kiến tạo giải pháp ERP bền vững (*Solution First*).
