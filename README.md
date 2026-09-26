# Odoo Agent Harness: Khung Phương Pháp Luận "Harness Engineering" Dành Cho Odoo Software Engineers

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Odoo Version](https://img.shields.io/badge/Odoo-16%20|%2017%20|%2018%20|%2019-714B67.svg)](https://www.odoo.com)
[![Methodology](https://img.shields.io/badge/Methodology-Harness%20Engineering-darkgreen.svg)]()
[![Status](https://img.shields.io/badge/Status-Production%20Ready-success.svg)]()

> **Handbook thực hành & Starter Kit dành cho Odoo Software Engineers**  
> Định hình mô hình làm việc có cấu trúc: từ phát triển thủ công sang **thiết lập Harness Engineering**, hiện thực hóa nguyên lý **Solution First.**

---

## 📖 Mục Lục

1. [Khung Lý Thuyết: Phương Pháp Luận Harness Engineering](#-khung-lý-thuyết-phương-pháp-luận-harness-engineering)
2. [Nguyên Lý Nền Tảng: Solution First Và Kiểm Soát Technical Debt](#-nguyên-lý-nền-tảng-solution-first-và-kiểm-soát-technical-debt)
3. [Thành Phần 1: Guides — Skills & Planning](#-thành-phần-1-guides--skills--planning)
4. [Thành Phần 2: Actuators — Model Context Protocol (MCP)](#-thành-phần-2-actuators--model-context-protocol-mcp)
5. [Thành Phần 3: Verifiers & Feedback — Subagents](#-thành-phần-3-verifiers--feedback--subagents)
6. [Cấu Trúc Thư Mục & Hướng Dẫn Tích Hợp](#-cấu-trúc-thư-mục--hướng-dẫn-tích-hợp)
7. [License & Third-Party Notices](#-license--third-party-notices)

---

## ⚙️ Khung Lý Thuyết: Phương Pháp Luận Harness Engineering

Trong kỹ nghệ phần mềm hiện đại (được nghiên cứu bởi **Martin Fowler** và cộng đồng AI Agents), một Coding Agent tự hành không chỉ là một Large Language Model (LLM). Độ tin cậy và tính ổn định của hệ thống được xác định bởi mối quan hệ:

$$
\mathbf{Agent = Model + Harness}
$$

* **Model**: Cung cấp năng lực hiểu ngữ nghĩa và suy luận logic thô, tuy nhiên mang bản chất stateless và dễ gặp hiện tượng hallucination khi thiếu dữ liệu thực tế từ hệ thống.
* **Harness**: Là toàn bộ hạ tầng kỹ thuật deterministic được thiết lập bao quanh mô hình, bao gồm: các ràng buộc kiến trúc, công cụ kết nối dữ liệu thực tế, môi trường thực thi cô lập và các chốt kiểm định chất lượng.

```
   ┌────────────────────────────────────────────────────────────────────────┐
   │                       THE AGENT HARNESS SYSTEM                         │
   │                                                                        │
   │   [GUIDES - Ràng buộc & Chỉ dẫn]                                       │
   │   • Skills (.agents/skills)     ──► Ràng buộc ORM, hạn chế SQL thô     │
   │   • Reverse Interview Gate      ──► Phân tích đặc tả kỹ thuật chi tiết │
   │                                                                        │
   │   [ACTUATORS - Kết nối & Tương tác]┌────────────────────────────────┐   │
   │   • PostgreSQL MCP                 │          THE BRAIN             │   │
   │   • Chrome DevTools MCP            │            (LLM)               │   │
   │   • Sequential Thinking            │      Năng lực suy luận thô     │   │
   │                                    └────────────────────────────────┘   │
   │   [VERIFIERS - Kiểm tra & Đánh giá]                                    │
   │   • Role-play Debate            ──► Phản biện phương án kiến trúc      │
   │   • Independent Reviewer        ──► Kiểm soát ACL, N+1 query, i18n     │
   └────────────────────────────────────────────────────────────────────────┘
```

> **Sự chuyển dịch vai trò**: Thay vì chỉ tập trung viết từng dòng code chi tiết, Software Engineer đóng vai trò như một **Harness Engineer** — người thiết kế luật chơi, trang bị công cụ kiểm soát và thiết lập feedback loop đảm bảo chất lượng cho Coding Agent.

---

## 🧭 Nguyên Lý Nền Tảng: Solution First Và Kiểm Soát Technical Debt

### 1. Nguyên lý Solution First

Software Engineer định vị vai trò là **Solution Engineer** thay vì chỉ dừng lại ở **Feature Coder**. Mục tiêu cốt lõi không nằm ở số dòng code sinh ra, mà tập trung vào việc lựa chọn giải pháp kiến trúc tối ưu, giảm thiểu độ phức tạp và chi phí vận hành lâu dài của hệ thống ERP.

### 2. Quản trị Nợ kỹ thuật (Technical Debt) khi tích hợp tác tử

Khi các công cụ Coding Agent được cấp quyền can thiệp trực tiếp vào mã nguồn và môi trường dòng lệnh (Terminal), việc thiếu vắng một quy trình Harness có kiểm soát sẽ dẫn đến các rủi ro hệ thống:

- **Tái phát minh các thành phần sẵn có**: Tự phát triển các giải pháp tùy biến phức tạp trong khi framework Odoo đã hỗ trợ tính năng tiêu chuẩn (ví dụ: tự lập trình cơ chế định giá thay vì kế thừa cấu hình `product.pricelist`).
- **Phá vỡ tính toàn vẹn của kiến trúc**: Sử dụng các truy vấn SQL trực tiếp (`cr.execute()`), lạm dụng quyền quản trị tối cao (`sudo()`) dẫn đến vi phạm bảo mật dữ liệu đa công ty (Multi-company Isolation), hoặc bỏ sót các hàm bản địa hóa `_()` phục vụ môi trường đa ngôn ngữ (i18n).

---

## 🧠 Thành Phần 1: Guides — Skills & Planning

Trong mô hình Harness, **Guides** đóng vai trò xác lập các ranh giới kiến trúc trước khi agent bắt đầu sinh mã.

### 1. Quy trình Reverse Interview Gate

Trước khi tiến hành sửa đổi mã nguồn, engineer kích hoạt vai trò thẩm vấn để agent phản biện lại tài liệu đặc tả kỹ thuật:

- Làm rõ bản chất nghiệp vụ và mục tiêu vận hành của tính năng.
- Xác định ma trận phân quyền, dữ liệu biên và edge cases.
- Hạn chế tối đa các sai lệch trong việc tiếp nhận yêu cầu.

### 2. Tiêu chuẩn hóa quy tắc phát triển thông qua `SKILL.md` (Progressive Disclosure)

Toàn bộ tri thức và chuẩn mực kỹ thuật được đóng gói theo định dạng chuẩn hóa trong thư mục `.agents/skills/`. Cơ chế **Progressive Disclosure** đảm bảo agent chỉ nạp các chỉ dẫn chi tiết khi ngữ cảnh công việc yêu cầu, tối ưu hóa context window và duy trì độ chính xác cao:

* **`odoo-workflow` (Nguyên tắc: *Căn cứ thực tế trước khi sinh mã*)**: Mọi thao tác override method hoặc kế thừa view qua XPath bắt buộc phải trích dẫn chính xác file và vị trí dòng (`file:line`) từ base addons làm cơ sở.
* **`odoo-16.0` đến `odoo-19.0` Reference Packs**: Cung cấp tài liệu tra cứu chuẩn xác về data model, ORM, kiến trúc View và thư viện OWL tương ứng với từng phiên bản.
* **`ponytail` (Triết lý: *Ladder of Laziness — Tối giản hóa giải pháp*)**: Khung quy tắc kiểm soát độ phức tạp mã nguồn (anti-bloat) từ **DietrichGebert**, yêu cầu agent ưu tiên giải pháp có mức độ can thiệp thấp nhất:
  1. *YAGNI (You Aren't Gonna Need It)*: Loại bỏ các tính năng mang tính suy đoán; tái sử dụng tối đa Odoo standard.
  2. *Kế thừa nội tại*: Tái sử dụng các method và design pattern sẵn có trong codebase.
  3. *Tối ưu can thiệp*: Ưu tiên các giải pháp cấu hình hoặc code ngắn gọn trước khi xây dựng module mới.
* **`superpowers` (Quy chuẩn software engineering)**: Khung phương pháp luận kỹ thuật từ **Jesse Vincent (`obra/superpowers`)**:
  - `systematic-debugging`: Yêu cầu điều tra và xác định root cause trước khi đề xuất bất kỳ phương án sửa lỗi nào.
  - `subagent-driven-development`: Mô hình thực thi kế hoạch theo từng subagent độc lập kèm khâu kiểm định chéo.
  - `verification-before-completion`: Nguyên tắc chỉ xác nhận hoàn tất nhiệm vụ khi có đầy đủ bằng chứng kiểm thử thực tế.

---

## 🔌 Thành Phần 2: Actuators — Model Context Protocol (MCP)

Model không thể trực tiếp quan sát môi trường thực tế. Giao thức mở **Model Context Protocol (MCP)** cung cấp các kênh giao tiếp chuẩn hóa, cho phép agent truy cập an toàn vào cơ sở dữ liệu, tài liệu kỹ thuật và môi trường kiểm thử.

Tùy thuộc vào hạ tầng của từng dự án, dưới đây là **4 MCP Server được tác giả đúc kết và thường xuyên sử dụng trong thực tế** khi phát triển Odoo:

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
2. **Parallel Search**: Tra cứu nhanh tài liệu chính thức và các giải pháp đã được kiểm chứng từ kho mã nguồn mở OCA.
3. **MCP Toolbox for Databases (PostgreSQL)**: Kết nối database môi trường development để đối soát cấu trúc bảng, quan hệ khóa ngoại và index.
4. **Chrome DevTools MCP**: Tự động hóa kiểm thử giao diện và ghi nhận console log / lỗi JavaScript OWL tại trình duyệt.

👉 Xem tài liệu hướng dẫn và liên kết kho mã nguồn tại: [`mcp-configs/README.md`](mcp-configs/README.md).

---

## 👥 Thành Phần 3: Verifiers & Feedback — Subagents

Trong phương pháp luận Harness, việc thiết lập các **chốt kiểm tra độc lập (verification)** và vận hành **feedback loop** là yếu tố then chốt nhằm ngăn chặn lỗi kỹ thuật trước khi mã nguồn được chuyển giao cho engineer.

### 1. Nguyên lý Context Isolation

Việc tích lũy lịch sử tương tác kéo dài trong một phiên làm việc dễ dẫn đến hiện tượng context drift / attention decay. Engineer giải quyết vấn đề này bằng cách phân bổ nhiệm vụ cho các **Subagents** hoạt động trong các context window hoàn toàn độc lập.

### 2. Hai mô hình phân vai thực tế tác giả thường áp dụng:

#### 🔹 Mô hình 1: Phản biện Kiến trúc Đối lập (Role-play Debate)

* **Solution Proponent**: Xây dựng phương án kỹ thuật ban đầu nhằm đáp ứng yêu cầu nghiệp vụ.
* **System & Performance Critic**: Rà soát và chất vấn các giới hạn kỹ thuật (nguy cơ table lock, vấn đề hiệu năng với dữ liệu lớn, tính tương thích khi migration).
* **Kết quả thực tế**: Quá trình phản biện tự động giúp engineer nhận diện các điểm mù trong thiết kế và đưa ra quyết định kiến trúc chính xác.

#### 🔹 Mô hình 2: Chốt Kiểm Định Độc Lập (Independent Reviewer Gate)

* **Coder Agent**: Đảm nhiệm việc hiện thực hóa mã nguồn theo phương án đã thống nhất.
* **Reviewer Subagent**: Được giữ độc lập hoàn toàn với quá trình sinh mã, tiếp nhận thay đổi (Git Diff) để review theo checklist:
  - *Bảo mật*: Kiểm tra khai báo quyền hạn trong `ir.model.access.csv`, kiểm soát rủi ro lộ lọt dữ liệu khi sử dụng `sudo()`.
  - *Hiệu năng*: Nhận diện các vấn đề N+1 query trong các vòng lặp dữ liệu.
  - *i18n*: Đảm bảo việc wrap các chuỗi hiển thị qua hàm dịch thuật `_()`.
* **Feedback loop điều chỉnh**: Coder tiếp nhận review feedback và tự động hiệu chỉnh mã nguồn cho đến khi đạt đầy đủ các tiêu chuẩn đề ra.

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

### Quy trình tích hợp vào dự án Odoo:

1. Sao chép thư mục `.agents/skills/` vào thư mục gốc của repository dự án Odoo.
2. Cấu hình các công cụ MCP tương thích với môi trường phát triển theo hướng dẫn tại `mcp-configs/README.md`.
3. Áp dụng quy trình 3 giai đoạn khi tiếp nhận yêu cầu: **Làm rõ đặc tả (Clarify) ➔ Khảo sát kiến trúc (Trace) ➔ Triển khai có thẩm định (Dual Review)**.

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
- Chuẩn giao thức **[Model Context Protocol (MCP)](https://github.com/modelcontextprotocol)** và các máy chủ tham chiếu ([PostgreSQL](https://github.com/modelcontextprotocol/servers/tree/main/src/postgres), [Sequential Thinking](https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking)) do **Anthropic** khởi xướng (MIT License).
- Máy chủ tìm kiếm thời gian thực **[Parallel AI Search MCP](https://docs.parallel.ai/search/search-mcp)**.
- Công cụ kiểm thử tự động trình duyệt **[Chrome DevTools MCP](https://github.com/ChromeDevTools/chrome-devtools-mcp)** do **Google LLC** phát triển (Apache 2.0 License).
- Tinh thần kiến tạo giải pháp bền vững (*Solution First*) của tập thể kỹ sư tại **TGL Solutions**.
