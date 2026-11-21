# Phiếu QC 1 — Road v1.0

Lô được kiểm (học viên): nguyenvanan (Lô đối tác)  
Người kiểm (học viên): Phùng Danh Khoa (phungdanhkhoa)  
Ngày: 2026-11-20  
Phiên bản guideline: Road v1.0  
File được kiểm (tên + `revision` trong manifest): `nguyenvanan-front.zip`, `nguyenvanan-fisheye.zip` (`revision 1`)  
Số ảnh kiểm: 20 ảnh bắt buộc (12 front A01–A12, 8 fisheye B01–B08) (+ 8 ảnh thêm: A13–A16, B09–B12, kiểm riêng)  
Số nhãn đã kiểm (shape + tag): 198 nhãn  

## Tổng hợp theo mã
| Mã | Loại lỗi | Số mục kiểm | Số lỗi | Mức |
| --- | --- | ---: | ---: | --- |
| QC1-01 tag camera | sai thuộc tính / bỏ sót | 28 | 0 | nặng |
| QC1-02 đủ L1, R1 | bỏ sót | 20 | 0 | nặng |
| QC1-03 position | sai thuộc tính / thừa | 42 | 0 | nặng |
| QC1-04 tâm vạch, chiều, điểm dừng | sai phạm vi / bỏ sót | 42 | 2 | chính |
| QC1-05 vạch đứt, che | sai phạm vi / sai thuộc tính | 42 | 1 | chính |
| QC1-06 style, color, kind | sai thuộc tính | 42 | 0 | chính |
| QC1-07 vùng lái | sai phạm vi / bỏ sót / thừa | 30 | 1 | chính |
| QC1-08 đèn, màu | bỏ sót / sai phạm vi / sai thuộc tính | 20 | 0 | nặng / chính |
| QC1-09 facing, type, group, biển phụ | sai thuộc tính | 35 | 1 | chính |
| QC1-10 hình in, đủ biển | bỏ sót / sai phạm vi | 15 | 0 | chính |
| QC1-11 fisheye | sai phạm vi / sai thuộc tính | 45 | 1 | chính |
| QC1-12 file | lỗi file | 198 | 0 | nhẹ / nặng |

## Danh sách lỗi (mỗi lỗi một dòng)
| # | Mã | Ảnh | Nhãn (label, id) | Mức | Mô tả ngắn |
| --- | --- | --- | --- | --- | --- |
| 1 | QC1-04 | A01 | lane (id: 12) | chính | Điểm dừng polyline kéo quá xa vào đường chân trời (>10px ngoài vệt sơn thấy rõ) |
| 2 | QC1-04 | A03 | lane (id: 18) | chính | Tâm polyline bị lệch khỏi tâm vệt sơn ~7px ở đoạn giữa |
| 3 | QC1-05 | A02 | lane (id: 15) | chính | Vạch đứt L1 bị ngắt thành 2 polyline thay vì nối liền 1 polyline |
| 4 | QC1-07 | A04 | drivable direct (id: 25) | chính | Vùng lái direct tràn lấn sang dải đỗ xe bên phải ~15px |
| 5 | QC1-09 | A08 | traffic_sign (id: 34) | chính | Biển phụ "PASSENGER CARS ONLY" chưa tách hộp riêng, đang gộp chung biển gantry |
| 6 | QC1-11 | B03 | lane (id: 42) | chính | Vạch lane trên fisheye chỉ đặt 2 điểm thẳng, chưa uốn theo cung cong mép thấu kính |

## Ca mơ hồ
| Ảnh | Câu hỏi cho lead | Lời đáp (nếu có) |
| --- | --- | --- |
| A05 | Đường ngõ không có vạch sơn kẻ đường, bạn không vẽ L1/R1 có hợp lệ không? | **Hợp lệ.** Được miễn QC1-02 theo CHECKPOINTS.md và Guideline §6. Đã kiểm tra thực tế không có vạch kẻ. |
| B11 | Bãi đỗ xe không gán nhãn lane cho các ô kẻ đỗ xe có đúng không? | **Đúng.** Vạch ô đỗ xe không gán `lane` theo Guideline §4. |
| A07 | Mép trái có dải phân cách bê tông không vẽ L1 có đúng không? | **Đúng.** Mép bê tông không có vạch sơn được gán `road_curb`, làn xe chạy sát dải phân cách không có vạch L1 nên được miễn. |

## Quyết định
Lỗi nặng: 0   Lỗi chính: 6 / 198 nhãn = 3.03%   Lỗi nhẹ: 0   Ca chờ lead: 0  
Quyết định (Release / Hold / Rework): **Release**  
Lý do (theo luật của phiếu): Không còn lỗi nặng nào (các cảnh báo thiếu L1/R1 đều thuộc trường hợp miễn trừ hợp lệ đã xác nhận); tỉ lệ lỗi chính là 3.03% (nằm dưới ngưỡng tối đa cho phép 5.0%); các ca mơ hồ đều có quy định đối chiếu rõ ràng.
