# Nghiên cứu LTC trong phát hiện việc rửa tay
So sánh LTC với các mạng RNN khác

Dataset source: https://www.kaggle.com/realtimear/hand-wash-dataset

Bài báo LTC: https://github.com/raminmh/liquid_time_constant_networks/tree/master

# Cách chạy code

### Huấn luyện mô hình

- Đến thư muc src:
    1. Xử lý dữ liệu (sửa lại thuộc tính `REGENERATE_LAYER1_DATA` và `REGENERATE_LAYER2_DATA` thành **True** )
        
        > `python main.py -l preprocess`
        Nó sẽ tạo ra 1 danh sách dữ liệu cho cả 2 Layer cho tập dữ liệu train/valid/test, danh sách sẽ được ghi vào file. Sau đó dựa trên danh sách này sẽ tạo ra 2 thư mục data/layer_<1/2>_data

        Ở layer1, dữ liệu được đưa vào model sẽ nằm trong thư mục  data/layer_<1/2>_data

        Ở layer1, dữ liệu được đưa vào model sẽ nằm trong thư mục  LAYER_2_TRAIN_PROCESSED_PATH

    2. Huấn luyện layer1 (Ví dụ: layer1).

        > `python main.py -l layer1`

        Câu lệnh này sẽ bắt đầu huấn luyện và thử nghiệm cho layer1

        Ở layer1, đầu vào dữ liệu sẽ có kích thước: **(number_of_frame, batch_size, number_of_feature)**
        
    3. Huấn luyện lớp2 (i.e layer2). 

       Câu lệnh này sẽ bắt đầu huấn luyện và thử nghiệm cho layer2

        > `python main.py -l layer2`

        Ở layer2, đầu vào dữ liệu sẽ có kích thước: **(number_of_sample, 12)**

        Nó chứa phần trăm likelihood của 6 bước và điểm số tương ứng của nó.
e
        Chúng ta có thể không kích hoạt chế độ train/test bằng cachs thêm --train false --test false. Mặc định là true


- The property list is in **/src/property.py**. 

### Đánh giá trên tập dữ liệu đơn  
Sử dụng câu lệnh: 

> `python main.py --eval true --path <video_path>`

### Kết quả
* Kết quả của mỗi cây sẽ được lưu ở đường dẫn: *results/logs/<**layer1_model_type**>/<**layer1_model_size**>\_<**layer1_model_type**>\_<**tree_id**>/csv_file*

* Kết quả layer1 sẽ được lưu ở đường dẫn: *results/logs/<**layer1_model_type**>/forest_size<**layer1_model_size**>.csv

* Kết quả layer2 sẽ được lưu ở đường dẫn: *results/logs/<**layer1_model_type**>/<**layer2_model_type**>\_<**layer2_epoch**>_result.csv

* Bộ trọng số của mô hình sẽ được lưu ở: *data/trained-model/*
    *  **model-** thư mục là bộ trọng số của các cây
    *  **.pkl** file là bộ trọng số của layer2 
