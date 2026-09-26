# Hướng Dẫn Tích Hợp Các MCP Server Tham Khảo Cho Odoo

Tài liệu này tổng hợp thông tin, đường dẫn repository chính thức và file cấu hình mẫu cho **4 MCP Server được tổng hợp và kiểm chứng qua thực tế** khi phát triển Odoo. 

---

## 1. Sequential Thinking MCP
* **Repository GitHub**: [`modelcontextprotocol/servers/tree/main/src/sequentialthinking`](https://github.com/modelcontextprotocol/servers/tree/main/src/sequentialthinking)
* **Gói NPM**: `@modelcontextprotocol/server-sequential-thinking`
* **Mục đích**: Hỗ trợ Agent tư duy từng bước, đánh giá rủi ro và so sánh phương án trước khi code.
* **Cấu hình JSON**:
```json
{
  "mcpServers": {
    "sequential-thinking": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-sequential-thinking"]
    }
  }
}
```

---

## 2. PostgreSQL Database MCP (MCP Toolbox for Databases)
* **Repository GitHub**: [`googleapis/mcp-toolbox`](https://github.com/googleapis/mcp-toolbox) *(Hoặc tham khảo bản archived: [`modelcontextprotocol/servers-archived/tree/main/src/postgres`](https://github.com/modelcontextprotocol/servers-archived/tree/main/src/postgres))*
* **Gói NPM**: `@modelcontextprotocol/server-postgres` *(NPM registry)* hoặc công cụ enterprise [`mcp-toolbox`](https://github.com/googleapis/mcp-toolbox)
* **Mục đích**: Cho phép Agent truy vấn trực tiếp vào CSDL PostgreSQL (môi trường dev cục bộ) để đối soát cấu trúc bảng, các trường quan hệ Many2one/One2many và index của Odoo.
* **Cấu hình JSON mẫu (`@modelcontextprotocol/server-postgres`)**:
```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-postgres",
        "postgresql://odoo:odoo@localhost:5432/odoo_dev_db"
      ]
    }
  }
}
```
*(Thay thế user, password, port, và dbname tương ứng với cấu hình `odoo.conf` hoặc Docker của bạn. Khuyến nghị chỉ cấp quyền Read-Only cho tài khoản database dùng với MCP)*.

---

## 3. Chrome DevTools MCP
* **Repository GitHub**: [`ChromeDevTools/chrome-devtools-mcp`](https://github.com/ChromeDevTools/chrome-devtools-mcp)
* **Gói NPM**: `chrome-devtools-mcp`
* **Mục đích**: Tự động hóa trình duyệt Chrome, thực hiện kiểm thử giao diện Odoo OWL, bắt lỗi JavaScript Console và kiểm tra luồng Network.
* **Cấu hình JSON**:
```json
{
  "mcpServers": {
    "chrome-devtools": {
      "command": "npx",
      "args": ["-y", "chrome-devtools-mcp"]
    }
  }
}
```

---

## 4. Parallel Search MCP (Tra cứu OCA / Odoo Docs trực tiếp)
* **Tài liệu & Nhà phát triển**: [Parallel AI Search MCP](https://docs.parallel.ai/search/search-mcp)
* **Mục đích**: Cho phép Agent tìm kiếm thông tin thời gian thực trên web, tra cứu tài liệu kỹ thuật Odoo mới nhất và quét kho module mã nguồn mở OCA trên GitHub mà không cần API key phức tạp.
* **Cấu hình JSON**:
```json
{
  "mcpServers": {
    "parallel-search": {
      "command": "npx",
      "args": ["-y", "@parallel-ai/search-mcp"]
    }
  }
}
```
*(Lưu ý: Nếu sử dụng trong Google Antigravity, `parallel-search` đã được tích hợp sẵn làm công cụ mặc định, có thể kích hoạt trực tiếp mà không cần cấu hình)*.

---

## 5. NotebookLM MCP (Tra cứu tài liệu nghiệp vụ ERP quy mô lớn)
* **Repository GitHub**: [`PleasePrompto/notebooklm-mcp`](https://github.com/PleasePrompto/notebooklm-mcp)
* **Mục đích**: Cho phép Coding Agent tra cứu trực tiếp kho tài liệu đặc tả nghiệp vụ ERP, tài liệu BA hoặc guideline kỹ thuật nội bộ thông qua NotebookLM (Gemini) với câu trả lời bám sát nguồn trích dẫn (grounded citations), loại bỏ nguy cơ hallucination mà không gây tràn token (context bloat).
* **Cấu hình JSON**:
```json
{
  "mcpServers": {
    "notebooklm": {
      "command": "npx",
      "args": ["-y", "notebooklm-mcp"]
    }
  }
}
```
*(Xem hướng dẫn xác thực tài khoản Google và quản lý thư viện notebook tại kho lưu trữ của [PleasePrompto/notebooklm-mcp](https://github.com/PleasePrompto/notebooklm-mcp))*.
