# Odoo Agent Harness: Khung Phương Pháp Luận "Harness Engineering" Dành Cho Odoo Software Engineers

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Odoo Version](https://img.shields.io/badge/Odoo-16%20|%2017%20|%2018%20|%2019-714B67.svg)](https://www.odoo.com)
[![Methodology](https://img.shields.io/badge/Methodology-Harness%20Engineering-darkgreen.svg)]()
[![Status](https://img.shields.io/badge/Status-Production%20Ready-success.svg)]()

> **Gợi ý thực hành & Bộ công cụ mẫu tham khảo dành cho Odoo Software Engineers**  
> Chia sẻ góc nhìn và kinh nghiệm thực tế về việc áp dụng **Harness Engineering** vào quy trình phát triển Odoo, hướng tới nguyên lý **Solution First.**

---

## 📖 Mục Lục

1. [Góc Nhìn Kỹ Thuật: Phương Pháp Luận Harness Engineering](#-góc-nhìn-kỹ-thuật-phương-pháp-luận-harness-engineering)
2. [Nguyên Lý Nền Tảng: Solution First Và Kiểm Soát Technical Debt](#-nguyên-lý-nền-tảng-solution-first-và-kiểm-soát-technical-debt)
3. [Gợi Ý 1: Ràng Buộc & Dẫn Hướng (Guides — Skills & Planning)](#-gợi-ý-1-ràng-buộc--dẫn-hướng-guides--skills--planning)
4. [Gợi Ý 2: Công Cụ Kết Nối Hệ Thống (Actuators — Góc Nhìn Về MCP)](#-gợi-ý-2-công-cụ-kết-nối-hệ-thống-actuators--góc-nhìn-về-mcp)
5. [Gợi Ý 3: Phản Biện & Đánh Giá Độc Lập (Verifiers & Feedback — Subagents)](#-gợi-ý-3-phản-biện--đánh-giá-độc-lập-verifiers--feedback--subagents)
6. [Cấu Trúc Thư Mục & Gợi Ý Tích Hợp](#-cấu-trúc-thư-mục--gợi-ý-tích-hợp)
7. [License & Third-Party Notices](#-license--third-party-notices)

---

## ⚙️ Góc Nhìn Kỹ Thuật: Phương Pháp Luận Harness Engineering

Trong kỹ nghệ phần mềm hiện đại (được nghiên cứu bởi **Martin Fowler** và cộng đồng AI Agents), một Coding Agent tự hành không chỉ là một Large Language Model (LLM). Tính ổn định khi làm việc thực tế thường được định hình bởi mối quan hệ:

$$
\mathbf{Agent = Model + Harness}
$$

* **Model**: Cung cấp năng lực hiểu ngữ nghĩa và suy luận logic thô, tuy nhiên mang bản chất stateless và dễ gặp hiện tượng hallucination khi thiếu dữ liệu thực tế từ hệ thống.
* **Harness**: Là hạ tầng kỹ thuật deterministic được thiết lập bao quanh mô hình, bao gồm: các ràng buộc kiến trúc, công cụ kết nối dữ liệu thực tế, môi trường thực thi cô lập và các chốt kiểm tra chất lượng.

```
   ┌────────────────────────────────────────────────────────────────────────┐
   │                       THE AGENT HARNESS SYSTEM                         │
   │                                                                        │
   │   [GUIDES - Ràng buộc & Chỉ dẫn]                                       │
   │   • Skills (.agents/skills)     ──► Ràng buộc ORM, hạn chế SQL thô     │
   │   • Reverse Interview Gate      ──► Phân tích đặc tả kỹ thuật chi tiết │
   │                                                                        │
   │   [ACTUATORS - Kết nối & Tương tác]┌────────────────────────────────┐  │
   │   • PostgreSQL MCP                 │          THE BRAIN             │  │
   │   • Chrome DevTools MCP            │            (LLM)               │  │
   │   • Sequential Thinking            │      Năng lực suy luận thô     │  │
   │                                    └────────────────────────────────┘  │
   │   [VERIFIERS - Kiểm tra & Đánh giá]                                    │
   │   • Role-play Debate            ──► Phản biện phương án kiến trúc      │
   │   • Independent Reviewer        ──► Kiểm soát ACL, N+1 query, i18n     │
   └────────────────────────────────────────────────────────────────────────┘
```

> **Góc nhìn về sự chuyển dịch vai trò**: Thay vì chỉ tập trung viết từng dòng code chi tiết, Software Engineer có xu hướng dịch chuyển sang vai trò **Harness Engineer** — người thiết lập luật chơi, trang bị công cụ kiểm soát và theo dõi feedback loop để hỗ trợ Coding Agent làm việc hiệu quả hơn.

---

## 🧭 Nguyên Lý Nền Tảng: Solution First Và Kiểm Soát Technical Debt

### 1. Nguyên lý Solution First

Software Engineer hướng tới vai trò **Solution Engineer** thay vì chỉ dừng lại ở **Feature Coder**. Trọng tâm không nằm ở số lượng dòng code sinh ra, mà ưu tiên việc tìm kiếm giải pháp kiến trúc phù hợp, giảm thiểu độ phức tạp và chi phí bảo trì lâu dài của hệ thống ERP.

### 2. Quản trị Technical Debt khi tích hợp tác tử

Khi Coding Agent có khả năng can thiệp trực tiếp vào mã nguồn và môi trường dòng lệnh (Terminal), nếu thiếu đi môi trường kiểm soát (Harness), dự án có thể đối mặt với một số rủi ro:

- **Tái phát minh các thành phần sẵn có**: Agent có xu hướng tự viết logic tùy biến phức tạp trong khi Odoo đã có sẵn tính năng tiêu chuẩn (ví dụ: tự viết logic tính giá thay vì kế thừa `product.pricelist`).
- **Phá vỡ tính toàn vẹn của kiến trúc**: Sử dụng truy vấn SQL trực tiếp (`cr.execute()`), lạm dụng `sudo()` làm ảnh hưởng phân quyền multi-company, hoặc quên wrap chuỗi hiển thị qua hàm `_()` phục vụ đa ngôn ngữ (i18n).

---

## 🧠 Gợi Ý 1: Ràng Buộc & Dẫn Hướng (Guides — Skills & Planning)

Trong mô hình Harness, **Guides** đóng vai trò xác lập các giới hạn và định hướng kiến trúc trước khi agent bắt đầu sinh mã.

### 1. Gợi ý quy trình Reverse Interview Gate

Trước khi tiến hành sửa đổi mã nguồn, engineer có thể yêu cầu agent đóng vai trò thẩm vấn để phản biện lại yêu cầu kỹ thuật:

- Làm rõ bản chất nghiệp vụ thực tế đằng sau tính năng.
- Xác định ma trận phân quyền, dữ liệu biên và edge cases.
- Hạn chế hiểu nhầm yêu cầu giữa các bên liên quan.

### 2. Định hình tri thức tác tử qua các bộ kỹ năng tham khảo (`.agents/skills/`)

Thay vì nhồi nhét toàn bộ tài liệu vào System Prompt gây lãng phí bộ nhớ (context bloat), kiến trúc tác tử hiện đại hỗ trợ cơ chế **nạp tri thức theo ngữ cảnh (On-Demand Context Loading)**. Tác tử chỉ đọc lướt mô tả ban đầu và sẽ tự động nạp hướng dẫn chi tiết khi gặp tác vụ tương ứng.

Dưới đây là **các bộ kỹ năng tham khảo mẫu** được tổng hợp sẵn trong thư mục `.agents/skills/`, lập trình viên có thể tùy biến hoặc chọn lọc theo thực tế từng dự án:

* **Bộ quy tắc thẩm định kiến trúc Odoo (Tham khảo `odoo-workflow`)**: Gợi ý nguyên tắc *căn cứ thực tế trước khi sinh mã* — khuyến khích tác tử trích dẫn file và vị trí dòng (`file:line`) từ base addons khi override method hoặc kế thừa view qua XPath.
* **Bộ tra cứu nhanh phiên bản (Tham khảo `odoo-16.0` đến `odoo-19.0`)**: Cung cấp tài liệu tham chiếu về ORM, data model, XML view và cú pháp OWL cho từng phiên bản Odoo cụ thể.
* **Bộ quy tắc tối giản hóa giải pháp (Tham khảo `ponytail` - DietrichGebert)**: Gợi ý tư duy *Ladder of Laziness* nhằm hạn chế việc tác tử tự viết code rườm rà:
  1. *YAGNI*: Tận dụng tối đa cấu hình sẵn có của Odoo trước khi viết mã mới.
  2. *Kế thừa*: Tái sử dụng method và hàm tiện ích nội tại trong codebase.
  3. *Tối giản can thiệp*: Ưu tiên các giải pháp can thiệp gọn nhẹ nhất có thể.
* **Bộ quy chuẩn software engineering (Tham khảo `superpowers` - Jesse Vincent)**: Gợi ý các kỹ thuật phát triển kỷ luật:
  - `systematic-debugging`: Tìm hiểu rõ root cause trước khi nhảy vào sửa mã nguồn.
  - `subagent-driven-development`: Chia nhỏ kế hoạch và phân tách tác vụ cho các tác tử con.
  - `verification-before-completion`: Yêu cầu kiểm tra kết quả thực tế trước khi kết luận hoàn tất.

---

## 🔌 Gợi Ý 2: Công Cụ Kết Nối Hệ Thống (Actuators — Góc Nhìn Về MCP)

Model không thể trực tiếp quan sát môi trường thực tế. Giao thức mở **Model Context Protocol (MCP)** cung cấp các kênh giao tiếp chuẩn hóa, cho phép agent truy cập an toàn vào cơ sở dữ liệu, tài liệu kỹ thuật và môi trường kiểm thử.

Tùy thuộc vào hạ tầng của từng dự án, dưới đây là **4 MCP Server gợi ý tham khảo** hữu ích khi phát triển Odoo:

```
   ┌────────────────────────────────────────────────────────────────────────┐
   │            4 MCP THAM KHẢO HỮU ÍCH TRONG CÔNG VIỆC THỰC TẾ             │
   │                                                                        │
   │  [Sequential Thinking] ──► Hỗ trợ phân tích logic đa bước & rủi ro     │
   │           │                                                            │
   │  [Parallel Search]     ──► Tra cứu tài liệu kỹ thuật Odoo & OCA nhanh  │
   │           │                                                            │
   │  [Toolbox Databases]   ──► Đối soát schema PostgreSQL cục bộ           │
   │           │                                                            │
   │  [Chrome DevTools]     ──► Hỗ trợ kiểm thử và debug giao diện OWL      │
   └────────────────────────────────────────────────────────────────────────┘
```

1. **Sequential Thinking**: Hỗ trợ phân tích logic có cấu trúc, đánh giá rủi ro hệ thống và so sánh giải pháp trước khi triển khai.
2. **Parallel Search**: Tra cứu nhanh tài liệu chính thức và các giải pháp tham khảo từ cộng đồng OCA.
3. **MCP Toolbox for Databases (PostgreSQL)**: Kết nối database môi trường development để đối soát cấu trúc bảng, quan hệ khóa ngoại và index.
4. **Chrome DevTools MCP**: Tự động hóa kiểm thử giao diện và ghi nhận console log / lỗi JavaScript OWL tại trình duyệt.

👉 Xem tài liệu hướng dẫn và liên kết kho mã nguồn tại: [`mcp-configs/README.md`](mcp-configs/README.md).

---

## 👥 Gợi Ý 3: Phản Biện & Đánh Giá Độc Lập (Verifiers & Feedback — Subagents)

Trong phương pháp luận Harness, việc bố trí các **bước kiểm tra độc lập (verification)** và tạo lập **feedback loop** là cách tiếp cận hiệu quả giúp giảm thiểu lỗi phát sinh trước khi chuyển giao mã nguồn.

### 1. Góc nhìn về Context Isolation

Việc dồn toàn bộ lịch sử trao đổi kéo dài vào một phiên làm việc dễ làm suy giảm khả năng tập trung (attention decay / context drift). Một cách xử lý thực tế là phân bổ tác vụ cho các **Subagents** hoạt động trong các context window độc lập.

### 2. Hai mô hình phân vai tham khảo:

#### 🔹 Mô hình 1: Phản biện Kiến trúc Đối lập (Role-play Debate)

* **Solution Proponent**: Xây dựng phương án kỹ thuật ban đầu nhằm đáp ứng yêu cầu nghiệp vụ.
* **System & Performance Critic**: Rà soát và chất vấn các góc nhìn kỹ thuật (nguy cơ table lock, vấn đề hiệu năng với dữ liệu lớn, tính tương thích khi migration).
* **Hiệu quả thực tế**: Quá trình phản biện giúp engineer nhận diện thêm các điểm cần lưu ý trong thiết kế trước khi bắt tay vào code.

#### 🔹 Mô hình 2: Chốt Kiểm Định Độc Lập (Independent Reviewer Gate)

* **Coder Agent**: Đảm nhiệm việc hiện thực hóa mã nguồn theo phương án đã thống nhất.
* **Reviewer Subagent**: Được giữ độc lập với quá trình sinh mã, tiếp nhận thay đổi (Git Diff) để review theo checklist gợi ý:
  - *Bảo mật*: Kiểm tra khai báo quyền hạn trong `ir.model.access.csv`, hạn chế lạm dụng `sudo()` làm bypass multi-company rule.
  - *Hiệu năng*: Nhận diện các vấn đề N+1 query trong các vòng lặp dữ liệu.
  - *i18n*: Đảm bảo việc wrap các chuỗi hiển thị qua hàm dịch thuật `_()`.
* **Feedback loop điều chỉnh**: Coder tiếp nhận review feedback và tự động hiệu chỉnh mã nguồn cho đến khi đạt yêu cầu đề ra.

👉 Xem chi tiết các mẫu chỉ dẫn phân vai tại: [`prompts/README.md`](prompts/README.md).

---

## 📂 Cấu Trúc Thư Mục & Hướng Dẫn Tích Hợp

```text
odoo-coding-agent-playbook/
├── README.md                  # Tài liệu phương pháp luận Harness Engineering
├── LICENSE                    # Giấy phép mã nguồn mở MIT
├── .agents/
│   └── skills/                # Thư mục tri thức kỹ thuật (Odoo 16-19, Workflow, Ponytail, Superpowers)
│       ├── odoo-workflow/
│       ├── odoo-18.0/
│       ├── ponytail/
│       ├── systematic-debugging/
│       └── ...
├── mcp-configs/               # Cấu hình công cụ tương tác (PostgreSQL, Chrome DevTools, Thinking)
│   └── README.md
└── prompts/                   # Mẫu chỉ dẫn phân vai (Prompts cho Subagents Debate & Review)
    └── README.md
```

### Gợi ý quy trình tích hợp vào dự án Odoo:

1. Lựa chọn và sao chép các kỹ năng cần thiết từ thư mục `.agents/skills/` vào thư mục gốc của repository dự án Odoo.
2. Cấu hình các công cụ MCP phù hợp với môi trường phát triển hiện tại theo hướng dẫn tại `mcp-configs/README.md`.
3. Tham khảo quy trình phát triển 3 giai đoạn: **Làm rõ đặc tả (Clarify) ➔ Khảo sát kiến trúc (Trace) ➔ Triển khai có thẩm định (Dual Review)**.

---

## 📄 License & Third-Party Notices

### License

Dự án được phát hành theo giấy phép mã nguồn mở **[MIT License](LICENSE)**.

### Acknowledgments

Dự án kế thừa và tích hợp các nghiên cứu cũng như công cụ mã nguồn mở từ cộng đồng:

- Phương pháp luận **[Harness Engineering](https://martinfowler.com/articles/exploring-gen-ai.html)** dựa trên các nghiên cứu kỹ thuật về tác tử tự hành của **Martin Fowler** (Thoughtworks).
- Bộ kỹ năng Odoo và công cụ kiểm thử `odoo-workflow` từ tác giả **[unclecatvn/agent-skills](https://github.com/unclecatvn/agent-skills)** (MIT License).
- Triết lý tối giản hóa mã nguồn (*Ladder of Laziness*) và bộ kỹ năng `ponytail` từ tác giả **[DietrichGebert/ponytail](https://github.com/DietrichGebert/ponytail)** (MIT License).
- Khung kỹ năng kỷ luật công nghệ phần mềm từ tác giả **[obra/superpowers](https://github.com/obra/superpowers)** (Jesse Vincent, MIT License).
- Chuẩn giao thức **[Model Context Protocol (MCP)](https://github.com/modelcontextprotocol)** và các máy chủ tham chiếu ([Sequential Thinking](https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking), [PostgreSQL Archived](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/postgres)) do **Anthropic** khởi xướng (MIT License), cùng công cụ kết nối cơ sở dữ liệu **[MCP Toolbox for Databases](https://github.com/googleapis/mcp-toolbox)** của **Google** (Apache 2.0 License).
- Máy chủ tìm kiếm thời gian thực **[Parallel AI Search MCP](https://docs.parallel.ai/search/search-mcp)**.
- Công cụ kiểm thử tự động trình duyệt **[Chrome DevTools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp)** do **Google LLC** phát triển (Apache 2.0 License).
- Tinh thần kiến tạo giải pháp bền vững (*Solution First*) của tập thể kỹ sư tại **TGL Solutions**.
