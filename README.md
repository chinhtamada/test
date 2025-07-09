Chắc chắn rồi, dưới đây là bản dịch toàn bộ nội dung tài liệu nguồn sang tiếng Việt, giữ nguyên định dạng và các tham chiếu nguồn:

{
NEW SOURCE
Trích đoạn từ "8.製造依頼.md":


### 8. Yêu cầu sản xuất
### Wireframe
Xem trước
#### Tổng quan
Tham chiếu
#### Điểm cuối (Endpoint)
/manufacture_request ※Số đơn đặt hàng được kế thừa từ màn hình trước thông qua tham số.
#### Hiển thị ban đầu
1.  **Tạo các menu thả xuống từ master code** (chỉ cho mặt bích của chi tiết yêu cầu sản xuất).
    10. Tham khảo menu thả xuống master code.
2.  Hiển thị thông tin đơn đặt hàng (phần accordion).
    1.  Tham khảo hiển thị accordion.
3.  **Hiển thị chi tiết yêu cầu sản xuất**.
    1.  Lấy dữ liệu mục tiêu của thông tin chi tiết đơn đặt hàng. Sử dụng truy vấn `getOrderDetailsByOrderNo` của API chi tiết đơn đặt hàng số 5 để lấy dữ liệu mục tiêu. Nếu có dữ liệu trên nhiều trang, lặp lại cuộc gọi API tăng dần `page` cho đến khi đạt "tổng số trang (totalPages)" và lấy toàn bộ "tổng số phần tử (totalElements)".
        *   `order_no`: Số đơn đặt hàng được kế thừa từ màn hình trước.
        *   `sort_column`: "orderNo, detailNo" ※Định dạng camelCase.
        *   `order`: "asc".
        *   `page`: Chỉ định từ 0 đến tổng số trang (totalPages).
        *   `size`: Cố định 100.
        *   Tham khảo phần ghi chú của sheet chi tiết đơn đặt hàng để biết cách ánh xạ từng mục.
            *   **Nếu có 1 hoặc nhiều chi tiết đơn đặt hàng**, chỉ lọc các chi tiết có ký tự đầu tiên của mã sản phẩm (`product_code`) là '2' để tạo chi tiết yêu cầu sản xuất. Đặt các giá trị mục dựa trên các giá trị bảng của chi tiết đơn đặt hàng, thông số kỹ thuật sản xuất và sản phẩm.
                *   "No": Đặt chi tiết đơn đặt hàng. Số chi tiết (`detail_no`).
                *   Đặt các giá trị mục sau dựa trên chi tiết đơn đặt hàng:
                    *   "Mã sản phẩm": Đặt chi tiết đơn đặt hàng. Mã sản phẩm (`product_code`).
                    *   "Tên sản phẩm", "Thông số kỹ thuật": Đặt sản phẩm. Tên sản phẩm (`name`), sản phẩm. Thông số kỹ thuật (`spec`) được lấy bằng API sản phẩm.
                        53. Sử dụng truy vấn `getProductByCode` của API sản phẩm số 53 để lấy dữ liệu bản ghi mục tiêu.
                        *   `code`: Chi tiết đơn đặt hàng. Mã sản phẩm (`product_code`).
                        *   Tham khảo phần ghi chú của sheet sản phẩm để biết cách ánh xạ từng mục.
                    *   "Đơn giá dự toán thực hiện": Xác định giá trị cài đặt theo điều kiện dưới đây và đặt theo định dạng phân cách dấu ',' mỗi 3 chữ số (ví dụ: 1,500).
                        *   Nếu chi tiết đơn đặt hàng. Số lượng (`num`) khác '0', tính (`chi tiết đơn đặt hàng`.`dự toán thực hiện` / `chi tiết đơn đặt hàng`.`số lượng`) và đặt giá trị làm tròn xuống không có số thập phân.
                        *   Nếu chi tiết đơn đặt hàng. Số lượng (`num`) là '0', đặt chi tiết đơn đặt hàng. Dự toán thực hiện (`budget`).
                    *   "Số tiền dự toán thực hiện": Đặt chi tiết đơn đặt hàng. Dự toán thực hiện (`budget`) theo định dạng phân cách dấu ',' mỗi 3 chữ số (ví dụ: 1,500).
                *   Đặt các giá trị mục sau dựa trên thông tin thông số kỹ thuật sản xuất:
                    Thông tin thông số kỹ thuật sản xuất đã lấy cũng được sử dụng trong hộp thoại đăng ký thông số kỹ thuật (bồn hoặc bể nước) được hiển thị khi nhấn nút đăng ký thông số kỹ thuật trên chi tiết yêu cầu sản xuất.
                    *   Sử dụng truy vấn `getManufactureSpecifications` của API thông số kỹ thuật sản xuất số 8 để lấy danh sách thông số kỹ thuật sản xuất liên kết với số đơn đặt hàng.
                        *   `order_no`: Chi tiết đơn đặt hàng. Số đơn đặt hàng (`order_no`).
                        *   `detail_no`: Chi tiết đơn đặt hàng. Số chi tiết (`detail_no`).
                        *   Tham khảo phần ghi chú của sheet thông số kỹ thuật sản xuất để biết cách ánh xạ từng mục.
                *   "Dung tích": Đặt thông số kỹ thuật sản xuất. Dung tích (`capacity`) với 2 chữ số thập phân và định dạng phân cách dấu ',' mỗi 3 chữ số (ví dụ: 1,230.01).
                *   "Ngăn/Hố": Xác định giá trị cài đặt theo điều kiện dưới đây.
                    *   Nếu độ dài mã sản phẩm (`product_code`) của chi tiết đơn đặt hàng nhỏ hơn hoặc bằng 2 chữ số hoặc ký tự đầu tiên của mã sản phẩm không phải '2', đặt trống ('').
                    *   Trong các trường hợp khác:
                        Xác định giá trị cài đặt theo loại bồn của sản phẩm (`code`.`kind1`) được lấy bằng API mã.
                        *   Lấy loại bồn.
                            38. Sử dụng truy vấn `getCodes` của API mã số 38 để chỉ định và lấy `kind1`.
                            *   `kind_code`: TANK.
                            *   `code`: 2 chữ số từ ký tự thứ 2 đến thứ 3 của mã sản phẩm (`product_code`) của chi tiết đơn đặt hàng.
                            *   Tham khảo phần ghi chú của sheet mã để biết cách ánh xạ từng mục.
                        *   Kiểm tra loại bồn (`code`.`kind1`) và xác định giá trị cài đặt.
                            *   "Không có liên quan": Đặt trống ('').
                            *   Nếu là '1': Thông số kỹ thuật sản xuất. Ngăn (`partition`).
                            *   Nếu là '2': Thông số kỹ thuật sản xuất. Hố (`pit`).
                *   "Đường kính": Xác định giá trị cài đặt theo điều kiện dưới đây.
                    *   Nếu thông số kỹ thuật sản xuất. Đường kính (`diameter`) là '0':
                        *   Đặt đường kính lấy từ API sản phẩm.
                            53. Sử dụng truy vấn `getProductByCode` của API sản phẩm số 53 để lấy dữ liệu bản ghi mục tiêu.
                            *   `code`: Chi tiết đơn đặt hàng. Mã sản phẩm (`product_code`).
                            *   Tham khảo phần ghi chú của sheet sản phẩm để biết cách ánh xạ từng mục.
                    *   Nếu thông số kỹ thuật sản xuất. Đường kính (`diameter`) khác '0':
                        *   Đặt thông số kỹ thuật sản xuất. Đường kính (`diameter`) theo định dạng phân cách dấu ',' mỗi 3 chữ số (ví dụ: 1,500).
                *   "Chiều dài thân": Đặt thông số kỹ thuật sản xuất. Chiều dài thân (`torso_length`) theo định dạng phân cách dấu ',' mỗi 3 chữ số (ví dụ: 1,500).
                *   "Độ dày vỏ": Đặt thông số kỹ thuật sản xuất. Độ dày tấm (`plate_thickness`) với 1 chữ số thập phân và định dạng phân cách dấu ',' mỗi 3 chữ số (ví dụ: 1,500.7).
                *   "KHK": Tham khảo dưới đây.
                    *   Nếu 2 chữ số từ ký tự thứ 2 đến thứ 3 của mã sản phẩm (`product_code`) của chi tiết đơn đặt hàng là "01", thực hiện như sau:
                        1.  Lấy thông tin bồn.
                            23. Sử dụng truy vấn `getTankByNo` của API bồn số 23 để lấy thông tin bồn.
                            *   `figure_no`: Thông số kỹ thuật sản xuất. Số bản vẽ (`drawing_number`). ※Nếu số bản vẽ chứa '△', thì số bản vẽ là các ký tự từ đầu số bản vẽ cho đến trước '△'.
                            *   Tham khảo phần ghi chú của sheet bồn để biết cách ánh xạ từng mục.
                        2.  Nếu thông tin bồn được lấy, thực hiện như sau: Kiểm tra thông tin bồn và xác định giá trị cài đặt.
                            *   Sử dụng truy vấn `getCodes` của API mã số 38 để lấy `kind1` tương ứng dựa trên bồn. Loại hình thức (`zu24_code`) và bồn. Loại vật liệu (`zu23_code`).
                                *   `kind_code`: ZU24, ZU23.
                                *   `code`: Bồn. Loại hình thức (`zu24_code`), Bồn. Loại vật liệu (`zu23_code`).
                                *   Tham khảo phần ghi chú của sheet mã để biết cách ánh xạ từng mục.
                            *   Nếu `code`.`kind1` (loại hình thức) = "00001" hoặc (`code`.`kind1` (loại hình thức) <> "00001" VÀ `code`.`kind1` (loại vật liệu) = "00001"), thực hiện như sau:
                                *   9. Sử dụng truy vấn `getkhkManagements` của API quản lý KHK số 9 để lấy thông tin KHK.
                                    *   `capacity`: Bồn. Dung tích (`capacity`).
                                    *   `bore`: Bồn. Đường kính trong (`bore`).
                                    *   `torso_length`: Bồn. Chiều dài thân (`torso_length`).
                                    *   `adhesion_width`: Bồn. Chiều rộng tiếp xúc (`adhesion_width`).
                                    *   Tham khảo phần ghi chú của sheet quản lý KHK để biết cách ánh xạ từng mục.
                                *   Nếu thông tin KHK được lấy, lọc theo `KHK管理`.`削除フラグ` (`delete_flag`) <> '1' và thực hiện như sau:
                                    *   **Chỉnh sửa tên nhà máy**.
                                        Với mỗi thông tin KHK đã lấy, nối các ký tự đầu tiên của tên nhà máy lấy từ API mã theo thứ tự, phân cách bằng dấu phẩy (',').
                                        Tuy nhiên, nếu ký tự lấy được là "旧" (cũ), thì giá trị đã nối cho đến trước "旧" sẽ bị hủy, và bắt đầu nối lại từ "旧".
                                        *   38. Sử dụng truy vấn `getCodes` của API mã số 38 để lấy ký tự 1.
                                    *   Nếu kết quả chỉnh sửa tên nhà máy cho thông tin KHK đã hoàn thành có thông tin, đặt kết quả chỉnh sửa.
                                    *   Nếu kết quả chỉnh sửa tên nhà máy không có thông tin (''), thêm khóa và lấy lại thông tin KHK.
                                        *   9. Sử dụng truy vấn `getkhkManagements` của API quản lý KHK số 9 để lấy lại thông tin KHK.
                                            *   `capacity`: Bồn. Dung tích (`capacity`).
                                            *   `bore`: Bồn. Đường kính trong (`bore`).
                                            *   `torso_length`: Bồn. Chiều dài thân (`torso_length`).
                                            *   `adhesion_width`: '0'.
                                            *   Tham khảo phần ghi chú của sheet quản lý KHK để biết cách ánh xạ từng mục.
                                        *   Nếu thông tin KHK được lấy lại, lọc theo `KHK管理`.`削除フラグ` (`delete_flag`) <> '1' và thực hiện như sau:
                                            *   Chỉnh sửa tên nhà máy.
                                                Tham khảo phần chỉnh sửa tên nhà máy ở trên.
                                            *   Nếu kết quả chỉnh sửa tên nhà máy cho thông tin KHK đã lấy lại có thông tin, đặt kết quả chỉnh sửa.
                                            *   Nếu kết quả chỉnh sửa tên nhà máy trong lần lấy lại không có thông tin (''), đặt '無' (không có).
                        3.  Nếu thông tin bồn không được lấy, thực hiện như sau:
                            1.  9. Sử dụng truy vấn `getkhkManagements` của API quản lý KHK số 9 để lấy thông tin KHK.
                                *   `capacity`: Thông số kỹ thuật sản xuất. Dung tích (`capacity`).
                                *   `bore`: Thông số kỹ thuật sản xuất. Đường kính (`diameter`).
                                *   `torso_length`: Thông số kỹ thuật sản xuất. Chiều dài thân (`torso_length`).
                                *   Tham khảo phần ghi chú của sheet quản lý KHK để biết cách ánh xạ từng mục.
                            2.  Nếu thông tin KHK được lấy, lọc theo `KHK管理`.`削除フラグ` (`delete_flag`) <> '1' và thực hiện như sau:
                                *   Chỉnh sửa tên nhà máy.
                                    Tham khảo phần chỉnh sửa tên nhà máy ở trên.
                                *   Nếu kết quả chỉnh sửa tên nhà máy cho thông tin KHK đã hoàn thành có thông tin, đặt kết quả chỉnh sửa.
                                *   Nếu kết quả chỉnh sửa tên nhà máy không có thông tin (''), thêm khóa và lấy lại thông tin KHK.
                                    9. Sử dụng truy vấn `getkhkManagements` của API quản lý KHK số 9 để lấy lại thông tin KHK.
                                    *   `capacity`: Thông số kỹ thuật sản xuất. Dung tích (`capacity`).
                                    *   `bore`: Thông số kỹ thuật sản xuất. Đường kính (`diameter`).
                                    *   `torso_length`: Thông số kỹ thuật sản xuất. Chiều dài thân (`torso_length`).
                                    *   `adhesion_width`: '0'.
                                    *   Tham khảo phần ghi chú của sheet quản lý KHK để biết cách ánh xạ từng mục.
                                *   Nếu thông tin KHK được lấy lại, lọc theo `KHK管理`.`削除フラグ` (`delete_flag`) <> '1' và thực hiện như sau:
                                    *   Chỉnh sửa tên nhà máy.
                                        Tham khảo phần chỉnh sửa tên nhà máy ở trên.
                                    *   Nếu kết quả chỉnh sửa tên nhà máy cho thông tin KHK đã lấy lại có thông tin, đặt kết quả chỉnh sửa.
                                    *   Nếu kết quả chỉnh sửa tên nhà máy trong lần lấy lại không có thông tin (''), đặt '無' (không có).
                    *   "Chiều cao vòi": Thông số kỹ thuật sản xuất. Chiều cao vòi (`nozzle_height`).
                    *   "Mặt bích": Chọn thông số kỹ thuật sản xuất. Tiêu chuẩn mặt bích (`frng_code`).
                    *   "Số lượng": Xác định giá trị cài đặt theo điều kiện dưới đây và đặt theo định dạng phân cách dấu ',' mỗi 3 chữ số (ví dụ: 1,500).


*   Nếu thông số kỹ thuật sản xuất. Số lượng (`quantity`) khác "" (chưa cài đặt), đặt thông số kỹ thuật sản xuất. Số lượng (`quantity`).
*   Nếu thông số kỹ thuật sản xuất. Số lượng (`quantity`) là "" (chưa cài đặt), đặt chi tiết đơn đặt hàng. Số lượng (`num`).
*   "Đơn giá đặt hàng": Xác định giá trị cài đặt theo điều kiện dưới đây và đặt theo định dạng phân cách dấu ',' mỗi 3 chữ số (ví dụ: 1,500).
*   Nếu thông số kỹ thuật sản xuất. Đơn giá đặt hàng (`order_unit`) khác "" (chưa cài đặt), đặt thông số kỹ thuật sản xuất. Đơn giá đặt hàng (`order_unit`).
*   Nếu thông số kỹ thuật sản xuất. Đơn giá đặt hàng (`order_unit`) là "" (chưa cài đặt), đặt chi tiết đơn đặt hàng. Đơn giá (`unit`).
*   "Số tiền đặt hàng": Xác định giá trị cài đặt theo điều kiện dưới đây và đặt theo định dạng phân cách dấu ',' mỗi 3 chữ số (ví dụ: 1,500).
*   Nếu thông số kỹ thuật sản xuất. Số tiền đặt hàng (`order_amount`) khác "" (chưa cài đặt), đặt thông số kỹ thuật sản xuất. Số tiền đặt hàng (`order_amount`).
*   Nếu thông số kỹ thuật sản xuất. Số tiền đặt hàng (`order_amount`) là "" (chưa cài đặt), đặt chi tiết đơn đặt hàng. Số tiền (`amount`).
*   "Đơn giá yêu cầu sản xuất": Xác định giá trị cài đặt theo điều kiện dưới đây và đặt theo định dạng phân cách dấu ',' mỗi 3 chữ số (ví dụ: 1,500).
*   Nếu thông số kỹ thuật sản xuất. Đơn giá yêu cầu sản xuất (`manufacture_unit`) khác "" (chưa cài đặt), đặt thông số kỹ thuật sản xuất. Đơn giá yêu cầu sản xuất (`manufacture_unit`).
*   Nếu thông số kỹ thuật sản xuất. Đơn giá yêu cầu sản xuất (`manufacture_unit`) là "" (chưa cài đặt), đặt đơn giá dự toán thực hiện được tính từ chi tiết đơn đặt hàng trong cùng chi tiết.
*   "Số tiền yêu cầu sản xuất": Xác định giá trị cài đặt theo điều kiện dưới đây và đặt theo định dạng phân cách dấu ',' mỗi 3 chữ số (ví dụ: 1,500).
*   Nếu thông số kỹ thuật sản xuất. Số tiền yêu cầu sản xuất (`manufacture_amount`) khác "" (chưa cài đặt), đặt thông số kỹ thuật sản xuất. Số tiền yêu cầu sản xuất (`manufacture_amount`).
*   Nếu thông số kỹ thuật sản xuất. Số tiền yêu cầu sản xuất (`manufacture_amount`) là "" (chưa cài đặt), đặt chi tiết đơn đặt hàng. Dự toán thực hiện (`budget`).
*   "Số báo giá tích lũy": Đặt thông số kỹ thuật sản xuất. Số báo giá tích lũy (`cost_estimation_number`).
*   "Số bản vẽ (bồn/bể nước)": Đặt thông số kỹ thuật sản xuất. Số bản vẽ (`drawing_number`).
*   "Số bản vẽ (chôn ngầm)": Đặt thông số kỹ thuật sản xuất. Số bản vẽ chôn ngầm (`buried_drawing_number`).
*   **Nếu chi tiết đơn đặt hàng là 0 mục**:
    *   Đặt các mục nhập sau của chi tiết yêu cầu sản xuất không thể nhập và vô hiệu hóa nút "Đăng ký thông số kỹ thuật".
        *   "Dung tích", "Ngăn/Hố", "Đường kính", "Chiều dài thân", "Độ dày vỏ", "Chiều cao vòi", "Mặt bích", "Số lượng", "Đơn giá đặt hàng", "Số tiền đặt hàng".
           "Đơn giá dự toán thực hiện", "Số tiền dự toán thực hiện", "Đơn giá yêu cầu sản xuất", "Số tiền yêu cầu sản xuất", "Số báo giá tích lũy".
4.  Hiển thị thông tin đơn đặt hàng (phần tab).
    Đặt các mục sau dựa trên thông tin đơn đặt hàng đã lấy ở mục 1 và thông tin chi tiết yêu cầu sản xuất đã lấy ở mục 2.
    *   "Số tiền đặt hàng": Đặt đơn đặt hàng. Số tiền đặt hàng (`order_amount`).
    *   "Tổng số tiền đặt hàng chi tiết": Tổng số tiền đặt hàng trong chi tiết yêu cầu sản xuất.
    *   "Tổng số tiền yêu cầu sản xuất": Tổng số tiền yêu cầu sản xuất trong chi tiết yêu cầu sản xuất.


#### 1. Hiển thị Accordion
Lấy dữ liệu mục tiêu của thông tin đơn đặt hàng. Tham khảo "1. Lấy dữ liệu mục tiêu của thông tin đơn đặt hàng" trong xử lý chung 2 của nhập đơn đặt hàng.
*   Nếu hoàn thành bình thường, thực hiện các thao tác sau dựa trên dữ liệu đã lấy:
    *   "Số đơn đặt hàng": Đặt đơn đặt hàng. Số (`no`).
    *   Đối với các mục dưới đây, đặt tên viết tắt (`ryaku_name`) được lấy bằng API mã:
        "Loại đơn đặt hàng", "Loại chi tiết đơn đặt hàng", "Hình thức đơn đặt hàng".
        38. Sử dụng truy vấn `getCodes` của API mã số 38 để lấy tên viết tắt.
        *   `kind_code`: JUCY (loại đơn đặt hàng), SYSI (loại chi tiết đơn đặt hàng), JUKE (hình thức đơn đặt hàng).
        *   `code`: Loại đơn đặt hàng: đơn đặt hàng. Loại đơn đặt hàng (`jycy_code`), Loại chi tiết đơn đặt hàng: đơn đặt hàng. Chi tiết phân loại (`sysi_code`), Hình thức đơn đặt hàng: đơn đặt hàng. Hình thức đơn đặt hàng (`juke_code`).
        *   Tham khảo phần ghi chú của sheet mã để biết cách ánh xạ từng mục.
    *   "Chi nhánh quản lý": Đặt chi nhánh. Tên chi nhánh (`name`) được lấy bằng API chi nhánh.
        40. Sử dụng truy vấn `getBranches` của API chi nhánh số 40 để lấy tên chi nhánh (`name`).
        *   `code`: Đơn đặt hàng. Mã chi nhánh quản lý (`branch_code`).
    *   "Phòng ban phụ trách": Đặt phòng ban. Tên phòng ban (`name`) được lấy bằng API phòng ban.
        42. Sử dụng truy vấn `getDepartments` của API phòng ban số 42 để lấy tên phòng ban (`name`).
        *   `code`: Đơn đặt hàng. Phòng ban phụ trách.
        *   Tham khảo phần ghi chú của sheet phòng ban để biết cách ánh xạ từng mục.
    *   Đối với các mục dưới đây, đặt tên nhân viên được lấy bằng API nhân viên:
        "Người phụ trách", "Người phụ trách công trình".
        41. Sử dụng truy vấn `getEmployees` của API nhân viên số 41 để lấy tên nhân viên (`name`).
        *   `code`: Người phụ trách: đơn đặt hàng. Mã người phụ trách (`owner_code`), Người phụ trách công trình: đơn đặt hàng. Mã người thực hiện (`worker_code`).
        *   Tham khảo phần ghi chú của sheet nhân viên để biết cách ánh xạ từng mục.
    *   "Tên khách hàng đặt hàng": Đặt đơn đặt hàng. Tên điểm đến đặt hàng (`order_dest_name`).
    *   "Địa chỉ khách hàng đặt hàng (mã bưu điện)": Đặt đơn đặt hàng. Mã bưu điện điểm đến đặt hàng (`order_dest_postcode`).
    *   "Địa chỉ khách hàng đặt hàng (địa chỉ)": Đặt đơn đặt hàng. Địa chỉ điểm đến đặt hàng (`order_dest_address`).
    *   "Ngày đặt hàng": Đặt đơn đặt hàng. Ngày (`date`) theo định dạng YYYY/MM/DD.
    *   "Phòng ban phụ trách khách hàng đặt hàng": Đặt đơn đặt hàng. Phòng ban phụ trách điểm đến đặt hàng (`order_dest_department`).
    *   "Người phụ trách khách hàng đặt hàng": Đặt đơn đặt hàng. Người phụ trách điểm đến đặt hàng (`order_dest_owner`).
    *   "Số điện thoại khách hàng đặt hàng": Đặt đơn đặt hàng. Số điện thoại điểm đến đặt hàng (`order_dest_tel`).
    *   "Số FAX khách hàng đặt hàng": Đặt đơn đặt hàng. Số FAX điểm đến đặt hàng (`order_dest_fax`).
    *   "Ngày bàn giao công trình": Đặt đơn đặt hàng. Ngày bàn giao công trình (`work_delivery_at`) theo định dạng YYYY/MM/DD.
    *   "Thời gian thi công (bắt đầu)": Đặt đơn đặt hàng. Ngày bắt đầu thi công (`work_start_at`) theo định dạng YYYY/MM/DD.
    *   "Thời gian thi công (kết thúc)": Đặt đơn đặt hàng. Ngày kết thúc thi công (`work_end_at`) theo định dạng YYYY/MM/DD.
    *   "Ngày bàn giao bồn": Đặt đơn đặt hàng. Ngày bàn giao bồn (`tank_delivery_at`) theo định dạng YYYY/MM/DD.
    *   "Quyết định bàn giao bồn": Nếu đơn đặt hàng. Quyết định bàn giao (`is_delivery`) là '1', đặt "Quyết định". Nếu không phải '1', đặt trống ('').
    *   "Ngày bàn giao bồn 2": Đặt đơn đặt hàng. Ngày bàn giao bồn 2 (`tank_delivery2_at`) theo định dạng YYYY/MM/DD.
    *   "Quyết định bàn giao bồn 2": Nếu đơn đặt hàng. Quyết định bàn giao 2 (`is_delivery2`) là '1', đặt "Quyết định". Nếu không phải '1', đặt trống ('').
    *   "Ngày bàn giao bồn 3": Đặt đơn đặt hàng. Ngày bàn giao bồn 3 (`tank_delivery3_at`) theo định dạng YYYY/MM/DD.
    *   "Quyết định bàn giao bồn 3": Nếu đơn đặt hàng. Quyết định bàn giao 3 (`is_delivery3`) là '1', đặt "Quyết định". Nếu không phải '1', đặt trống ('').
    *   "Cơ quan quản lý": Đặt đơn đặt hàng. Cơ quan hành chính tại địa điểm (`site_gov`).
    *   "Tên địa điểm": Đặt đơn đặt hàng. Tên địa điểm (`site_name`).
    *   "Số điện thoại địa điểm": Đặt đơn đặt hàng. Số điện thoại địa điểm (`site_tel`).
    *   "Số FAX địa điểm": Đặt đơn đặt hàng. Số FAX địa điểm (`site_fax`).
    *   "Địa điểm (mã bưu điện)": Đặt đơn đặt hàng. Mã bưu điện địa điểm (`site_postcode`).
    *   "Địa điểm (địa chỉ)": Đặt đơn đặt hàng. Địa chỉ địa điểm (`site_address`).
    *   "Khu vực địa điểm": Đặt đơn đặt hàng. Khu vực kích thước (`size_area`).
    *   "Ghi chú đặc biệt": Đặt đơn đặt hàng. Ghi chú đặc biệt (`special_remark`).
*   Nếu hoàn thành bất thường:
    *   Nếu "Không có dữ liệu phù hợp":
        *   Hiển thị thông báo lỗi "Không thể lấy dữ liệu đơn đặt hàng." sau đó kết thúc lỗi.
    *   Nếu "Không phải không có dữ liệu phù hợp":
        *   Hiển thị thông báo lỗi hệ thống sau đó kết thúc lỗi.


#### 2. Lưu tạm thời
1.  **Kiểm tra sơ bộ**.
    Trong chi tiết yêu cầu sản xuất, thực hiện các kiểm tra sau và nếu có nhiều lỗi, hiển thị thông báo lỗi tổng hợp trong một modal.
    1.  **Kiểm tra tương quan giữa "Số lượng" và số lượng yêu cầu sản xuất**.
        *   Lấy số lượng bản ghi từ bảng yêu cầu sản xuất dựa trên các điều kiện sau:
            *   Điều kiện: (`yêu cầu sản xuất`.`số đơn đặt hàng` (`order_no`) = `số đơn đặt hàng` liên kết với `chi tiết yêu cầu sản xuất` VÀ `yêu cầu sản xuất`.`số chi tiết` (`detail_no`) = `số chi tiết` liên kết với `chi tiết yêu cầu sản xuất`).
        *   Nếu số lượng yêu cầu sản xuất đã lấy lớn hơn hoặc bằng 1 VÀ số lượng yêu cầu sản xuất > "Số lượng" đã nhập vào chi tiết yêu cầu sản xuất, hiển thị thông báo lỗi "Có lỗi trong nhập số lượng.".
    2.  **Kiểm tra loại bồn và thực hiện các kiểm tra sau**:
        *   Lấy loại bồn của sản phẩm (`code`.`kind1`) được lấy bằng API mã.
            *   Lấy loại bồn.
                38. Sử dụng truy vấn `getCodes` của API mã số 38 để chỉ định và lấy `kind1`.
                *   `kind_code`: TANK.
                *   `code`: 2 chữ số từ ký tự thứ 2 đến thứ 3 của mã sản phẩm của chi tiết yêu cầu sản xuất.
                *   Tham khảo phần ghi chú của sheet mã để biết cách ánh xạ từng mục.
            *   **Nếu loại bồn là '1' hoặc '2', kiểm tra các điều sau**:
                *   **Kiểm tra bắt buộc dung tích**.
                    Nếu "Dung tích" là 0 hoặc chưa cài đặt (''), hiển thị thông báo lỗi "Nhập dung tích là bắt buộc. Vui lòng nhập.".
                *   **Kiểm tra bắt buộc số lượng** (chỉ khi đăng ký yêu cầu sản xuất (nhấn nút đăng ký)).
                    Nếu "Số lượng" là 0 hoặc chưa cài đặt (''), hiển thị thông báo lỗi "Nhập số lượng là bắt buộc. Vui lòng nhập.".
2.  **Xác nhận tiếp tục xử lý**.
    Hiển thị thông báo xác nhận "Bạn có muốn đăng ký (cập nhật) không?" (OK・Hủy).
    Nếu **OK**, tiếp tục xử lý, nếu **Hủy**, dừng xử lý.
3.  **Xử lý đăng ký/cập nhật**.
    Thực hiện các xử lý sau cho mỗi chi tiết yêu cầu sản xuất.
    *   **Đăng ký nhật ký thay đổi**.
        So sánh danh sách thông tin thông số kỹ thuật sản xuất đã lấy ở mục "3. Hiển thị chi tiết yêu cầu sản xuất" trong hiển thị ban đầu với các giá trị nhập trên màn hình, nếu không khớp, đăng ký nhật ký.
        TBD.
    *   **Đăng ký/cập nhật thông số kỹ thuật sản xuất**.
        *   Kiểm tra xem chi tiết yêu cầu sản xuất trên màn hình có nằm trong danh sách thông tin thông số kỹ thuật sản xuất đã lấy ở mục "3. Hiển thị chi tiết yêu cầu sản xuất" trong hiển thị ban đầu hay không.
            *   Nếu không nằm trong danh sách thông tin thông số kỹ thuật sản xuất, thực hiện xử lý đăng ký (insert) sau:
                8. Sử dụng mutation `createManufactureSpecification` của API thông số kỹ thuật sản xuất số 8 để đăng ký thông tin thông số kỹ thuật sản xuất.
                *   `order_no`: Số đơn đặt hàng được kế thừa từ màn hình trước.
                *   `detail_no`: "No" của chi tiết yêu cầu sản xuất.
                *   `capacity`: "Dung tích" của chi tiết yêu cầu sản xuất đã nhập.
                *   Nếu độ dài của "Mã sản phẩm" nhỏ hơn hoặc bằng 2 hoặc ký tự đầu tiên của mã sản phẩm không phải '2':
                    *   `partition`: Chưa cài đặt ('').
                    *   `pit`: Chưa cài đặt ('').
                *   Nếu độ dài của "Mã sản phẩm" lớn hơn hoặc bằng 3 VÀ ký tự đầu tiên của mã sản phẩm là '2', thực hiện như sau:
                    *   Nếu loại bồn đã lấy ở mục "1. Kiểm tra sơ bộ" là '1':
                        *   `partition`: "Ngăn/Hố" của chi tiết yêu cầu sản xuất đã nhập.
                        *   `pit`: Chưa cài đặt ('').
                    *   Nếu loại bồn đã lấy ở mục "1. Kiểm tra sơ bộ" là '2':
                        *   `partition`: Chưa cài đặt ('').
                        *   `pit`: Đặt "Ngăn/Hố" của chi tiết yêu cầu sản xuất đã nhập.
                    *   Nếu loại bồn đã lấy ở mục "1. Kiểm tra sơ bộ" không phải '1' và không phải '2':
                        *   `partition`: Chưa cài đặt ('').
                        *   `pit`: Chưa cài đặt ('').
                *   `diameter`: "Đường kính" của chi tiết yêu cầu sản xuất đã nhập.
                *   `torso_length`: "Chiều dài thân" của chi tiết yêu cầu sản xuất đã nhập.
                *   `quantity`: "Số lượng" của chi tiết yêu cầu sản xuất đã nhập.
                *   `order_unit`: "Đơn giá đặt hàng" của chi tiết yêu cầu sản xuất đã nhập.
                *   `order_amount`: "Số tiền đặt hàng" của chi tiết yêu cầu sản xuất đã nhập.
                *   `manufacture_unit`: "Đơn giá yêu cầu sản xuất" của chi tiết yêu cầu sản xuất đã nhập.
                *   `manufacture_amount`: "Số tiền yêu cầu sản xuất" của chi tiết yêu cầu sản xuất đã nhập.
                *   `drawing_number`: Chưa cài đặt ('').
                *   `drawing_path`: Chưa cài đặt ('').
                *   `buried_drawing_number`: Chưa cài đặt ('').
                *   `buried_drawing_path`: Chưa cài đặt ('').
                *   `prior_work_inspection`: Chưa cài đặt ('').
                *   `inspection_schedule`: Chưa cài đặt ('').
                *   `factory_process_photos`: Chưa cài đặt ('').
                *   `dp_lubrication_port`: Chưa cài đặt ('').
                *   `dp_lubrication_port_no`: '0'.
                *   `dp_spare_mouth`: Chưa cài đặt ('').
                *   `dp_spare_mouth_no`: '0'.
                *   `si01_code`: Chưa cài đặt ('').
                *   `number_divisions`: '0'.
                *   `si03_code`: Chưa cài đặt ('').
                *   `si04_code`: Chưa cài đặt ('').
                *   `si02_code`: Chưa cài đặt ('').
                *   `banded`: '0'.
                *   `special_remark`: Chưa cài đặt ('').
                *   `delete_flag`: Chưa cài đặt ('').
                *   `plate_thickness`: '0'.
                *   `nozzle_height`: Chưa cài đặt ('').
                *   `frng_code`: Chưa cài đặt ('').
                *   `specialvehicle_app`: Chưa cài đặt ('').
                *   `specialvehicle_app_date`: Chưa cài đặt ('').
                *   `cost_estimation_number`: Chưa cài đặt ('').
                *   `water_level_marking`: Chưa cài đặt ('').
                *   `tnai_code`: Chưa cài đặt ('').
                *   `ptso_code`: Chưa cài đặt ('').
                *   `ptrt_code`: Chưa cài đặt ('').
                *   `protect_height`: '0'.
                *   `protect_height_decision`: Chưa cài đặt ('').
                *   `tank_drawing_decision_flag`: Chưa cài đặt ('').
                *   `protect_dimensional_details`: Chưa cài đặt ('').
                *   Tham khảo phần ghi chú của sheet thông số kỹ thuật sản xuất để biết cách ánh xạ từng mục.
            *   Nếu nằm trong danh sách thông tin thông số kỹ thuật sản xuất, thực hiện xử lý cập nhật (update) sau:
                8. Sử dụng mutation `updateManufactureSpecification` của API thông số kỹ thuật sản xuất số 8 để cập nhật thông tin thông số kỹ thuật sản xuất.
                *   `order_no`: Số đơn đặt hàng được kế thừa từ màn hình trước.
                *   `detail_no`: "No" của chi tiết yêu cầu sản xuất.
                *   `capacity`: "Dung tích" của chi tiết yêu cầu sản xuất đã nhập.
                *   Nếu độ dài của "Mã sản phẩm" nhỏ hơn hoặc bằng 2 hoặc ký tự đầu tiên của mã sản phẩm không phải '2':
                    *   `partition`: Chưa cài đặt ('').
                    *   `pit`: Chưa cài đặt ('').
                *   Nếu độ dài của "Mã sản phẩm" lớn hơn hoặc bằng 3 VÀ ký tự đầu tiên của mã sản phẩm là '2', thực hiện như sau:
                    *   Nếu loại bồn đã lấy ở mục "1. Kiểm tra sơ bộ" là '1':
                        *   `partition`: "Ngăn/Hố" của chi tiết yêu cầu sản xuất đã nhập.
                        *   `pit`: Chưa cài đặt ('').
                    *   Nếu loại bồn đã lấy ở mục "1. Kiểm tra sơ bộ" là '2':
                        *   `partition`: Chưa cài đặt ('').
                        *   `pit`: "Ngăn/Hố" của chi tiết yêu cầu sản xuất đã nhập.
                    *   Nếu loại bồn đã lấy ở mục "1. Kiểm tra sơ bộ" không phải '1' và không phải '2':
                        *   `partition`: Chưa cài đặt ('').
                        *   `pit`: Chưa cài đặt ('').
                *   `diameter`: "Đường kính" của chi tiết yêu cầu sản xuất đã nhập.
                *   `torso_length`: "Chiều dài thân" của chi tiết yêu cầu sản xuất đã nhập.
                *   `quantity`: "Số lượng" của chi tiết yêu cầu sản xuất đã nhập.
                *   `order_unit`: "Đơn giá đặt hàng" của chi tiết yêu cầu sản xuất đã nhập.
                *   `order_amount`: "Số tiền đặt hàng" của chi tiết yêu cầu sản xuất đã nhập.
                *   `manufacture_unit`: "Đơn giá yêu cầu sản xuất" của chi tiết yêu cầu sản xuất đã nhập.
                *   `manufacture_amount`: "Số tiền yêu cầu sản xuất" của chi tiết yêu cầu sản xuất đã nhập.
                *   `plate_thickness`: '0'.
                *   `nozzle_height`: Chưa cài đặt ('').
                *   `frng_code`: Chưa cài đặt ('').
                *   `cost_estimation_number`: Chưa cài đặt ('').
                *   Tham khảo phần ghi chú của sheet thông số kỹ thuật sản xuất để biết cách ánh xạ từng mục.
    *   **Đăng ký yêu cầu sản xuất**.
        Nếu `thông số kỹ thuật`.`số lượng` khác nhau giữa trước và sau khi đăng ký (cập nhật) VÀ loại bồn đã lấy ở mục "1. Kiểm tra sơ bộ" là từ '1' đến '4', thực hiện như sau:
        1.  Lấy số lượng bản ghi và giá trị lớn nhất của `yêu cầu sản xuất`.`số seri` từ bảng yêu cầu sản xuất dựa trên số đơn đặt hàng được kế thừa từ màn hình trước và "No" (số chi tiết) của chi tiết yêu cầu sản xuất.
        2.  Nếu "`thông số kỹ thuật`.`số lượng` (trước) < `thông số kỹ thuật`.`số lượng` (sau)", thêm thông tin yêu cầu sản xuất tăng thêm bằng cách sử dụng mutation `createManufactureRequest` của API yêu cầu sản xuất số 7.
            *   `order_no`: Số đơn đặt hàng được kế thừa từ màn hình trước.
            *   `detail_no`: "No" của chi tiết yêu cầu sản xuất.
            *   `serial_no`: ※(Tham khảo dưới đây).
            *   `seiz_code`: Chưa cài đặt ('').
            *   `fact_code`: Chưa cài đặt ('').
            *   `outsourcing_code`: Chưa cài đặt ('').
            *   `tank_no`: Chưa cài đặt ('').
               ※Nếu `thông số kỹ thuật`.`số lượng` (trước) là '0', `yêu cầu sản xuất`.`số seri` bắt đầu từ 1 và thêm số lượng bằng `thông số kỹ thuật`.`số lượng` (sau).
               Nếu `thông số kỹ thuật`.`số lượng` (trước) lớn hơn hoặc bằng '1', `yêu cầu sản xuất`.`số seri` bắt đầu từ giá trị lớn nhất của số seri đã lấy ở mục 1 trên + 1, và thêm số lượng bằng "`thông số kỹ thuật`.`số lượng` (sau) - `thông số kỹ thuật`.`số lượng` (trước)".
        3.  Nếu "`thông số kỹ thuật`.`số lượng` (trước) > `thông số kỹ thuật`.`số lượng` (sau)", xóa (delete) các yêu cầu sản xuất bị giảm đi.
            1.  7. Sử dụng truy vấn `getManufactureRequests` của API yêu cầu sản xuất số 7 để lấy thông tin yêu cầu sản xuất tương ứng.
                *   `order_no`: Số đơn đặt hàng được kế thừa từ màn hình trước.
                *   `detail_no`: "No" của chi tiết yêu cầu sản xuất.
                *   Tham khảo phần ghi chú của sheet yêu cầu sản xuất để biết cách ánh xạ từng mục.
            2.  10. Sử dụng truy vấn `getManufactureInstructions` của API chỉ dẫn sản xuất số 10 để lấy thông tin chỉ dẫn sản xuất có số chỉ dẫn sản xuất chưa cài đặt.
                *   `instruct_no`: Chưa cài đặt ('').
                *   `order_no`: Số đơn đặt hàng được kế thừa từ màn hình trước.
                *   `detail_no`: "No" của chi tiết yêu cầu sản xuất.
                *   Tham khảo phần ghi chú của sheet chỉ dẫn sản xuất để biết cách ánh xạ từng mục.
            3.  Kiểm tra các số seri có trong thông tin yêu cầu sản xuất đã lấy ở mục 1 trên theo thứ tự giảm dần giá trị, xem chúng có nằm trong thông tin chỉ dẫn sản xuất đã lấy ở mục 2 hay không. Nếu có, xóa thông tin yêu cầu sản xuất tương ứng.
               Tuy nhiên, việc xóa chỉ giới hạn đến số lượng "`thông số kỹ thuật`.`số lượng` (trước) - `thông số kỹ thuật`.`số lượng` (sau)". Ví dụ: Nếu `yêu cầu sản xuất`.`số seri` là 1～5, số seri có trong thông tin chỉ dẫn sản xuất là 1,3,5, và "`thông số kỹ thuật`.`số lượng` (trước) - `thông số kỹ thuật`.`số lượng` (sau)" là 2, thì các mục sẽ bị xóa là 3,5.
                *   7. Sử dụng mutation `deleteManufactureRequest` của API yêu cầu sản xuất số 7 để xóa thông tin yêu cầu sản xuất.


*   `order_no`: Yêu cầu sản xuất. Số đơn đặt hàng (`order_no`).
*   `detail_no`: Yêu cầu sản xuất. Số chi tiết (`detail_no`).
*   `serial_no`: Yêu cầu sản xuất. Số seri (`serial_no`).
*   Tham khảo phần ghi chú của sheet yêu cầu sản xuất để biết cách ánh xạ từng mục.
    4.  **Đăng ký (cập nhật) chi tiết đơn đặt hàng**.
        Cập nhật các mục sau trong thông tin chi tiết đơn đặt hàng bằng cách sử dụng mutation `updateOrderDetail` của API chi tiết đơn đặt hàng số 5, dựa trên chi tiết yêu cầu sản xuất.
        Các mục cập nhật: "Số lượng", "Đơn giá", "Số tiền", "Dự toán thực hiện", "Số tiền đã đặt hàng", "Số tiền đã thanh toán".
        *   `order_no`: Số đơn đặt hàng được kế thừa từ màn hình trước.
        *   `detail_no`: "No" của chi tiết yêu cầu sản xuất.
        *   `num`: "Số lượng" của chi tiết yêu cầu sản xuất.
        *   `unit`: "Đơn giá" của chi tiết yêu cầu sản xuất.
        *   `amount`: "Số tiền" của chi tiết yêu cầu sản xuất.
        *   `budget`: "Số tiền dự toán thực hiện" của chi tiết yêu cầu sản xuất.
        *   `ordered_amount`: Giá trị tổng hợp của "Tổng số tiền đặt hàng của công trình" và "Tổng số tiền yêu cầu sản xuất của thông số kỹ thuật sản xuất" đã tính dưới đây.
            *   Tính tổng số tiền đặt hàng của công trình:
                *   Tính tổng số tiền đặt hàng bằng cách sử dụng truy vấn `getConstructionOrders` của API đặt hàng công trình số 45, dựa trên số đơn đặt hàng và số chi tiết trên.
                    *   `contract_no`: Số đơn đặt hàng được kế thừa từ màn hình trước.
                    *   `detail_no`: "No" của chi tiết yêu cầu sản xuất.
            *   Tính tổng số tiền yêu cầu sản xuất của thông số kỹ thuật sản xuất:
                *   Tính tổng số tiền yêu cầu sản xuất bằng cách sử dụng truy vấn `getManufactureSpecifications` của API thông số kỹ thuật sản xuất số 8, dựa trên số đơn đặt hàng và số chi tiết trên.
                    *   `order_no`: Số đơn đặt hàng được kế thừa từ màn hình trước.
                    *   `detail_no`: "No" của chi tiết yêu cầu sản xuất.
        *   `paid_amount`: Giá trị tổng hợp của "Tổng số tiền nghiệm thu công trình" đã tính dưới đây và "Tổng số tiền yêu cầu sản xuất của thông số kỹ thuật sản xuất" đã tính trong phần tính tổng số tiền đặt hàng của công trình ở trên.
            *   Tính tổng số tiền nghiệm thu công trình:
                *   Tính tổng số tiền nghiệm thu bằng cách sử dụng truy vấn `getConstructionInspections` của API nghiệm thu công trình số 46 để lấy `công trình nghiệm thu`.`số tiền nghiệm thu`, dựa trên số đơn đặt hàng và số chi tiết trên.
                    *   `contract_no`: Số đơn đặt hàng được kế thừa từ màn hình trước.
                    *   `detail_no`: "No" của chi tiết yêu cầu sản xuất.
            *   Tham khảo phần ghi chú của sheet chi tiết đơn đặt hàng để biết cách ánh xạ từng mục.
4.  **Hiển thị thông báo hoàn tất**.
    *   Hiển thị thông báo "Đã hoàn tất lưu tạm thời".


#### 3. In tài liệu yêu cầu sản xuất
TBD.
#### 4. Lịch sử thay đổi
Chuyển đến màn hình tra cứu lịch sử thay đổi số 20.
※Kế thừa số đơn đặt hàng sang màn hình tra cứu lịch sử thay đổi.
#### 5. Đăng ký yêu cầu sản xuất
1.  **Xử lý đăng ký/cập nhật**.
    Thực hiện mục 2. Lưu tạm thời.
2.  **Xử lý phê duyệt**.
    TBD <!-- Đang xem xét triển khai xử lý phê duyệt bằng Teams -->.
3.  **Hiển thị thông báo hoàn tất**.
    *   Hiển thị thông báo "Đăng ký yêu cầu đã hoàn tất".
    *   Khi thông báo hoàn tất đóng lại, đóng màn hình yêu cầu sản xuất và quay lại màn hình trước.
       Khi đó, tải lại bằng các tham số tìm kiếm trước khi chuyển đến màn hình yêu cầu sản xuất.
#### 6. Đăng ký thông số kỹ thuật
##### Kiểm tra Bồn/Bể nước
*   Kiểm tra ký tự đầu tiên của mã sản phẩm liên quan đến nút đăng ký thông số kỹ thuật.
    *   Nếu ký tự đầu tiên là '2':
        *   38. Sử dụng truy vấn `getCodes` của API mã số 38 để lấy `kind1`.
            *   `kind_code`: TANK.
            *   `code`: 2 chữ số từ ký tự thứ 2 đến thứ 3 của mã sản phẩm.
            *   Tham khảo phần ghi chú của sheet mã để biết cách ánh xạ từng mục.
        *   Kiểm tra `kind1` đã lấy và hiển thị hộp thoại "Thông số kỹ thuật sản xuất (bồn)" hoặc "Thông số kỹ thuật sản xuất (bể nước)".
            *   Nếu là '1', hiển thị hộp thoại "Thông số kỹ thuật sản xuất (bồn)".
            *   Nếu là '2', hiển thị hộp thoại "Thông số kỹ thuật sản xuất (bể nước)".
            *   Trong các trường hợp khác, hiển thị thông báo lỗi "Không phải bồn, không thể đăng ký thông số kỹ thuật.".
    *   Nếu ký tự đầu tiên không phải '2':
        *   Hiển thị thông báo lỗi "Không phải bồn, không thể đăng ký thông số kỹ thuật.".


##### Hiển thị hộp thoại ban đầu (Bồn)
1.  **Tạo các menu thả xuống**.
    *   "Xác nhận bản vẽ", "Hoàn thiện bề mặt trong bồn", "Bộ bảo vệ (màu sơn)", "Bộ bảo vệ (lắp đặt)".
        10. Tham khảo menu thả xuống master code.
    *   "Có/Không kiểm tra trực tiếp giai đoạn trước", "Có/Không ảnh quy trình nhà máy", "Xe chuyên dụng (có/không đăng ký)", "Đường ống nhỏ giọt (vòi phun dầu)".
        9. Tham khảo menu thả xuống Có/Không.
2.  Đặt các mục sau dựa trên thông tin thông số kỹ thuật sản xuất đã lấy ở mục 3. Hiển thị chi tiết yêu cầu sản xuất trong hiển thị ban đầu.
    *   "Số bản vẽ bồn": Đặt thông số kỹ thuật sản xuất. Số bản vẽ (`drawing_number`).
    *   "Xác nhận bản vẽ": Chọn thông số kỹ thuật sản xuất. Cờ xác nhận bản vẽ bồn (`tank_drawing_decision_flag`).
    *   "Số bản vẽ chôn ngầm": Đặt thông số kỹ thuật sản xuất. Số bản vẽ chôn ngầm (`buried_drawing_number`).
    *   "Số KHK": Tham khảo dưới đây.
        *   1. Lấy loại đơn đặt hàng từ dữ liệu mục tiêu của thông tin đơn đặt hàng đã lấy trong hiển thị accordion.
            *   Nếu loại đơn đặt hàng là "31" (SF: SF Tank), thực hiện như sau:
                *   Lấy thông tin bồn.
                    23. Sử dụng truy vấn `getTankByNo` của API bồn số 23 để lấy thông tin bồn.
                    *   `figure_no`: Thông số kỹ thuật sản xuất. Số bản vẽ (`drawing_number`). ※Nếu số bản vẽ chứa '△', thì số bản vẽ là các ký tự từ đầu số bản vẽ cho đến trước '△'.
                    *   Tham khảo phần ghi chú của sheet bồn để biết cách ánh xạ từng mục.
                    *   Nếu thông tin bồn được lấy, thực hiện như sau:
                        *   Kiểm tra thông tin bồn.
                            *   Sử dụng truy vấn `getCodes` của API mã số 38 để lấy `kind1` tương ứng dựa trên bồn. Loại hình thức (`zu24_code`) và bồn. Loại vật liệu (`zu23_code`).
                                *   `kind_code`: ZU24, ZU23.
                                *   `code`: Bồn. Loại hình thức (`zu24_code`), Bồn. Loại vật liệu (`zu23_code`).
                            *   Nếu `code`.`kind1` (loại hình thức) = "00001" hoặc (`code`.`kind1` (loại hình thức) <> "00001" VÀ `code`.`kind1` (loại vật liệu) = "00001"), thực hiện như sau:
                                *   9. Sử dụng truy vấn `getkhkManagements` của API quản lý KHK số 9 để lấy thông tin KHK.
                                    *   `capacity`: Bồn. Dung tích (`capacity`).
                                    *   `bore`: Bồn. Đường kính trong (`bore`).
                                    *   `torso_length`: Bồn. Chiều dài thân (`torso_length`).
                                    *   `adhesion_width`: Bồn. Chiều rộng tiếp xúc (`adhesion_width`).
                                    *   Tham khảo phần ghi chú của sheet quản lý KHK để biết cách ánh xạ từng mục.
                                *   Nếu thông tin KHK được lấy, lọc theo `KHK管理`.`削除フラグ` (`delete_flag`) <> '1' và thực hiện như sau:
                                    *   **Chỉnh sửa tên nhà máy**.
                                        Với mỗi thông tin KHK đã lấy, nối các ký tự đầu tiên của tên nhà máy lấy từ API mã theo thứ tự, theo định dạng "ký tự đầu tiên của tên nhà máy + ' ' + KHK information.KHK number", phân cách bằng dấu phẩy (',').
                                        Ví dụ: "関 TMD3-SF-01-009".
                                        Tuy nhiên, nếu ký tự lấy được là "旧" (cũ), thì giá trị đã nối cho đến trước "旧" sẽ bị hủy, và bắt đầu nối lại từ "旧". Ngoài ra, định dạng chỉ bao gồm "ký tự đầu tiên của tên nhà máy" mà không bao gồm số KHK.
                                        *   38. Sử dụng truy vấn `getCodes` của API mã số 38 để lấy ký tự 1.
                                            *   `kind_code`: KHKK.
                                            *   `code`: KHK management.Factory (`khkk_code`).
                                            *   Tham khảo phần ghi chú của sheet mã để biết cách ánh xạ từng mục.
                                        *   Nếu kết quả chỉnh sửa tên nhà máy cho thông tin KHK đã hoàn thành có thông tin, đặt kết quả chỉnh sửa.
                                        *   Nếu kết quả chỉnh sửa tên nhà máy không có thông tin (''), thêm khóa và lấy lại thông tin KHK.
                                            *   9. Sử dụng truy vấn `getkhkManagements` của API quản lý KHK số 9 để lấy lại thông tin KHK.
                                                *   `capacity`: Bồn. Dung tích (`capacity`).
                                                *   `bore`: Bồn. Đường kính trong (`bore`).
                                                *   `torso_length`: Bồn. Chiều dài thân (`torso_length`).
                                                *   `adhesion_width`: '0'.
                                                *   Tham khảo phần ghi chú của sheet quản lý KHK để biết cách ánh xạ từng mục.
                                        *   Nếu thông tin KHK được lấy lại, lọc theo `KHK管理`.`削除フラグ` (`delete_flag`) <> '1' và thực hiện như sau:
                                            *   Chỉnh sửa tên nhà máy.
                                                Tham khảo phần chỉnh sửa tên nhà máy ở trên.
                                            *   Nếu kết quả chỉnh sửa tên nhà máy cho thông tin KHK đã lấy lại có thông tin, đặt kết quả chỉnh sửa.
                                            *   Nếu kết quả chỉnh sửa tên nhà máy trong lần lấy lại không có thông tin (''), đặt '無' (không có).
    *   "Có/Không kiểm tra trực tiếp giai đoạn trước": Chọn thông số kỹ thuật sản xuất. Kiểm tra trực tiếp công việc trước (`prior_work_inspection`).
    *   "Ngày dự kiến kiểm tra": Đặt thông số kỹ thuật sản xuất. Ngày dự kiến kiểm tra tại chỗ (`inspection_schedule`) theo định dạng YYYY/MM/DD.
    *   "Có/Không ảnh quy trình nhà máy": Chọn thông số kỹ thuật sản xuất. Ảnh quy trình nhà máy (`factory_process_photos`).
    *   "Xe chuyên dụng (có/không đăng ký)": Chọn thông số kỹ thuật sản xuất. Đăng ký xe chuyên dụng (`specialvehicle_app`).
    *   "Xe chuyên dụng (ngày đăng ký)": Đặt thông số kỹ thuật sản xuất. Ngày đăng ký xe chuyên dụng (`specialvehicle_app_date`) theo định dạng YYYY/MM/DD.
    *   "Hoàn thiện bề mặt trong bồn": Chọn thông số kỹ thuật sản xuất. Hoàn thiện bề mặt trong bồn (`tnai_code`).
    *   "Bộ bảo vệ (màu sơn)": Chọn thông số kỹ thuật sản xuất. Màu sơn bộ bảo vệ (`ptso_code`).
    *   "Bộ bảo vệ (lắp đặt)": Chọn thông số kỹ thuật sản xuất. Lắp đặt bộ bảo vệ (`ptrt_code`).
    *   "Bộ bảo vệ (chi tiết kích thước)": Đặt thông số kỹ thuật sản xuất. Chi tiết kích thước bộ bảo vệ (`protect_dimensional_details`).
    *   "Đường ống nhỏ giọt (vòi phun dầu)": Chọn thông số kỹ thuật sản xuất. Đường ống nhỏ giọt vòi phun dầu (`dp_lubrication_port`).
    *   "Ghi chú đặc biệt": Đặt thông số kỹ thuật sản xuất. Ghi chú đặc biệt (`special_remark`).
    *   Nếu "Có/Không kiểm tra trực tiếp giai đoạn trước" khác Có ('1'), đặt "Ngày dự kiến kiểm tra" trống ('') và chỉ đọc.
    *   Nếu "Xe chuyên dụng (có/không đăng ký)" khác Có ('1'), đặt "Xe chuyên dụng (ngày đăng ký)" trống ('') và chỉ đọc.


##### Hiển thị hộp thoại ban đầu (Bể nước)
1.  **Tạo các menu thả xuống**.
    *   "Xác nhận bản vẽ", "Loại số lượng phân chia", "Phương pháp chôn ngầm", "Vị trí lắp đặt", "Đánh dấu mực nước", "Tải trọng xe ô tô".
        10. Tham khảo menu thả xuống master code.
    *   "Có/Không kiểm tra trực tiếp giai đoạn trước", "Có/Không ảnh quy trình nhà máy", "Xe chuyên dụng (có/không đăng ký)".
        9. Tham khảo menu thả xuống Có/Không.
2.  Đặt các mục sau dựa trên thông tin thông số kỹ thuật sản xuất đã lấy ở mục 2. Hiển thị chi tiết yêu cầu sản xuất trong hiển thị ban đầu.
    *   "Số bản vẽ bồn": Đặt thông số kỹ thuật sản xuất. Số bản vẽ (`drawing_number`).
    *   "Xác nhận bản vẽ": Chọn thông số kỹ thuật sản xuất. Cờ xác nhận bản vẽ bồn (`tank_drawing_decision_flag`).
    *   "Số bản vẽ chôn ngầm": Đặt thông số kỹ thuật sản xuất. Số bản vẽ chôn ngầm (`buried_drawing_number`).
    *   "Có/Không kiểm tra trực tiếp giai đoạn trước": Chọn thông số kỹ thuật sản xuất. Kiểm tra trực tiếp công việc trước (`prior_work_inspection`).
    *   "Ngày dự kiến kiểm tra": Đặt thông số kỹ thuật sản xuất. Ngày dự kiến kiểm tra tại chỗ (`inspection_schedule`) theo định dạng YYYY/MM/DD.
    *   "Có/Không ảnh quy trình nhà máy": Chọn thông số kỹ thuật sản xuất. Ảnh quy trình nhà máy (`factory_process_photos`).
    *   "Xe chuyên dụng (có/không đăng ký)": Chọn thông số kỹ thuật sản xuất. Đăng ký xe chuyên dụng (`specialvehicle_app`).
    *   "Xe chuyên dụng (ngày đăng ký)": Đặt thông số kỹ thuật sản xuất. Ngày đăng ký xe chuyên dụng (`specialvehicle_app_date`) theo định dạng YYYY/MM/DD.
    *   "Loại số lượng phân chia": Chọn thông số kỹ thuật sản xuất. Loại số lượng phân chia (`si01_code`).
    *   "Số lượng phân chia": Đặt thông số kỹ thuật sản xuất. Số lượng phân chia (`number_divisions`).
    *   "Phương pháp chôn ngầm": Chọn thông số kỹ thuật sản xuất. Phương pháp chôn ngầm (`si02_code`).
    *   "Vị trí lắp đặt": Chọn thông số kỹ thuật sản xuất. Vị trí lắp đặt (`si03_code`).
    *   "Đánh dấu mực nước": Chọn thông số kỹ thuật sản xuất. Đánh dấu mực nước (`water_level_marking`).
    *   "Dải băng": Đặt thông số kỹ thuật sản xuất. Dải băng (`banded`).
    *   "Tải trọng xe ô tô": Chọn thông số kỹ thuật sản xuất. Tải trọng xe ô tô (`si04_code`).
    *   "Ghi chú đặc biệt": Đặt thông số kỹ thuật sản xuất. Ghi chú đặc biệt (`special_remark`).
    *   Nếu "Có/Không kiểm tra trực tiếp giai đoạn trước" khác Có ('1'), đặt "Ngày dự kiến kiểm tra" trống ('') và chỉ đọc.
    *   Nếu "Xe chuyên dụng (có/không đăng ký)" khác Có ('1'), đặt "Xe chuyên dụng (ngày đăng ký)" trống ('') và chỉ đọc.
    *   Nếu "Loại số lượng phân chia" khác Phân chia ('2'), đặt "Số lượng phân chia" trống ('') và chỉ đọc.


#### 7. Tự động hoàn thành số bản vẽ
1.  **Bồn**. Tạo menu thả xuống bằng cách sử dụng truy vấn `getTanks` của API bồn số 23 với số bản vẽ đã nhập.
    *   `figure_no`: Số bản vẽ đã nhập.
    *   `zu22_code`: '00001' (Bồn).
    *   Tham khảo phần ghi chú của sheet bồn để biết cách ánh xạ từng mục.
    *   Nếu số bản vẽ chưa cài đặt (''), API bồn sẽ không được gọi. Nếu kết quả tìm kiếm lớn hơn hoặc bằng 501 mục, hiển thị thông báo lỗi "Quá nhiều kết quả.".
2.  **Bể nước**. Tạo menu thả xuống bằng cách sử dụng truy vấn `getWaterTanks` của API bể nước số 57 với số bản vẽ đã nhập.
    *   `figure_no`: Số bản vẽ đã nhập.
    *   `zu03_code`: '00001' (Bản vẽ lắp ráp thân).
    *   Tham khảo phần ghi chú của sheet bể nước để biết cách ánh xạ từng mục.
    *   Nếu số bản vẽ chưa cài đặt (''), API bể nước sẽ không được gọi. Nếu kết quả tìm kiếm lớn hơn hoặc bằng 501 mục, hiển thị thông báo lỗi "Quá nhiều kết quả.".
#### 8. Tự động hoàn thành số bản vẽ chôn ngầm
1.  **Bồn**. Tạo menu thả xuống bằng cách sử dụng truy vấn `getTanks` của API bồn số 23 với số bản vẽ chôn ngầm đã nhập.
    *   `figure_no`: Số bản vẽ chôn ngầm đã nhập.
    *   `zu22_code`: '00002' (Bản vẽ chôn ngầm).
    *   Tham khảo phần ghi chú của sheet bồn để biết cách ánh xạ từng mục.
    *   Nếu số bản vẽ chôn ngầm chưa cài đặt (''), API bồn sẽ không được gọi. Nếu kết quả tìm kiếm lớn hơn hoặc bằng 501 mục, hiển thị thông báo lỗi "Quá nhiều kết quả.".
2.  **Bể nước**. Tạo menu thả xuống bằng cách sử dụng truy vấn `getWaterTanks` của API bể nước số 57 với số bản vẽ chôn ngầm đã nhập.
    *   `figure_no`: Số bản vẽ chôn ngầm đã nhập.
    *   `zu03_code`: '00002' (Bản vẽ chôn ngầm).
    *   Tham khảo phần ghi chú của sheet bể nước để biết cách ánh xạ từng mục.
    *   Nếu số bản vẽ chôn ngầm chưa cài đặt (''), API bể nước sẽ không được gọi. Nếu kết quả tìm kiếm lớn hơn hoặc bằng 501 mục, hiển thị thông báo lỗi "Quá nhiều kết quả.".


#### 9. Menu thả xuống Có/Không
Tạo menu thả xuống với các giá trị sau:
*   Giá trị: Văn bản.
    *   0: Không.
    *   1: Có.
    *   2: "".
#### 10. Menu thả xuống Master Code
Tham khảo thông số kỹ thuật này.
*   Ghi chú bổ sung:
    *   Mặt bích: FRNG.
    *   Xác nhận bản vẽ: TKAK.
    *   Hoàn thiện bề mặt trong bồn: TNAI.
    *   Màu sơn bộ bảo vệ: PTSO.
    *   Lắp đặt bộ bảo vệ: PTRT.
    *   Loại số lượng phân chia: SI01.
    *   Phương pháp chôn ngầm: SI02.
    *   Vị trí lắp đặt: SI03.
    *   Đánh dấu mực nước: KSYA.
    *   Tải trọng xe ô tô: SI04.
#### 11. Đăng ký thông số kỹ thuật bồn
1.  **Kiểm tra sơ bộ**.
    Thực hiện các kiểm tra sau và nếu có nhiều lỗi, hiển thị thông báo lỗi tổng hợp trong một modal.
    *   Nếu độ dài ký tự của "Chi tiết kích thước" vượt quá 600 byte, hiển thị thông báo lỗi "Chi tiết kích thước quá dài. Yêu cầu = 600: Thực tế = nnnn".
        ※nnnn là số byte thực tế của chi tiết kích thước.
    *   Nếu độ dài ký tự của "Ghi chú đặc biệt" vượt quá 1200 byte, hiển thị thông báo lỗi "Ghi chú đặc biệt quá dài. Yêu cầu = 1200: Thực tế = nnnn".
        ※nnnn là số byte thực tế của ghi chú đặc biệt.
    *   Nếu độ dài ký tự của "Số bản vẽ" vượt quá 10 byte, hiển thị thông báo lỗi "Số bản vẽ quá dài. Yêu cầu = 10: Thực tế = nnnn".
        ※nnnn là số byte thực tế của số bản vẽ.
    *   Nếu độ dài ký tự của "Số bản vẽ chôn ngầm" vượt quá 20 byte, hiển thị thông báo lỗi "Số bản vẽ chôn ngầm quá dài. Yêu cầu = 20: Thực tế = nnnn".
        ※nnnn là số byte thực tế của số bản vẽ chôn ngầm.
2.  **Xác nhận tiếp tục xử lý**.
    Hiển thị thông báo xác nhận "Bạn có muốn cập nhật không?" (OK・Hủy).
    Nếu **OK**, tiếp tục xử lý, nếu **Hủy**, dừng xử lý.
3.  **Xử lý cập nhật thông số kỹ thuật sản xuất**.
    *   Cập nhật thông số kỹ thuật sản xuất liên quan đến đăng ký thông số kỹ thuật (bồn) dựa trên số đơn đặt hàng được kế thừa từ màn hình trước và "No" (số chi tiết) của chi tiết yêu cầu sản xuất. 8. Sử dụng mutation `updateManufactureSpecification` của API thông số kỹ thuật sản xuất số 8 để cập nhật.
        *   `order_no`: Số đơn đặt hàng được kế thừa từ màn hình trước.
        *   `detail_no`: "No" của chi tiết yêu cầu sản xuất.
        *   `drawing_number`: "Số bản vẽ" đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bồn).
        *   `buried_drawing_number`: "Số bản vẽ chôn ngầm" đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bồn).
        *   `prior_work_inspection`: ID của "Có/Không kiểm tra trực tiếp công việc trước (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bồn).
        *   `inspection_schedule`: "Ngày dự kiến kiểm tra" đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bồn) (định dạng YYYYMMDD).
        *   `factory_process_photos`: ID của "Có/Không ảnh quy trình nhà máy (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bồn).
        *   `dp_lubrication_port`: ID của "Vòi phun dầu (hộp chọn)" trong "Đường ống nhỏ giọt" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bồn).
        *   `specialvehicle_app`: ID của "Có/Không đăng ký xe chuyên dụng (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bồn).
        *   `specialvehicle_app_date`: "Ngày đăng ký xe chuyên dụng" đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bồn) (định dạng YYYYMMDD).
        *   `tnai_code`: ID của "Hoàn thiện bề mặt trong bồn (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bồn).
        *   `ptso_code`: ID của "Màu sơn bộ bảo vệ (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bồn).
        *   `ptrt_code`: ID của "Lắp đặt bộ bảo vệ (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bồn).
        *   `tank_drawing_decision_flag`: ID của "Xác nhận" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bồn).
        *   `protect_dimensional_details`: "Chi tiết kích thước" đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bồn).
        *   `special_remark`: "Ghi chú đặc biệt" đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bồn).
        *   Tham khảo phần ghi chú của sheet thông số kỹ thuật sản xuất để biết cách ánh xạ từng mục.
4.  **Hiển thị thông báo hoàn tất**.
    *   Hiển thị thông báo "Đăng ký thông số kỹ thuật bồn đã hoàn tất".
    *   Khi thông báo hoàn tất đóng lại, đóng hộp thoại đăng ký thông số kỹ thuật bồn và quay lại màn hình yêu cầu sản xuất.
5.  **Tự động nhập chi tiết yêu cầu sản xuất** (dung tích, đường kính, chiều dài thân).
    Lấy "dung tích", "đường kính (đường kính trong)", "chiều dài thân" từ bảng bồn dựa trên số bản vẽ đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bồn) và đặt vào chi tiết yêu cầu sản xuất trên màn hình yêu cầu sản xuất.
    *   23. Sử dụng truy vấn `getTankByNo` của API bồn số 23 để lấy thông tin bồn (dung tích, đường kính (đường kính trong), chiều dài thân).
        *   `figure_no`: Số bản vẽ đã nhập. ※Nếu số bản vẽ chứa '△', thì số bản vẽ là các ký tự từ đầu số bản vẽ cho đến trước '△'.
        *   Tham khảo phần ghi chú của sheet bồn để biết cách ánh xạ từng mục.
    *   Đặt thông tin bồn đã lấy (dung tích, đường kính (đường kính trong), chiều dài thân) vào các mục "Dung tích", "Đường kính", "Chiều dài thân" của chi tiết yêu cầu sản xuất trên màn hình yêu cầu sản xuất.


#### 12. Hủy đăng ký thông số kỹ thuật bồn
Hiển thị thông báo xác nhận "Bạn có muốn hủy đăng ký thông số kỹ thuật bồn không? Bạn có chắc chắn không?" (OK・Hủy).
Nếu **OK**, đóng hộp thoại và quay lại màn hình yêu cầu sản xuất, nếu **Hủy**, quay lại hộp thoại.
#### 13. Đăng ký thông số kỹ thuật bể nước
1.  **Kiểm tra sơ bộ**.
    Thực hiện các kiểm tra sau và nếu có nhiều lỗi, hiển thị thông báo lỗi tổng hợp trong một modal.
    *   Nếu "Có/Không kiểm tra trực tiếp công việc trước" là Có ('1') VÀ "Ngày dự kiến kiểm tra" chưa cài đặt (''), hiển thị thông báo lỗi "Vui lòng nhập ngày dự kiến kiểm tra.".
    *   Nếu "Loại số lượng phân chia" là Phân chia ('2') VÀ "Số lượng phân chia" chưa cài đặt ('') hoặc là '0', hiển thị thông báo lỗi "Vui lòng nhập số lượng phân chia.".
    *   Nếu độ dài ký tự của "Ghi chú đặc biệt" vượt quá 1200 byte, hiển thị thông báo lỗi "Ghi chú đặc biệt quá dài. Yêu cầu = 1200: Thực tế = nnnn".
        ※nnnn là số byte thực tế của ghi chú đặc biệt.
    *   Nếu độ dài ký tự của "Số bản vẽ" vượt quá 10 byte, hiển thị thông báo lỗi "Số bản vẽ quá dài. Yêu cầu = 10: Thực tế = nnnn".
        ※nnnn là số byte thực tế của số bản vẽ.
    *   Nếu độ dài ký tự của "Số bản vẽ chôn ngầm" vượt quá 20 byte, hiển thị thông báo lỗi "Số bản vẽ chôn ngầm quá dài. Yêu cầu = 20: Thực tế = nnnn".
        ※nnnn là số byte thực tế của số bản vẽ chôn ngầm.
2.  **Xác nhận tiếp tục xử lý**.
    Hiển thị thông báo xác nhận "Bạn có muốn cập nhật không?" (OK・Hủy).
    Nếu **OK**, tiếp tục xử lý, nếu **Hủy**, dừng xử lý.
3.  **Xử lý cập nhật**.
    *   Cập nhật thông số kỹ thuật sản xuất liên quan đến đăng ký thông số kỹ thuật (bể nước) dựa trên số đơn đặt hàng được kế thừa từ màn hình trước và "No" (số chi tiết) của chi tiết yêu cầu sản xuất. 8. Sử dụng mutation `updateManufactureSpecification` của API thông số kỹ thuật sản xuất số 8 để cập nhật.
        *   `order_no`: Số đơn đặt hàng được kế thừa từ màn hình trước.
        *   `detail_no`: "No" của chi tiết yêu cầu sản xuất.
        *   `drawing_number`: "Số bản vẽ" đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   `buried_drawing_number`: "Số bản vẽ chôn ngầm" đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   `prior_work_inspection`: ID của "Có/Không kiểm tra trực tiếp công việc trước (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   `inspection_schedule`: "Ngày dự kiến kiểm tra" đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bể nước) (định dạng YYYYMMDD).
        *   `factory_process_photos`: ID của "Có/Không ảnh quy trình nhà máy (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   `si01_code`: ID của "Loại số lượng phân chia (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   `number_divisions`: "Số lượng phân chia" đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   `si03_code`: ID của "Vị trí lắp đặt (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   `si04_code`: ID của "Tải trọng xe ô tô (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   `si02_code`: ID của "Phương pháp chôn ngầm (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   `banded`: "Dải băng" đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   `specialvehicle_app`: ID của "Có/Không đăng ký xe chuyên dụng (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   `specialvehicle_app_date`: "Ngày đăng ký xe chuyên dụng" đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bể nước) (định dạng YYYYMMDD).
        *   `water_level_marking`: ID của "Hoàn thiện bề mặt trong bồn (hộp chọn)" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   `tank_drawing_decision_flag`: ID của "Xác nhận" đã chọn trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   `special_remark`: "Ghi chú đặc biệt" đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bể nước).
        *   Tham khảo phần ghi chú của sheet thông số kỹ thuật sản xuất để biết cách ánh xạ từng mục.
4.  **Hiển thị thông báo hoàn tất**.
    *   Hiển thị thông báo "Đăng ký thông số kỹ thuật bể nước đã hoàn tất".
    *   Khi thông báo hoàn tất đóng lại, đóng hộp thoại đăng ký thông số kỹ thuật bể nước và quay lại màn hình yêu cầu sản xuất.
5.  **Tự động nhập chi tiết yêu cầu sản xuất** (dung tích, đường kính). Lấy "dung tích", "đường kính (đường kính trong)" từ bảng bể nước dựa trên số bản vẽ đã nhập trong hộp thoại thông số kỹ thuật sản xuất (bể nước) và đặt vào chi tiết yêu cầu sản xuất trên màn hình yêu cầu sản xuất.
    *   57. Sử dụng truy vấn `getWaterTankByNo` của API bể nước số 57 để lấy thông tin bể nước (dung tích, đường kính (đường kính trong)).
        *   `figure_no`: Số bản vẽ đã nhập. ※Nếu số bản vẽ chứa '△', thì số bản vẽ là các ký tự từ đầu số bản vẽ cho đến trước '△'.
        *   Tham khảo phần ghi chú của sheet bể nước để biết cách ánh xạ từng mục.
    *   Đặt thông tin bể nước đã lấy (dung tích, đường kính (đường kính trong)) vào các mục "Dung tích", "Đường kính" của chi tiết yêu cầu sản xuất trên màn hình yêu cầu sản xuất.


#### 14. Hủy đăng ký thông số kỹ thuật bể nước
Hiển thị thông báo xác nhận "Bạn có muốn hủy đăng ký thông số kỹ thuật bể nước không? Bạn có chắc chắn không?" (OK・Hủy).
Nếu **OK**, đóng hộp thoại và quay lại màn hình yêu cầu sản xuất, nếu **Hủy**, quay lại hộp thoại.
}
