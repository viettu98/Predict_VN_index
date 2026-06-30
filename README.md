# 📈 Dự Báo Chỉ Số VN-Index

> Ứng dụng học máy dự báo chỉ số VN-Index (chứng khoán Việt Nam) sử dụng mô hình **LSTM (Long Short-Term Memory)**

## 📋 Tổng Quan Dự Án

Dự án này xây dựng một mô hình học sâu (Deep Learning) để dự báo giá đóng cửa của chỉ số VN-Index - chỉ số chứng khoán lớn nhất tại Việt Nam. Mô hình sử dụng **kiến trúc LSTM** để học từ chuỗi thời gian (Time Series) của dữ liệu lịch sử chỉ số và có khả năng dự báo hướng di chuyển giá trong tương lai.

### Đặc điểm:
- 🔄 **Xử lý chuỗi thời gian**: Sử dụng 60 bước thời gian trước để dự báo bước kế tiếp
- 🧠 **Mô hình LSTM 4 lớp**: Kiến trúc sâu với Dropout để tránh overfitting
- 📊 **Dữ liệu lịch sử**: Từ 2000-2018 để huấn luyện, 2019 để kiểm thử
- 🎯 **Dự báo nhiều bước**: Dự báo 28 ngày tiếp theo từ dữ liệu hiện tại

---

## ✨ Tính Năng

### 1. **Huấn Luyện Mô Hình LSTM**
   - 4 lớp LSTM với 50 units mỗi lớp
   - Dropout 20% để regularization
   - Hàm tối ưu Adam, loss function: Mean Squared Error
   - Tự động lưu model sau khi huấn luyện

### 2. **Chuẩn Hóa Dữ Liệu (Normalization)**
   - MinMaxScaler: Scale dữ liệu về khoảng [0, 1]
   - Giúp mô hình học tốt hơn và ổn định

### 3. **Dự Báo Giá**
   - Dự báo giá VN-Index trên tập dữ liệu test (2019)
   - So sánh giá thực tế vs giá dự báo bằng biểu đồ
   - Dự báo phía trước 28 ngày tiếp theo

### 4. **Trực Quan Hóa Kết Quả**
   - Vẽ biểu đồ so sánh giá thực tế (đỏ) vs giá dự báo (xanh)
   - In ra giá dự báo cho từng ngày tiếp theo

---

## 🚀 Cách Sử Dụng

### Yêu Cầu Hệ Thống
```bash
Python >= 3.6
```

### Cài Đặt Thư Viện
```bash
pip install numpy pandas scikit-learn matplotlib keras tensorflow
```

### Chạy Dự Báo

1. **Đặt tệp vào cùng thư mục**:
   ```
   Predict_VN_index/
   ├── predict-vnindex.py
   ├── VNINDEX.csv           (dữ liệu 2000-2018)
   ├── VNINDEX-2019.csv      (dữ liệu 2019)
   └── mymodel.h5            (model đã huấn luyện)
   ```

2. **Chạy script**:
   ```bash
   python predict-vnindex.py
   ```

3. **Kết Quả**:
   - Hiển thị biểu đồ so sánh giá thực tế vs dự báo
   - In ra giá dự báo cho 28 ngày tiếp theo
   - Mô hình được lưu để sử dụng lại

### Cấu Trúc Dữ Liệu Input

**VNINDEX.csv** (dữ liệu huấn luyện):
```
DATE,CLOSE
1/1/2000,99.99
1/2/2000,105.50
...
```

**VNINDEX-2019.csv** (dữ liệu kiểm thử):
```
DATE,CLOSE
1/1/2019,850.00
1/2/2019,855.30
...
```

---

## 🛠️ Công Nghệ & Thư Viện

| Thành Phần | Công Nghệ | Mục Đích |
|-----------|-----------|---------|
| **Deep Learning** | Keras + TensorFlow | Xây dựng mô hình LSTM |
| **Xử Lý Dữ Liệu** | Pandas + NumPy | Đọc, xử lý, reshape dữ liệu |
| **ML Preprocessing** | Scikit-learn | Chuẩn hóa dữ liệu (MinMaxScaler) |
| **Trực Quan Hóa** | Matplotlib | Vẽ biểu đồ kết quả |

### Kiến Trúc Mô Hình

```
Input (60 time steps)
        ↓
LSTM Layer 1 (50 units) → Dropout (20%)
        ↓
LSTM Layer 2 (50 units) → Dropout (20%)
        ↓
LSTM Layer 3 (50 units) → Dropout (20%)
        ↓
LSTM Layer 4 (50 units) → Dropout (20%)
        ↓
Dense Layer (1 unit)
        ↓
Output (predicted price)
```

### Chi Tiết Tham Số

| Tham Số | Giá Trị | Giải Thích |
|---------|--------|-----------|
| **Time Steps** | 60 | Sử dụng 60 ngày quá khứ để dự báo ngày tới |
| **LSTM Units** | 50 | Số lượng hidden state trong mỗi lớp |
| **Dropout Rate** | 0.2 | Tỷ lệ dropout để regularization |
| **Epochs** | 100 | Số lần huấn luyện trên toàn bộ dataset |
| **Batch Size** | 32 | Kích thước batch mỗi lần cập nhật |
| **Optimizer** | Adam | Thuật toán tối ưu |
| **Loss Function** | MSE | Mean Squared Error |

---

## 📊 Quy Trình Hoạt Động

### 1. **Chuẩn Bị Dữ Liệu**
```
Đọc CSV → Scale [0,1] → Tạo sequences (X, y)
```

### 2. **Huấn Luyện**
```
Nếu model tồn tại → Load model
Nếu chưa → Huấn luyện 100 epochs → Lưu model
```

### 3. **Dự Báo Trên Test Set (2019)**
```
Đọc dữ liệu 2019 → Dùng model dự báo → So sánh với giá thực
```

### 4. **Dự Báo Phía Trước 28 Ngày**
```
Vòng lặp 28 lần:
  ├── Lấy 60 ngày gần nhất
  ├── Dự báo ngày tiếp theo
  ├── Thêm vào dataset
  └── In ra kết quả
```

---

## 📈 Ví Dụ Output

```
Biểu đồ: So sánh giá thực tế (red) vs giá dự báo (blue)

Stock price 3/3/2020 of VNINDEX :  890.45
Stock price 4/3/2020 of VNINDEX :  895.32
Stock price 5/3/2020 of VNINDEX :  898.12
...
```

---

## 🔧 Cải Tiến Trong Tương Lai

- [ ] **Thêm nhiều biến đầu vào**: Volume giao dịch, các chỉ số kỹ thuật khác
- [ ] **Tối ưu hóa siêu tham số**: Grid Search, Random Search
- [ ] **So sánh các mô hình**: GRU, Transformer, Hybrid models
- [ ] **Thêm tính năng xác thực**: Cross-validation, backtest
- [ ] **Giao diện web**: Streamlit hoặc Flask UI để tương tác
- [ ] **Cấu hình API**: REST API để dự báo theo yêu cầu
- [ ] **Cập nhật dữ liệu tự động**: Kết nối API chứng khoán real-time
- [ ] **Đánh giá mô hình**: MAE, RMSE, MAPE metrics
- [ ] **Ensemble Methods**: Kết hợp nhiều mô hình

---

## 📝 Ghi Chú

- **Dữ liệu**: Sử dụng dữ liệu lịch sử VN-Index từ 2000-2019
- **Model Training**: Nếu `mymodel.h5` tồn tại, script sẽ load model thay vì huấn luyện lại
- **Thời gian Execution**: Lần đầu tiên (~5-10 phút), lần tiếp theo (~2-3 phút)
- **Dự báo**: Dự báo 28 ngày tiếp theo từ ngày 2/10/2019


---

## ⚠️ Lưu Ý

- **Rủi ro**: Dự báo thị trường không bao giờ 100% chính xác. Không sử dụng để quyết định đầu tư thực tế.
- **Dữ liệu quá cũ**: Dataset được sử dụng từ năm 2000-2019, có thể không phản ánh chính xác tình hình hiện tại.
- **Cải tiến cần thiết**: Cần thêm dữ liệu mới, tối ưu hóa mô hình, và xác thực toàn diện trước khi sử dụng trong thực tiễn.

---

**⭐ Nếu dự án hữu ích, vui lòng cho một Star!** ⭐
