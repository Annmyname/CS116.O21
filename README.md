# 🏆 Home Credit - Credit Risk Model Stability Challenge

## Giới Thiệu Chung

Dự án này nhằm giải quyết vấn đề được đề cập trong cuộc thi Kaggle **Home Credit - Credit Risk Model Stability**. Mục tiêu chính là xây dựng một mô hình dự đoán **khả năng vỡ nợ của các khoản vay**, giúp Home Credit đưa ra quyết định tín dụng ổn định và chính xác hơn theo thời gian.

* **Đầu vào (Input):** Dữ liệu liên quan đến khoản vay (ví dụ: `case_id`, `WEEK_NUM`, các biến tài chính như `credamount_770A`, `applicationcnt_361L`, v.v.).
* **Đầu ra (Output):** Tỉ lệ vỡ nợ (có giá trị trong khoảng $[0, 1]$).

---

## ⚙️ Quy Trình Thực Hiện

Quy trình giải quyết bài toán được chia thành các bước chính sau:

### 1. Tiền Xử Lý Dữ Liệu (Data Preprocessing)

* **Đọc và Kết nối Dữ liệu:** Đọc các bảng dữ liệu có **depth = 0, 1** và một phần **depth = 2**. Tiến hành tổng hợp dữ liệu và nối các bảng theo khóa `case_id`.
* **Xử lý Cột:**
    * Xử lý cột loại **`date`**.
    * **Lọc cột NULL:** Loại bỏ các cột có trên **70%** giá trị là NULL.
    * **Lọc cột `string`:** Loại bỏ các cột có 1 nhãn hoặc nhiều hơn 200 nhãn.
    * **Xử lý Tương quan:** Chỉ giữ lại một cột đại diện từ các nhóm cột có độ tương quan (correlation) cao với nhau.
* **Tối ưu Bộ nhớ:** Giảm dung lượng bộ nhớ bằng cách thay đổi các kiểu dữ liệu `int` và `float` sang các kiểu phù hợp hơn.

### 2. Huấn Luyện Mô Hình (Model Training)

Nhóm sử dụng các mô hình Gradient Boosting Machine hàng đầu để đảm bảo hiệu suất và tốc độ:

* **CatBoost:** Kết hợp các dự đoán từ các cây quyết định bằng cách tính trung bình có trọng số.
* **LightGBM:** Sử dụng chiến lược phát triển cây **Leaf-wise** thay vì Level-wise (như XGBoost). Chiến lược này thường giúp các cây phát triển nhanh hơn và out-perform hơn với số lượng node nhỏ. 

### 3. Ensemble Learning

Để tăng cường **tính ổn định** và **khả năng tổng quát hóa**, nhóm đã kết hợp (Ensemble) nhiều mô hình:

* **CatBoost**
* **LightGBM (LGB)** với `boosting_type` là **"gbdt"**
* **LightGBM (LGB)** với `boosting_type` là **"goss"**

Việc kết hợp này giúp bù đắp lỗi dự đoán của mỗi mô hình, dẫn đến hiệu suất và tính ổn định cao hơn.

### 4. Metric Hack

Nhằm tối ưu hóa điểm số và đạt được mục tiêu dự đoán chính xác nhiều trường hợp nhãn **0 (không vỡ nợ)** nhất, nhóm đã áp dụng một kỹ thuật xử lý hậu kỳ (post-processing):

* **Các trường hợp được dự đoán có điểm (score) thấp hơn 0.05 sẽ được gán lại thành 0**.

---

## 🎯 Kết Quả Đạt Được

Nhờ vào các kỹ thuật tiền xử lý, huấn luyện và kết hợp mô hình, nhóm đã đạt được thành tích ấn tượng trong cuộc thi:

* **Xếp hạng:** **134/3858**.
* **Điểm số (Private Score):** **0.519**.

Những kỹ thuật này giúp tạo ra một hệ thống dự đoán mạnh mẽ, ổn định và chính xác, phù hợp với yêu cầu ứng dụng thực tế.

---

## 🔮 Hướng Phát Triển Tương Lai

* Tối ưu hóa siêu tham số (Hyperparameter Tuning) sâu hơn cho các mô hình CatBoost và LightGBM.
* Nghiên cứu các phương pháp Ensemble nâng cao như Stacking hoặc Blending.
* Ứng dụng mô hình vào thực tế để đánh giá tính ổn định theo thời gian.
