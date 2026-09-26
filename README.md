# Odoo Agent Harness: Phương Pháp Luận "Harness Engineering" Cho Kỹ Sư Phát Triển Odoo

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Odoo Version](https://img.shields.io/badge/Odoo-16%20|%2017%20|%2018%20|%2019-714B67.svg)](https://www.odoo.com)
[![Methodology](https://img.shields.io/badge/Methodology-Harness%20Engineering-darkgreen.svg)]()
[![Status](https://img.shields.io/badge/Status-Production%20Ready-success.svg)]()

> **Bộ khung thực hành kỹ thuật (Harness Engineering Playbook) & Starter Kit dành cho Kỹ sư Phát triển Odoo**  
> Chuyển đổi mô hình làm việc từ "gõ mã thủ công" sang "thiết kế hạ tầng giàn giáo (Harness) kiểm soát Coding Agent", hiện thực hóa tư duy **Solution First** và đáp ứng tiêu chuẩn khắt khe của thị trường Nhật Bản.

---

## 📖 Mục Lục
1. [Phương Pháp Luận: Harness Engineering Cho Coding Agent](#-phương-pháp-luận-harness-engineering-cho-coding-agent)
2. [Tư Duy Cốt Lõi: "Solution First" & Thách Thức Về Chất Lượng](#-tư-duy-cốt-lõi-solution-first--thách-thức-về-chất-lượng)
3. [Thành Phần 1: Cơ Chế Dẫn Đường Kỹ Thuật (Guides — Skills & Planning)](#-thành-phần-1-cơ-chế-dẫn-đường-kỹ-thuật-guides--skills--planning)
4. [Thành Phần 2: Cơ Quan Tương Tác Hệ Thống (Actuators — Góc Nhìn MCP Thực Chiến)](#-thành-phần-2-cơ-quan-tương-tác-hệ-thống-actuators--góc-nhìn-mcp-thực-chiến)
5. [Thành Phần 3: Cảm Biến & Vòng Lặp Kiểm Thử (Sensors & Feedback — Subagents)](#-thành-phần-3-cảm-biến--vòng-lặp-kiểm-thử-sensors--feedback--subagents)
6. [Cấu Trúc Thư Mục & Hướng Dẫn Triển Khai](#-cấu-trúc-thư-mục--hướng-dẫn-triển-khai)
7. [Lời Cảm Ơn & Bản Quyền Mã Nguồn Mở (Credits & License)](#-lời-cảm-ơn--bản-quyền-mã-nguồn-mở-credits--license)

---

## ⚙️ Phương Pháp Luận: Harness Engineering Cho Coding Agent

Trong kỹ thuật phần mềm hiện đại (được chuẩn hóa bởi **Martin Fowler** và cộng đồng kỹ nghệ tác tử), một Tác tử Tự hành không đơn thuần chỉ là một mô hình ngôn ngữ lớn (LLM). Sức mạnh và độ tin cậy của tác tử được định nghĩa qua phương trình:

$$\mathbf{Agent = Model + Harness}$$

* **Model (Bộ não suy luận)**: Các mô hình LLM (Gemini, Claude, GPT) cung cấp năng lực nhận thức và xử lý ngôn ngữ, nhưng vốn dĩ mang tính phi định thân (stateless), dễ ảo giác và bị cô lập trong "phòng kín".
* **Harness (Bộ khung cương / Hệ thống giàn giáo)**: Là **toàn bộ hạ tầng công kỹ thuật mang tính tiền định (Deterministic Infrastructure)** do kỹ sư thiết lập bao quanh Model, bao gồm: các quy tắc kiểm soát, công cụ truy cập dữ liệu thật, môi trường thực thi và các chốt chặn kiểm thử.

```
   ┌────────────────────────────────────────────────────────────────────────┐
   │                       THE AGENT HARNESS SYSTEM                         │
   │                                                                        │
   │   [GUIDES - Dẫn đường]                                                 │
   │   • Skills (.agents/skills)     ──► Ép tuân thủ ORM, no raw SQL        │
   │   • Reverse Interview (/grill)  ──► Bóc tách nghiệp vụ chuẩn Nhật      │
   │                                                                        │
   │   [ACTUATORS - Tương tác]       ┌──────────────────────────────────┐   │
   │   • PostgreSQL MCP              │          THE BRAIN               │   │
   │   • Chrome DevTools MCP         │            (LLM)                 │   │
   │   • Sequential Thinking         │     Khả năng suy luận thô        │   │
   │                                 └──────────────────────────────────┘   │
   │   [SENSORS - Kiểm định]                                                │
   │   • Role-play Debate            ──► Đấu trí phản biện kiến trúc        │
   │   • Independent Reviewer        ──► Rà soát ACL, N+1 query, i18n       │
   └────────────────────────────────────────────────────────────────────────┘
```

> **Bước chuyển dịch vai trò**: Lập trình viên không còn đóng vai trò người gõ phím sao chép mã nguồn từ AI, mà trở thành **Harness Engineer** — người thiết kế luật chơi, trang bị công cụ và thiết lập vòng phản hồi để tác tử hoạt động chuẩn xác và an toàn.

---

## 🧭 Tư Duy Cốt Lõi: "Solution First" & Thách Thức Về Chất Lượng

### 1. Nguyên lý "Solution First"
Kỹ sư phần mềm là **Kỹ sư giải pháp (Solution Engineer)** chứ không đơn thuần là **Lập trình viên tính năng (Feature Coder)**. Giá trị cốt lõi không nằm ở tốc độ sinh mã (LOC), mà ở việc giải quyết đúng bài toán nghiệp vụ với giải pháp bền vững và chi phí bảo trì thấp nhất.

### 2. Nguy cơ Nợ kỹ thuật (Technical Debt) khi thiếu Harness
Khi Coding Agent (như Antigravity, Claude Code, Cursor) được cấp quyền can thiệp thẳng vào hệ thống tệp và terminal, việc thiếu một Harness chặt chẽ sẽ gây ra hậu quả nghiêm trọng:
- **Tái phát minh bánh xe (Reinventing the wheel)**: Tác tử tự lập trình logic mới trong khi Odoo Core đã có sẵn tính năng (ví dụ: tự viết module phân cấp giá thay vì cấu hình `product.pricelist` tiêu chuẩn).
- **Phá vỡ kiến trúc Odoo**: Sinh mã raw SQL (`cr.execute()`), lạm dụng `sudo()` làm hở ranh giới đa công ty (Multi-company Isolation), hoặc thiếu hàm dịch thuật `_()` gây vỡ giao diện tiếng Nhật (`ja_JP`).

---

## 🧠 Thành Phần 1: Cơ Chế Dẫn Đường Kỹ Thuật (Guides — Skills & Planning)

Trong mô hình Harness, **Guides (Cơ chế Feedforward)** định hình hành vi của tác tử ngay từ trước khi một dòng mã nào được tạo ra.

### 1. Phỏng vấn đảo chiều (Reverse Interview Gate)
Trước khi lập trình, kỹ sư kích hoạt tác tử đóng vai Kiến trúc sư giải pháp để chất vấn ngược lại đặc tả kỹ thuật:
- Xác định mục đích thực sự của tính năng (báo cáo, thống kê hay tích hợp).
- Làm rõ ma trận phân quyền và các trường hợp ngoại lệ (Edge cases).
- Ngăn ngừa tình trạng hiểu sai spec của đối tác Nhật Bản.

### 2. Tiêu chuẩn hóa tri thức Odoo với `SKILL.md` (Progressive Disclosure)
Toàn bộ tri thức và quy chuẩn dự án được đóng gói thành các module Skill trong thư mục `.agent/skills/`. Nhờ cơ chế **Progressive Disclosure**, tác tử chỉ nạp chỉ dẫn chi tiết khi phát hiện ngữ cảnh phù hợp, tránh hiện tượng suy giảm độ chú ý (Attention Decay):
* **`odoo-workflow` (Nguyên tắc: *No citation, no code*)**: Mọi thao tác ghi đè phương thức (Override) hay kế thừa giao diện (XPath) bắt buộc phải trích dẫn chính xác `file:line` trong mã nguồn Odoo Base làm bằng chứng trước khi sinh mã.
* **`odoo-16.0` đến `odoo-19.0` Reference Packs**: Cung cấp tài liệu tra cứu API ORM, View, Controller và OWL framework tương thích chính xác theo từng phiên bản Odoo.

---

## 🔌 Thành Phần 2: Cơ Quan Tương Tác Hệ Thống (Actuators — Góc Nhìn MCP Thực Chiến)

Mô hình LLM thuần túy không có giác quan vật lý. Thông qua giao thức **Model Context Protocol (MCP)**, Harness trang bị cho tác tử các "cơ quan hành động" để tương tác trực tiếp với dữ liệu thực tế.

Mỗi dự án có hạ tầng công cụ riêng, dưới đây là **4 MCP hữu ích được đúc kết từ kinh nghiệm thực tế của tác giả** nhằm phục vụ phát triển Odoo:

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

1. **Sequential Thinking**: Hỗ trợ phân tích đa bước, so sánh trade-off giữa việc cấu hình Odoo Standard vs Viết module custom.
2. **Parallel Search / Web Fetch**: Tra cứu nhanh giải pháp từ kho module mã nguồn mở OCA và Odoo Official Docs.
3. **MCP Toolbox for Databases (PostgreSQL)**: Kết nối CSDL môi trường phát triển để đối soát cấu trúc bảng, khóa ngoại và chỉ mục thực tế.
4. **Chrome DevTools MCP**: Tự động hóa điều hướng trình duyệt, kiểm thử giao diện người dùng và bắt lỗi JavaScript/OWL tại Console.

👉 Chi tiết hướng dẫn và file cấu hình tham khảo tại: [`mcp-configs/README.md`](mcp-configs/README.md).

---

## 👥 Thành Phần 3: Cảm Biến & Vòng Lặp Kiểm Thử (Sensors & Feedback — Subagents)

Trong mô hình Harness, **Sensors (Cơ chế Feedback)** đóng vai trò giám sát, đo lường và thẩm định kết quả đầu ra của tác tử, kích hoạt vòng lặp tự sửa lỗi (Self-healing Loop) trước khi chuyển giao mã nguồn cho lập trình viên.

### 1. Nguyên lý Phân lập Ngữ cảnh (Context Isolation)
Thay vì nhồi nhét toàn bộ lịch sử trao đổi vào một cửa sổ ngữ cảnh duy nhất gây loãng thông tin, hệ thống điều phối các **Subagents** hoạt động độc lập với vai trò và bộ nhớ riêng biệt.

### 2. Hai mô hình phối hợp chuẩn mực:

#### 🔹 Mô hình 1: Phản biện Kiến trúc Đối lập (Role-play Debate)
* **Tác tử Đề xuất (Solution Proponent)**: Đưa ra giải pháp kỹ thuật đáp ứng yêu cầu nghiệp vụ.
* **Tác tử Thẩm định (System & Performance Critic)**: Phân tích rủi ro hệ thống (khóa bảng DB, lỗi timeout khi dữ liệu lớn, rủi ro migration).
* **Vòng lặp**: Hai tác tử tự tranh biện để loại bỏ điểm mù kiến trúc, giúp kỹ sư lựa chọn phương án tối ưu nhất.

#### 🔹 Mô hình 2: Chốt Kiểm Định Độc Lập (Independent Reviewer Gate)
* **Tác tử Triển khai (Coder)**: Thực hiện chuyển đổi đặc tả thành mã nguồn.
* **Tác tử Đánh giá (Reviewer)**: Được cô lập hoàn toàn với quá trình viết mã, chỉ tiếp nhận Git Diff và bộ tiêu chí kiểm định:
  - *Bảo mật*: Kiểm tra khai báo quyền trong `ir.model.access.csv`, rà soát `sudo()` tránh rò rỉ dữ liệu đa công ty.
  - *Hiệu năng*: Bắt lỗi truy vấn N+1 (N+1 Query Problem) trong các vòng lặp xử lý dữ liệu.
  - *Bản địa hóa*: Đảm bảo 100% chuỗi ký tự hiển thị được bọc qua hàm `_()` cho ngôn ngữ Nhật Bản.
* **Feedback Loop**: Coder buộc phải tự sửa mã đến khi Reviewer xác nhận đạt toàn bộ tiêu chuẩn.

👉 Chi tiết các mẫu System Prompt tại: [`prompts/README.md`](prompts/README.md).

---

## 📂 Cấu Trúc Thư Mục & Hướng Dẫn Triển Khai

```text
odoo-coding-agent-playbook/
├── README.md                  # Tài liệu phương pháp luận Harness Engineering
├── LICENSE                    # Giấy phép mã nguồn mở MIT
├── .agents/
│   └── skills/                # Gói tri thức Guides (Odoo 16-19, Workflow, Code-review)
│       ├── odoo-workflow/
│       ├── odoo-18.0/
│       ├── code-review/
│       └── ...
├── mcp-configs/               # Cấu hình Actuators (PostgreSQL, Chrome DevTools, Thinking)
│   └── README.md
└── prompts/                   # Kịch bản Sensors (Prompts cho Subagents Debate & Review)
    └── README.md
```

### Quy trình áp dụng vào dự án Odoo:
1. Sao chép thư mục `.agents/skills/` (hoặc `.agent/skills/`) vào thư mục gốc của repository dự án Odoo.
2. Cấu hình các công cụ MCP phù hợp với môi trường làm việc theo `mcp-configs/README.md`.
3. Khi tiếp nhận yêu cầu, sử dụng các prompt tại `prompts/README.md` để khởi động chu trình: **Làm rõ (Clarify) ➔ Lập kiến trúc (Trace) ➔ Thực thi có kiểm định (Dual Review)**.

---

## 🙏 Lời Cảm Ơn & Bản Quyền Mã Nguồn Mở (Credits & License)

Dự án này được phát hành dưới giấy phép mã nguồn mở **[MIT License](LICENSE)**.

### Ghi nhận đóng góp (Acknowledgments):
- Phương pháp luận **Harness Engineering** dựa trên các nghiên cứu kỹ thuật của **Martin Fowler** (Thoughtworks).
- Bộ kỹ năng Odoo và công cụ kiểm thử `odoo-workflow` xuất sắc từ tác giả **[unclecatvn/agent-skills](https://github.com/unclecatvn/agent-skills)**.
- Chuẩn giao thức **Model Context Protocol (MCP)** do **Anthropic** khởi xướng.
- Tinh thần kiến tạo giải pháp bền vững (*Solution First*) của tập thể kỹ sư tại **TGL Solutions**.
