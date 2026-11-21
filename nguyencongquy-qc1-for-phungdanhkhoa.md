# Phiếu QC 1 — Road v1.0

**Lô được kiểm:** Phùng Danh Khoa (2A202603206)

**Người kiểm:** Nguyễn Công Quý (2A202603149)

**Ngày:** 21/11/2026

**File được kiểm:** `phungdanhkhoa-front.zip` và `phungdanhkhoa-fisheye.zip`, revision 1. SHA-256 của cả hai ZIP khớp manifest tại [commit nguồn của lô nhãn](https://github.com/phungdanhkhoa/K5-L23-Day11-PhungDanhKhoa-2A202603206-Road-Elements-SVM/tree/2bb5feef12cef6102612015312bb8cd85c0f4381).

## Mẫu kiểm

Đối chiếu trực quan 6 ảnh bắt buộc với ảnh gốc. Mẫu gồm các ca vạch chưa rõ, giao lộ và hai hướng camera fisheye. Thứ tự shape trong XML dùng chỉ số từ 0, trong từng ảnh. Chạy lint riêng trên toàn bộ 28 ảnh.

| Ảnh | Shape | Tag | Tổng |
| --- | ---: | ---: | ---: |
| A06 | 12 | 4 | 16 |
| A08 | 21 | 4 | 25 |
| A09 | 7 | 4 | 11 |
| A10 | 8 | 4 | 12 |
| B03 | 8 | 4 | 12 |
| B05 | 3 | 4 | 7 |
| **Tổng** | **59** | **24** | **83** |

## Tổng hợp theo mã

| Mã | Mục kiểm | Số mục kiểm | Lỗi xác nhận | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 | Camera và tag ảnh | 24 tag | 0 | — |
| QC1-02 | Hai phía làn ở 4 ảnh front và B03 rear | 10 phía | 0 | 4 cảnh báo mẫu chờ xác nhận |
| QC1-03 | Vị trí vạch | 9 lane | 0 | — |
| QC1-04 | Tâm vạch, chiều và điểm dừng ở front | 8 lane | 1 | Chính |
| QC1-05 | Vạch đứt và đoạn bị che | 9 lane | 0 | — |
| QC1-06 | Style, color và kind | 9 lane | 0 | — |
| QC1-07 | Vùng lái, vùng qua đường và mép lề front nếu có | 14 mục | 3 | Chính |
| QC1-08 | Box đèn và màu | 20 box đèn | 0 | — |
| QC1-09 | Facing, type, group và biển phụ | 33 box | 0 | — |
| QC1-10 | Biển thật, biển có thể thấy và độ sát box | 13 box và 6 ảnh rà bỏ sót | 0 | — |
| QC1-11 | Cung, mép lề và vùng rìa fisheye | 8 shape | 1 | Chính |
| QC1-12 | Tệp ZIP và cấu trúc XML | 2 ZIP | 0 | — |

## Danh sách lỗi

| # | Mã | Ảnh | Nhãn | Mức | Quan sát và đề nghị sửa |
| ---: | --- | --- | --- | --- | --- |
| 1 | QC1-07 | A08 | `drivable direct`, shape 3 | Chính | Cạnh trái polygon cắt chéo qua mặt đường, tách khỏi vạch L1 đang thấy trước vạch qua đường. Rà lại ranh trái của làn trực tiếp và chỉnh polygon bám ranh đã xác định. |
| 2 | QC1-04 | A09 | `lane L2`, shape 5 | Chính | Đầu xa kéo sang phải tới gần xe trắng, quá đoạn còn xác định được tâm vạch đôi. Rút điểm cuối về đoạn sơn nhìn thấy. |
| 3 | QC1-07 | A10 | `drivable direct`, shape 5 | Chính | Polygon dừng ngay ở cản sau xe phía trước. Xác định tiếp ranh làn theo phần mặt đường có thể đi nếu đường trống, theo quy tắc vùng lái. |
| 4 | QC1-11 | B03 | `lane R1`, shape 5 | Chính | Polyline chỉ bắt đầu ở đoạn vạch xa quanh tọa độ (783,357). Đoạn trắng gần camera khoảng (890,550) đến (958,632) cùng hướng với vạch đó chưa được bao. Rà và nối theo cung trong một polyline. |
| 5 | QC1-07 | B05 | `crosswalk`, shape 0 | Chính | Cạnh xa của polygon tràn lên vỉa hè phía sau các vạch sơn. Chỉnh cạnh xa bám dải sơn qua đường. |

## Ca cần xác nhận

Lint toàn lô báo 8 cảnh báo `QC1-02`. Các cảnh báo dưới đây chưa được tính là lỗi xác nhận.

| Phạm vi | Ảnh và phía bị báo | Câu hỏi cần xác nhận |
| --- | --- | --- |
| Trong mẫu | A06 L1 và R1 | Nét vàng ngắn sau vạch qua đường và nét trắng sát lề có phải hai ranh của làn xe chụp qua nút giao không? |
| Trong mẫu | A09 R1 | Làn `direct` trong ZIP nằm bên phải vạch đứt. Xác nhận ranh phải là lề hay có vạch sơn riêng. |
| Trong mẫu | A10 R1 | Bên phải xe chụp có xe và phản chiếu che phần mặt đường. Xác nhận đoạn sơn nào còn đủ rõ để gán R1. |
| Ngoài mẫu | A05 L1 và R1, A07 L1, A14 R1 | Cảnh báo từ lint. Cần rà ảnh gốc trước khi kết luận bỏ sót. |

## Quyết định

**Rework.** Mẫu có **0 lỗi nặng đã xác nhận, 5 lỗi chính trên 83 nhãn (6,02%) và 0 lỗi nhẹ đã xác nhận**. Tỷ lệ lỗi chính vượt ngưỡng 5%. Sửa các lỗi trong bảng và đối chiếu lại 8 cảnh báo thiếu vạch trước khi chốt lô. [Thang mức lỗi QC 1](docs/qc_severity_road_v1.0.md)
