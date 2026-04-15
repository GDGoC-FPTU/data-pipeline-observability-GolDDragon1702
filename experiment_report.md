# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** AI20K-2A202600261
**Name:** Phạm Hoàng Long
**Date:** 15/4/2026

---

## 1. Kết quả thí nghiệm

Chạy `agent_simulation.py` với 2 bộ dữ liệu và ghi lại kết quả:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | "Based on my data, the best choice is **Laptop** at **$1200**." | 9 | Câu trả lời chính xác, đúng sản phẩm và giá. Pipeline ETL đã lọc sạch dữ liệu trước khi đưa vào Agent. |
| Garbage Data (`garbage_data.csv`) | "Based on my data, the best choice is **Nuclear Reactor** at **$999999**." | 1 | Câu trả lời hoàn toàn sai lệch. Agent bị đánh lừa bởi outlier nghiêm trọng trong dữ liệu chưa qua xử lý. |

---

## 2. Phân tích & nhận xét

### Tại sao Agent trả lời sai khi dùng Garbage Data?

Khi sử dụng `garbage_data.csv`, Agent đã đưa ra câu trả lời hoàn toàn không hợp lý ("Nuclear Reactor at $999999") vì dữ liệu đầu vào chứa nhiều vấn đề nghiêm trọng về chất lượng:

**1. Duplicate IDs (ID trùng lặp):**
Record ID `1` xuất hiện hai lần cho hai sản phẩm hoàn toàn khác nhau (Laptop và Banana). Điều này gây ra sự không nhất quán trong dữ liệu — Agent không thể xác định sản phẩm nào mới là đúng, dẫn đến kết quả truy vấn bị nhiễu.

**2. Wrong Data Types (Sai kiểu dữ liệu):**
Cột `price` của "Broken Chair" có giá trị `"ten dollars"` (dạng chuỗi văn bản) thay vì số. Khi Agent cố gắng so sánh hoặc sắp xếp theo giá, việc có kiểu dữ liệu hỗn hợp (int và string) trong cùng một cột có thể gây ra lỗi runtime hoặc bỏ qua record đó tùy cách xử lý.

**3. Extreme Outlier (Giá trị ngoại lệ cực đoan):**
"Nuclear Reactor" có giá $999,999 — một outlier rõ ràng không có ý nghĩa thực tế trong ngữ cảnh mua sắm điện tử thông thường. Vì Agent dùng logic `idxmax()` (chọn sản phẩm có giá cao nhất trong category "electronics"), nó đã chọn Nuclear Reactor thay vì Laptop — câu trả lời vô nghĩa và gây hại cho người dùng.

**4. Null Values (Giá trị null):**
Record "Ghost Item" có `id = None` và `category = None`. Các giá trị null này có thể gây lỗi khi Agent thực hiện filtering theo category, hoặc làm sai lệch kết quả thống kê nếu không được xử lý đúng cách.

**Kết luận phân tích:** Dữ liệu "rác" không chỉ làm cho Agent trả lời sai, mà còn có thể gây ra các hành vi không thể đoán trước (exceptions, crashes, hoặc kết quả ngẫu nhiên). Đây là minh chứng rõ ràng rằng một ETL pipeline với validation chặt chẽ là bắt buộc trước khi đưa dữ liệu vào bất kỳ hệ thống AI nào.

---

## 3. Kết luận

**Quality Data > Quality Prompt?** 

**Đồng ý hoàn toàn.**

Qua thí nghiệm này, có thể thấy rằng dù Agent có logic tốt đến đâu, nếu dữ liệu đầu vào bị ô nhiễm (duplicate, null, outlier, wrong type), kết quả đầu ra sẽ không thể tin cậy. Một prompt tốt không thể "cứu" được một model đang hoạt động trên dữ liệu sai. Ngược lại, khi pipeline ETL làm sạch dữ liệu đúng cách (loại bỏ giá <= 0, category rỗng), Agent ngay lập tức cho ra câu trả lời chính xác và có ý nghĩa. **Data quality là nền tảng — không có gì thay thế được.**
