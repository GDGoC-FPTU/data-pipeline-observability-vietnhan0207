# Experiment Report: Data Quality Impact on AI Agent

**Student ID:** 2A202600279
**Name:** Phan Nguyễn Việt Nhân
**Date:** 2026-04-15

---

## 1. Kết quả thí nghiệm

Chạy `agent_simulation.py` với 2 bộ dữ liệu và ghi lại kết quả:

| Scenario | Agent Response | Accuracy (1-10) | Notes |
|----------|----------------|-----------------|-------|
| Clean Data (`processed_data.csv`) | Based on my data, the best choice is Laptop at $1200. | 9 | Agent trả lời chính xác, chọn đúng sản phẩm electronics có giá cao nhất sau khi dữ liệu đã được lọc và chuẩn hóa. |
| Garbage Data (`garbage_data.csv`) | Based on my data, the best choice is Nuclear Reactor at $999999. | 1 | Agent trả lời sai hoàn toàn. Dữ liệu rác chứa outlier cực đoan (Nuclear Reactor $999.999) khiến agent chọn sản phẩm không có thực tế. |

---

## 2. Phân tích & nhận xét

### Tai sao Agent trả lời sai khi dùng Garbage Data?

Khi em chạy agent với bộ dữ liệu rác (`garbage_data.csv`), kết quả trả về hoàn toàn sai lệch so với thực tế. Có nhiều nguyên nhân dẫn đến vấn đề này:

**Duplicate IDs:** Bộ dữ liệu rác có hai bản ghi cùng ID là 1 (Laptop và Banana). Điều này gây ra sự nhầm lẫn trong việc xác định sản phẩm, khiến agent có thể tham chiếu đến sản phẩm sai.

**Wrong Data Types:** Bản ghi "Broken Chair" có trường `price` là chuỗi "ten dollars" thay vì số. Nếu agent có phần logic tính toán giá, nó sẽ gặp lỗi hoặc bỏ qua bản ghi này, dẫn đến kết quả phân tích thiếu chính xác.

**Extreme Outliers:** Đây chính là nguyên nhân chủ yếu khiến agent trả lời sai. "Nuclear Reactor" có giá $999.999 — một giá trị bất thường nhưng vì agent chỉ tìm sản phẩm có `price` cao nhất trong category "electronics", nó sẽ chọn ngay sản phẩm này mà không có bất kỳ bước kiểm tra nào.

**Null Values:** Bản ghi "Ghost Item" có cả `id` và `category` là null, giá trị price là 0. Những bản ghi kiểu này nếu không được lọc trước sẽ làm nhiễu dữ liệu đầu vào và có thể gây ra exception trong quá trình xử lý.

Tóm lại, agent hoạt động theo logic "garbage in, garbage out", chất lượng dữ liệu đầu vào quyết định trực tiếp đến chất lượng câu trả lời đầu ra. Một pipeline ETL tốt với bước Validate và Transform là chìa khóa để bảo vệ agent khỏi những sai lệch này.

---

## 3. Kết luận

**Quality Data > Quality Prompt?** Em đồng ý với nhận định này.

Dù prompt có được thiết kế tinh vi đến đâu, nếu dữ liệu đầu vào chứa nhiều lỗi như duplicate, outlier, null hay sai kiểu dữ liệu, agent vẫn sẽ trả về kết quả sai lệch hoặc vô nghĩa. Trong bài thí nghiệm này, cùng một prompt "What is the best electronic product?", agent cho ra kết quả chính xác với clean data nhưng cho ra kết quả hoàn toàn phi thực tế với garbage data. Vì vậy, đầu tư vào chất lượng dữ liệu thông qua các bước Extract, Validate, Transform bài bản là nền tảng không thể thiếu trước khi nói đến việc tối ưu hóa prompt hay model.
