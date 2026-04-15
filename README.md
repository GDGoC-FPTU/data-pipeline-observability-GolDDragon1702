[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23573969&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** 26ai.longph@vinuni.edu.vn
**Name:** Phạm Hoàng Long

---

## Mô tả

Bài lab này xây dựng một **ETL Pipeline tự động** để xử lý dữ liệu sản phẩm từ file JSON, bao gồm 4 bước chính: Extract, Validate, Transform và Load. Sau đó, tiến hành thí nghiệm **Stress Test** để so sánh phản hồi của một AI Agent khi sử dụng dữ liệu sạch (đã qua pipeline) và dữ liệu "rác" (chứa lỗi chất lượng), từ đó chứng minh tầm quan trọng của **Data Quality** đối với hiệu suất AI.

**Những gì đã thực hiện:**
- Hoàn thành 4 hàm ETL trong `solution.py`: `extract()`, `validate()`, `transform()`, `load()`
- Tạo `garbage_data.csv` chứa dữ liệu lỗi (duplicate IDs, wrong types, outliers, null values)
- Chạy `agent_simulation.py` với cả 2 bộ dữ liệu và ghi nhận kết quả
- Phân tích chi tiết nguyên nhân Agent trả lời sai khi dùng dữ liệu rác

---

## Cach chay (How to Run)

### Prerequisites
```bash
pip install pandas
```

### Chay ETL Pipeline
```bash
python solution.py
```

### Chay Agent Simulation (Stress Test)
```bash
# Chạy file Python để test agent với 2 loại dữ liệu
python agent_simulation.py

# Trong code đã có sẵn:
# - Test với CLEAN data (processed_data.csv)
# → Agent lọc electronics và trả kết quả hợp lý

# - Test với GARBAGE data (garbage_data.csv)
# → Agent vẫn chạy nhưng trả kết quả sai do dữ liệu lỗi
```

---

## Cau truc thu muc

```
├── solution.py              # ETL Pipeline script
├── processed_data.csv       # Output cua pipeline
├── experiment_report.md     # Bao cao thi nghiem
└── README.md                # File nay
```

---

## Ket qua

Tổng records đầu vào (garbage): 5
Records hợp lệ sau xử lý: 3 (Laptop, Chair, Monitor)
Records bị loại: 2
- 1 record do trùng ID (Banana)
- 1 record do sai kiểu dữ liệu (price = "ten dollars")
- 1 record do thiếu giá trị (Ghost Item) (có thể bị loại trong pipeline làm sạch)

→ Pipeline đã loại bỏ các dữ liệu lỗi và giữ lại 3 records sạch, đảm bảo agent hoạt động chính xác hơn.
