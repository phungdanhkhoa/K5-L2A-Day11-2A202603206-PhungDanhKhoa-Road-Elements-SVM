# Phiếu QC 1 — Road v1.0

**Lô được kiểm** Nguyễn Công Quý (2A202603149)  
**Người nộp phiếu** Phùng Danh Khoa (2A202603206)  
**Ngày rà soát** 21/11/2026  
**Nguồn** Hai ZIP ở [commit 6573ff4 của Nguyễn Công Quý](https://github.com/nguyencongquy/K5-L23-Day11-2A202603149-NguyenCongQuy-Road-Elements-SVM/tree/6573ff4bc51c3d6369202f0de36059c963cd6991), revision 1. Đã so SHA-256 và số byte với manifest của commit.  
**Cách kiểm** Codex hỗ trợ đối chiếu ảnh gốc với lớp phủ XML và chạy lint. Mẫu gồm 6 ảnh bắt buộc. Lint đọc cả 28 ảnh. Các lỗi gán nhãn dưới đây dùng mẫu trực quan, chưa đối chiếu GT của coach.

| Ảnh trong mẫu | Shape | Tag | Tổng |
| --- | ---: | ---: | ---: |
| A06 | 13 | 4 | 17 |
| A07 | 10 | 4 | 14 |
| A10 | 10 | 4 | 14 |
| B01 | 6 | 4 | 10 |
| B03 | 10 | 4 | 14 |
| B05 | 4 | 4 | 8 |
| **Tổng** | **53** | **24** | **77** |

## Tổng hợp theo mã

| Mã | Hạng mục | Số mục kiểm | Lỗi xác nhận | Mức |
| --- | --- | --- | ---: | --- |
| QC1-01 | Tag và camera | 28 tag camera bằng lint, 24 tag trong mẫu | 0 | — |
| QC1-02 | Đủ L1/R1 | 6 phía trên 3 ảnh front trong mẫu, cộng rà cảnh báo lint toàn lô | 0 | 5 cảnh báo cần coach rà |
| QC1-03 | Position | 9 polyline lane | 0 | — |
| QC1-04 | Tâm vạch, chiều, điểm cuối | 9 polyline lane | 1 | Chính |
| QC1-05 | Vạch đứt và đoạn bị che | 9 polyline lane | 0 | — |
| QC1-06 | Style, color và kind | 9 polyline lane | 1 | Chính |
| QC1-07 | Drivable và crosswalk | 7 drivable, 2 crosswalk, rà bỏ sót trên 6 ảnh | 4 | Chính |
| QC1-08 | Box đèn và màu | 9 box đèn và rà bỏ sót trên 6 ảnh | 1 | Chính |
| QC1-09 | Facing, type, group và biển phụ | 25 box | 0 | — |
| QC1-10 | Hình in, đủ biển và độ sát hộp | 16 box biển và rà bỏ sót trên 6 ảnh | 2 | Chính |
| QC1-11 | Fisheye, cung, mép và edge zone | 20 shape trên 3 ảnh fisheye | 0 | — |
| QC1-12 | File | 2 ZIP, 1 manifest | 0 | — |

## Danh sách lỗi

Chỉ số shape tính từ 0 trong từng ảnh, bỏ qua tag. Tọa độ tính theo ảnh gốc.

| # | Mã | Ảnh | Nhãn, vị trí | Mức | Quan sát |
| ---: | --- | --- | --- | --- | --- |
| 1 | QC1-04 | A06 | lane #10 | Chính | Từ mép vạch qua đường xuống điểm đầu khoảng (180,632), polyline đi trên một đoạn dài mặt đường không thấy vệt sơn. Giữ đoạn vạch dọc nhìn thấy sau vạch qua đường. |
| 2 | QC1-06 | A06 | lane #10 | Chính | Hai nét sơn dọc cạnh dòng xe bên trái có màu vàng và đi thành cặp. XML ghi `white/single`. |
| 3 | QC1-07 | A06 | crosswalk, thiếu | Chính | Vạch qua đường chạy ngang giao lộ trước và giữa các cột. XML của ảnh chưa có crosswalk. |
| 4 | QC1-08 | A06 | traffic_light #2 | Chính | Box ở khoảng (934,288) dừng trên mắt xanh đang sáng. Box cần bao trọn vỏ cụm đèn trước khi xét màu và hướng. |
| 5 | QC1-07 | A07 | drivable #3 | Chính | Biên xa dừng gần cản sau xe bạc mặc dù vẫn nhận ra hành lang giữa dải chắn và vạch trắng bên phải. |
| 6 | QC1-10 | A07 | traffic_sign #1 | Chính | Box bao bảng quảng cáo cửa hàng “SAM JIN ROOFING SUPPLY”, được ghi `mandatory`. |
| 7 | QC1-10 | A07 | traffic_sign #4 | Chính | Box ở khoảng (1164,424) nằm trên vạch sơn sát bánh xe tải, không có tấm biển ở vị trí đó. |
| 8 | QC1-07 | A10 | drivable #5 | Chính | Biên xa cắt ngang đáy hàng xe đang dừng. Hai rìa quan sát được cho phép kéo tiếp hành lang qua vùng bị che ngắn. |
| 9 | QC1-07 | B05 | crosswalk #1 | Chính | Biên xa đi qua gờ và phần lát sát chân người, trong khi nét sơn xa nhất bắt đầu thấp hơn trên mặt đường. |

## Ca mơ hồ

| Ảnh | Quan sát | Trạng thái |
| --- | --- | --- |
| A05 L1/R1 | Hai cảnh báo lint. Người làm ghi đường khu dân cư có xe đỗ và chưa thấy vạch dọc. Ảnh ngoài mẫu trực quan phiếu này. | Chưa có xác nhận coach |
| A06 R1 | Vạch trắng phía phải nằm phía bên kia hàng cột sát lề, chưa rõ có phải ranh làn xe chụp. | Chờ coach xác định |
| A07 L1 | Ranh dễ thấy là dải chắn bê tông. Chưa thấy vạch sơn riêng dọc chân dải chắn. | Chờ coach xác nhận |
| A09 position | `drivable direct` ở bên phải vạch đứt nhưng vạch này có `R1`. Vị trí làn xe chụp cần được xác định trước khi chốt mã nặng. | Chờ coach xác định |
| A12 R1 | Một cảnh báo lint. Người làm ghi có lề và xe đỗ phía phải sau vạch qua đường. Ảnh ngoài mẫu trực quan phiếu này. | Chưa có xác nhận coach |

## Quyết định

**Rework.** Có **9 lỗi chính / 77 nhãn kiểm = 11,69%**, vượt ngưỡng 5%. Có 0 lỗi nặng đã xác nhận và 0 lỗi nhẹ đã xác nhận. Năm cảnh báo QC1-02 và ca position A09 cần xác nhận riêng. B01 là một ảnh trong mẫu chưa thấy lỗi cần sửa qua lượt đối chiếu này.

## Ghi chú bàn giao lô của Phùng Danh Khoa

Sáu cảnh báo QC1-02 của lô Phùng Danh Khoa và lý do để trống từng phía được ghi ở [mục 4 của báo cáo cá nhân](phungdanhkhoa-report.md#4-phiếu-qc1-nhận-được-và-lượt-sửa).
