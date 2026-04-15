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

## Cách chạy (How to Run)

### Prerequisites
```bash
pip install pandas
```

### Chạy ETL Pipeline
```bash
python solution.py
```

### Tạo Garbage Data
```bash
python generate_garbage.py
```

### Chạy Agent Simulation (Stress Test)
```bash
# Chạy thí nghiệm so sánh Clean vs Garbage data
python agent_simulation.py
```

---

## Cấu trúc thư mục

```
├── solution.py              # ETL Pipeline script (Extract → Validate → Transform → Load)
├── raw_data.json            # Dữ liệu thô đầu vào (5 records)
├── processed_data.csv       # Output của pipeline (3 records sau validation)
├── generate_garbage.py      # Script tạo dữ liệu "rác" để stress test
├── garbage_data.csv         # Dữ liệu rác (duplicate, null, outlier, wrong type)
├── agent_simulation.py      # Script mô phỏng AI Agent trả lời dựa trên data
├── experiment_report.md     # Báo cáo thí nghiệm Clean vs Garbage
├── STUDENT_GUIDE.md         # Hướng dẫn làm bài
├── tests/                   # Thư mục chứa test cases (auto-grading)
└── README.md                # File này
```

---

## Kết quả

**ETL Pipeline:**
- Tổng số records đầu vào: **5**
- Records hợp lệ (giữ lại): **3** (Laptop, Chair, Monitor)
- Records bị loại: **2**
  - ID 3 — "Mystery Box": giá âm (price = -10) → loại bỏ
  - ID 4 — "Phone": category rỗng → loại bỏ
- Transformations áp dụng: giảm giá 10% (`discounted_price`), chuẩn hóa category (Title Case), thêm `processed_at` timestamp

**Agent Simulation (Stress Test):**
- **Clean Data** → Agent trả lời đúng: "Laptop at $1200" ✅
- **Garbage Data** → Agent trả lời sai: "Nuclear Reactor at $999999" ❌

**Kết luận:** Chất lượng dữ liệu quan trọng hơn chất lượng prompt. Data pipeline với validation chặt chẽ là bắt buộc trước khi đưa dữ liệu vào AI Agent.
