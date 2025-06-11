# Dự đoán giá nhà ở thành phố Hồ Chí Minh sử dụng các mô hình máy học bằng dữ liệu thu thập từ web
## Data Collection  
Tổng quan:   
- Ở bước này, ta sẽ sử dụng Request để mở đường dẫn đến Web chứa Data cần thu thập, Beautiful Soup để truy xuất Html của trang Web, lưu vào các List 
được khởi tạo và sau khi hoàn thành, lưu lại dưới dạng một File CSV để sử dụng về sau.

## Anyscale
Tổng quan: Ở giai đoạn này, ta sẽ thực hiện:
- Bằng cách xem xét các chuỗi trong phần Mô tả, ta nhận thấy ở đây sẽ có thông tin của các thuộc tính như: PN, WC, Diện tích, Giá
và một số Keyword mới mà ta có thể khai thác làm thuộc tính như: Tầng (Floor), Sổ sách/Sổ hồng/Pháp lý. 
- Ta sẽ tìm cách để Fill các giá trị NULL trong các thuộc tính như: PN, WC, và tạo các thuộc tính mới bằng thông tin xử lý ở phần mô tả bằng Anyscale.

## Data Preprocessing and Feature Engineering
Tổng quan: Ở giai đoạn này, ta sẽ thực hiện:
- Nếu các giá trị từ Dataframe ban đầu là null, ta sẽ điền các giá trị từ bên phải (Dữ liệu lấy từ Mô tả) vừa xây dựng bên trên vào
- Xây dựng và áp dụng các hàm chuyển đổi các Thuộc tính như: Area (Diện tích), PN (Số Phòng ngủ),... từ dạng object (string)
về dạng số
- Xóa bỏ các Duplicates
- Nhận thấy trực quan các giá trị Price có đơn vị chưa chính xác, ta sẽ Format các giá trị về bằng cách đưa các số lớn hơn > 1 tỷ / 1 tỷ, >1 triệu / 1 triệu,...
- Xử lý các Outliers (của Price, WC, PN, Area) bằng Percentile.
- Fill các giá trị null của các thuộc tính số bằng K-Neighbor Regressor
- Xử lý các giá trị Categorical:
  + Chuyển dạng một vài giá trị của thuộc tính "SoSach" về dạng "Có", "Không", sau đó chuyển các giá trị chưa biết thành "Unknown".
  + Dùng Target Encoding cho các thuộc tính "Type" (Loại nhà), "district" (Vị trí), "SoSach" (Pháp lý).
- Trực quan hóa các thuộc tính để xem xét phân phối của thuộc tính.
- Vẽ Heatmap để xem xét Correlation giữa các thuộc tính, sau đó loại bỏ các thuộc tính không cần thiết.

## Modeling and Evaluation
Tổng quan: Ở giai đoạn này, ta sẽ:
- Chia dữ liệu thành tập X (Gồm các thuộc tính hay biến giải thích: Diện tích, Số phòng ngủ,...) và y (Gồm biến phụ thuộc: Price).
- Tiến hành chạy một số mô hình lên dữ liệu bao gồm:
  + Linear Regression.
  + Ridge Regression.
  + Random Forest Regressor.
  + XGBoost Regressor.
- Dùng các chỉ số đánh giá các mô hình:
  + R2 Score.
  + Mean Square Error.
- Qua thử nghiệm thấy được:
  + Random Forest Regression (R2 Score: 0.61, MSE: 3.72): Mô hình RFR cho hiệu quả khá tốt khi có thể hoạt động tốt với các dữ liệu phi tuyến, sử dụng nhiều Decision Tree để đưa ra quyết định nên giảm thiểu Over và Underfitting, cải thiện độ chính xác và ổn định.
  + XGBoost Regressor (R2 Score: 0.62, MSE: 3.65): Mô hình XGB cho hiệu quả tốt nhất khi vừa hoạt động tốt với dữ liệu phi tuyến, vừa kết hợp của các cây quyết định và các Regularization để giảm thiểu Overfitting.
- So sánh các chỉ số của các mô hình.
## Một vài thông tin thêm:
- Biểu đồ Heatmap thể hiện phân bố số lượng nhà được đăng bán tại khu vực Tp HCM.
