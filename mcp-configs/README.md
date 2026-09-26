# Bộ Tứ MCP Cốt Lõi Cho Odoo Developer (The Core Four MCPs)

Tài liệu này hướng dẫn cách cấu hình 4 MCP Server thiết yếu cho môi trường phát triển Odoo (áp dụng cho **Google Antigravity**, **Claude Code**, **Cursor**, hoặc **Windsurf**).

---

## 1. Sequential Thinking MCP
- **Mục đích**: Kích hoạt khả năng phân tích logic từng bước (chain-of-thought) và đánh giá đánh đổi (trade-offs) trước khi sinh mã.
- **Cấu hình (Node / npx)**:
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

## 2. PostgreSQL / Database MCP
- **Mục đích**: Kết nối trực tiếp vào CSDL PostgreSQL cục bộ/dev để soi cấu trúc bảng, trường dữ liệu, quan hệ Many2one/One2many và index thực tế.
- **Cấu hình (Docker hoặc Python / Node)**:
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
*(Thay thế user, password, port, và dbname tương ứng với cấu hình `odoo.conf` hoặc Docker của bạn)*.

---

## 3. Chrome DevTools MCP
- **Mục đích**: Tự động hóa trình duyệt Chrome, thực hiện kiểm thử giao diện Odoo OWL, bắt lỗi JavaScript Console và kiểm tra luồng Network.
- **Cấu hình**:
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

## 4. Web Fetch & Search MCP (Tra cứu OCA / Docs)
- **Mục đích**: Cho phép Agent tra cứu các module mã nguồn mở OCA trên GitHub và tài liệu chính thức của Odoo.
- **Cấu hình (Ví dụ với Fetch MCP)**:
```json
{
  "mcpServers": {
    "fetch": {
      "command": "uvx",
      "args": ["mcp-server-fetch"]
    }
  }
}
```
*(Hoặc sử dụng Parallel Search MCP tích hợp sẵn nếu chạy trong Antigravity)*.
