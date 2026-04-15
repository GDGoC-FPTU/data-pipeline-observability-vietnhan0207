[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=23574131&assignment_repo_type=AssignmentRepo)
# Day 10 Lab: Data Pipeline & Data Observability

**Student Email:** 26ai.nhanpnv@vinuni.edu.com
**Name:** Phan Nguyễn Việt Nhân

---

## Mô tả

Trong bài lab này, em đã xây dựng một ETL Pipeline tự động hoàn chỉnh bao gồm 4 bước chính: **Extract** dữ liệu từ file JSON, **Validate** để lọc bỏ các bản ghi không hợp lệ (giá âm hoặc bằng 0, category trống), **Transform** để chuẩn hóa dữ liệu (tính giá sau giảm 10%, đổi category về Title Case, thêm timestamp), và **Load** kết quả ra file CSV. Bên cạnh đó, em cũng thực hiện thí nghiệm so sánh hiệu quả của AI Agent khi hoạt động với dữ liệu sạch và dữ liệu rác, qua đó thấy rõ tầm quan trọng của chất lượng dữ liệu đối với kết quả đầu ra.

---

## Cách chạy (How to Run)

### Prerequisites

Tạo và kích hoạt virtual environment, sau đó cài đặt các thư viện cần thiết:

```bash
python -m venv venv
.\venv\Scripts\activate
pip install pandas pytest
```

### Chạy ETL Pipeline

```bash
python solution.py
```

Sau khi chạy thành công, file `processed_data.csv` sẽ được tạo ra trong cùng thư mục.

### Chạy Agent Simulation (Stress Test)

Bước 1 — Tạo dữ liệu sạch bằng cách chạy pipeline trước:

```bash
python solution.py
```

Bước 2 — Tạo dữ liệu rác:

```bash
python generate_garbage.py
```

Bước 3 — Chạy thí nghiệm so sánh:

```bash
python agent_simulation.py
```

### Chạy Tests (Autograder)

```bash
pytest tests/test_autograder.py -v
```

---

## Cấu trúc thư mục

```
├── solution.py              # ETL Pipeline script
├── raw_data.json            # Dữ liệu đầu vào
├── processed_data.csv       # Output của pipeline (được tạo sau khi chạy)
├── garbage_data.csv         # Dữ liệu rác (được tạo bởi generate_garbage.py)
├── agent_simulation.py      # Script mô phỏng AI Agent
├── generate_garbage.py      # Script tạo dữ liệu rác
├── experiment_report.md     # Báo cáo thí nghiệm so sánh
└── README.md                # File này
```

---

## Kết quả

Pipeline xử lý tổng cộng **5 records** từ file `raw_data.json`:
- **3 records hợp lệ** được giữ lại và lưu vào `processed_data.csv`
- **2 records bị loại**: 1 record có giá âm (-10), 1 record có category trống
- Giá sau giảm 10% đã được tính toán chính xác vào cột `discounted_price`
- Category đã được chuẩn hóa về dạng Title Case (vd: "electronics" → "Electronics")
- Mỗi record được thêm cột `processed_at` với timestamp xử lý

Qua thí nghiệm Agent Simulation, em nhận thấy rằng dữ liệu sạch giúp agent trả lời chính xác (chọn Laptop $1200), trong khi dữ liệu rác khiến agent chọn Nuclear Reactor $999.999 — một kết quả do outlier trong dữ liệu.
