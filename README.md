# 🏆 Home Credit - Credit Risk Model Stability Challenge

## Giới Thiệu Chung

[cite_start]Dự án này nhằm giải quyết vấn đề được đề cập trong cuộc thi Kaggle **Home Credit - Credit Risk Model Stability**[cite: 3, 4]. [cite_start]Mục tiêu chính là xây dựng một mô hình dự đoán khả năng vỡ nợ của các khoản vay, giúp Home Credit đưa ra quyết định tín dụng ổn định và chính xác hơn theo thời gian[cite: 26].

* [cite_start]**Đầu vào (Input):** Dữ liệu liên quan đến khoản vay (ví dụ: `case_id`, `WEEK_NUM`, các biến tài chính như `credamount_770A`, `applicationcnt_361L`, v.v.)[cite: 27, 28].
* [cite_start]**Đầu ra (Output):** Tỉ lệ vỡ nợ (có giá trị trong khoảng $[0, 1]$)[cite: 29].

---

## ⚙️ Quy Trình Thực Hiện

Quy trình giải quyết bài toán được chia thành các bước chính sau:

### [cite_start]1. Tiền Xử Lý Dữ Liệu (Data Preprocessing) [cite: 33]

* [cite_start]**Đọc và Kết nối Dữ liệu:** Đọc các bảng dữ liệu có **depth = 0, 1** và một phần **depth = 2**[cite: 34, 35]. [cite_start]Tiến hành tổng hợp dữ liệu và nối các bảng theo khóa `case_id`[cite: 39, 40, 41].
* **Xử lý Cột:**
    * [cite_start]Xử lý cột loại **`date`**[cite: 42].
    * [cite_start]**Lọc cột NULL:** Loại bỏ các cột có trên **70%** giá trị là NULL[cite: 44].
    * [cite_start]**Lọc cột `string`:** Loại bỏ các cột có 1 nhãn hoặc nhiều hơn 200 nhãn[cite: 45].
    * [cite_start]**Xử lý Tương quan:** Chỉ giữ lại một cột đại diện từ các nhóm cột có độ tương quan (correlation) cao với nhau[cite: 51].
* [cite_start]**Tối ưu Bộ nhớ:** Giảm dung lượng bộ nhớ bằng cách thay đổi các kiểu dữ liệu `int` và `float` sang các kiểu phù hợp hơn[cite: 46, 47, 48].

### [cite_start]2. Huấn Luyện Mô Hình (Model Training) [cite: 53]

Nhóm sử dụng các mô hình Gradient Boosting Machine hàng đầu để đảm bảo hiệu suất và tốc độ:

* [cite_start]**CatBoost:** Kết hợp các dự đoán từ các cây quyết định bằng cách tính trung bình có trọng số[cite: 62].
* **LightGBM:** Sử dụng chiến lược phát triển cây **Leaf-wise** thay vì Level-wise (như XGBoost). [cite_start]Chiến lược này thường giúp các cây phát triển nhanh hơn và out-perform hơn với số lượng node nhỏ[cite: 65, 66, 67].

### [cite_start]3. Ensemble Learning [cite: 59, 69, 80]

Để tăng cường **tính ổn định** và **khả năng tổng quát hóa**, nhóm đã kết hợp (Ensemble) nhiều mô hình:

* [cite_start]**CatBoost** [cite: 70]
* [cite_start]**LightGBM (LGB)** với `boosting_type` là **"gbdt"** [cite: 73, 74]
* [cite_start]**LightGBM (LGB)** với `boosting_type` là **"goss"** [cite: 75, 76]

[cite_start]Việc kết hợp này giúp bù đắp lỗi dự đoán của mỗi mô hình, dẫn đến hiệu suất và tính ổn định cao hơn[cite: 82, 83].

### [cite_start]4. Metric Hack [cite: 84]

[cite_start]Nhằm tối ưu hóa điểm số và đạt được mục tiêu dự đoán chính xác nhiều trường hợp nhãn **0 (không vỡ nợ)** nhất[cite: 89], nhóm đã áp dụng một kỹ thuật xử lý hậu kỳ (post-processing):

* [cite_start]**Các trường hợp được dự đoán có điểm (score) thấp hơn 0.05 sẽ được gán lại thành 0**[cite: 85].

---

## 🎯 Kết Quả Đạt Được

Nhờ vào các kỹ thuật tiền xử lý, huấn luyện và kết hợp mô hình, nhóm đã đạt được thành tích ấn tượng trong cuộc thi:

* [cite_start]**Xếp hạng:** **134/3858**[cite: 93].
* [cite_start]**Điểm số (Private Score):** **0.519**[cite: 93].

[cite_start]Những kỹ thuật này giúp tạo ra một hệ thống dự đoán mạnh mẽ, ổn định và chính xác, phù hợp với yêu cầu ứng dụng thực tế[cite: 94].

---

## 🔮 Hướng Phát Triển Tương Lai

* Tối ưu hóa siêu tham số (Hyperparameter Tuning) sâu hơn cho các mô hình CatBoost và LightGBM.
* Nghiên cứu các phương pháp Ensemble nâng cao như Stacking hoặc Blending.
* [cite_start]Ứng dụng mô hình vào thực tế để đánh giá tính ổn định theo thời gian[cite: 95].

---

Bạn có muốn tôi thêm hoặc chỉnh sửa bất kỳ chi tiết nào trong bản nháp `README` này không?
