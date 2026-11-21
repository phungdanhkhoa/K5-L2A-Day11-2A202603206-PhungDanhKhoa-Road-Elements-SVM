# Báo cáo cá nhân — Ngày 11
Họ tên: Phùng Danh Khoa               Mã học viên: 2A202603206  
Phiên bản CVAT (Help → About): v2.74.1  
Guideline: Road v1.0    Lần nộp (`revision` trong manifest): 2  
Thời điểm export (ngày giờ trên CVAT): 2026-11-21 03:30 UTC    Số lần nộp lại sau Rework: 1  

## 1. Kiểm project
| Mục | Đúng như guideline Road v1.0? | Ghi chú |
| --- | --- | --- |
| 6 label, đúng kiểu | Có | Đủ `lane` (polyline), `road_curb` (polyline), `crosswalk` (polygon), `drivable` (polygon), `traffic_light` (rectangle), `traffic_sign` (rectangle) |
| Thuộc tính và giá trị | Có | Khớp 100% với `taxonomy.py` và `configs/road_v1_labels.json` |
| 4 tag ảnh | Có | Đủ `camera`, `timeofday`, `weather`, `source`, mỗi tag có thuộc tính select `value` |
Cách kết thúc polyline trên bản CVAT này: Nhấn phím `N` hoặc click đúp điểm cuối.  
Cách đặt tag ảnh trên bản CVAT này: Chọn công cụ Tag ở thanh bên trái $\rightarrow$ chọn label tag tương ứng $\rightarrow$ vào bảng Objects $\rightarrow$ Details để chọn giá trị `value`.  

## 2. Số lượng đã gán
| Task | Ảnh | lane | road_curb | crosswalk | drivable | traffic_light | traffic_sign |
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
| front | 12 | 26 | 4 | 6 | 23 | 28 | 19 |
| front-extra (đã làm 4 / 4) | 4 | 10 | 1 | 2 | 8 | 7 | 11 |
| fisheye | 8 | 6 | 9 | 9 | 6 | 3 | 5 |
| fisheye-extra (đã làm 4 / 4) | 4 | 0 | 3 | 4 | 3 | 0 | 0 |

## 3. Bảng ca khó
| Ảnh | Nhãn | Phân vân giữa | Đã chọn | Lý do |
| --- | --- | --- | --- | --- |
| A05 | `lane` | Có tự vẽ L1/R1 giả định hay không | Không vẽ vạch | Đường khu dân cư không có vạch sơn phân làn trên mặt đường; theo Checkpoints và Guideline §6 được miễn QC1-02, chỉ vẽ `road_curb` và `drivable direct`. |
| A06 | `lane` | Có gán lane dưới gầm cầu qua giao lộ không | Không gán lane | Mặt đường trong giao lộ có vạch qua đường lớn `crosswalk`, không có vạch kẻ làn sơn trắng riêng biệt. |
| A07 | `lane` | Gán L1 ở mép dải phân cách bê tông hay không | Gán `road_curb` bên trái, `R1`/`R2` bên phải | Mép trái là gờ bê tông (jersey barrier) không có vạch sơn kẻ mép nên gán `road_curb`, không gán lane L1. |
| A08 | `traffic_sign` | Biển phụ "PASSENGER CARS ONLY" gộp chung hay tách | Tách riêng bounding box | Biển phụ có chức năng bổ trợ, quy chuẩn Guideline §3 yêu cầu gắn bounding box riêng biệt với `group = supplementary`. |
| B06 | `lane` | Gán `position = L1` hay `other` trên camera trái | `position = other` | Guideline §4 quy định nghiêm ngặt: camera hông (`fisheye_left`, `fisheye_right`) bắt buộc gán `position = other` (tránh lỗi nặng QC1-03). |
| B11 | `lane` | Có gán vạch ô đỗ xe và ô người khuyết tật thành `lane` không | Không gán `lane` (0 lane) | Guideline §4 xác định vạch ô đỗ xe không phải vạch làn xe chạy; gán nhãn lane ở đây là vi phạm bẫy QC1-03. |

## 4. Phiếu QC 1 nhận được
Bạn kiểm lô của tôi: Nguyễn Công Quý (2A202603149)    Quyết định nhận được: Rework (5 lỗi chính / 83 mục kiểm = 6,02% > ngưỡng 5%)  
| Mã lỗi | Số lỗi | Đồng ý? | Đã sửa? |
| --- | ---: | --- | --- |
| QC1-01 (tag camera) | 0 | Đồng ý | Đã rà soát chuẩn 100% tag camera (đặc biệt B11 là fisheye_rear) |
| QC1-02 (đủ L1/R1) | 0 | Đồng ý | 8 cảnh báo lint ở A05, A06, A07, A09, A10, A14 đã xác nhận miễn trừ (đường không vạch, qua giao lộ, hoặc mép là dải phân cách bê tông/lề đường không có vạch sơn riêng) |
| QC1-03 (position) | 0 | Đồng ý | Đã gán position = other cho toàn bộ polyline trên camera hông B06, B07, B08 |
| QC1-04 (chiều, tâm vạch) | 1 | Đồng ý | **Đã sửa A09**: Rút điểm cuối của vạch đôi L2 về đoạn sơn còn nhìn thấy rõ tại `(459.5, 458.1)`, loại bỏ 2 điểm vươn vào vùng bóng/thân xe trắng |
| QC1-05 (vạch đứt) | 0 | Đồng ý | Nối liền các nét đứt thành 1 polyline duy nhất từ gần ra xa |
| QC1-07 (vùng lái, crosswalk) | 3 | Đồng ý | **Đã sửa 3 lỗi:**<br>1. **A08**: Nắn lại cạnh trái polygon `drivable direct` bám sát vạch L1 từ `(485.7, 281.5)` qua `(296.6, 314.0)` đến `(42.5, 357.6)` và mép ảnh trước khi nối xuống vạch qua đường, không cắt chéo lòng đường.<br>2. **A10**: Mở rộng polygon `drivable direct` xuyên qua xe phía trước theo quy tắc "đi được nếu đường trống", kéo dài hành lang làn bám vạch L1 lên tận $y=380$.<br>3. **B05**: Thu gọn cạnh xa của polygon `crosswalk` bám sát mép dải sơn ngựa vằn (chỉnh $y$ từ ~55–60 xuống ~70–75), không trèo lên vỉa hè/gờ đá. |
| QC1-08 (đèn, màu) | 0 | Đồng ý | Đầy đủ thuộc tính color, type, facing |
| QC1-09 (biển, facing, biển phụ) | 0 | Đồng ý | Tách riêng bounding box cho biển phụ, gán group = supplementary |
| QC1-10 (hình in, đủ biển) | 0 | Đồng ý | Bounding box ôm sát mép biển |
| QC1-11 (fisheye arc & edge_zone) | 1 | Đồng ý | **Đã sửa B03**: Bổ sung đoạn vạch trắng đứt gần camera từ `(958, 632)` qua `(915, 580)`, `(845, 460)` nối vào đoạn xa `(783, 357)` thành 1 polyline uốn cong (7 điểm, vẽ từ gần ra xa) |
| QC1-12 (ZIP & XML) | 0 | Đồng ý | Cấu trúc file đạt chuẩn, lint pass 100% |

## 5. Kiểm file export
| File | Mở lại được? | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera có trong XML? |
| --- | --- | --- | --- | --- |
| `phungdanhkhoa-front.zip` | Có (`road_check lint` đọc tốt) | 8 (A01: 4 lane, 3 drivable, 1 sign) | 7 (A02: 3 lane, 3 drivable, 1 curb) | Có (`camera = front`) |
| `phungdanhkhoa-fisheye.zip` | Có (`road_check lint` đọc tốt) | 4 (B01: 1 crosswalk, 1 drivable, 2 curb) | 7 (B02: 2 crosswalk, 1 drivable, 2 curb, 2 light) | Có (`camera = fisheye_front`) |

## 6. Ảnh chụp màn hình
1. **Tab Labels của project**: Project `d11-road-phungdanhkhoa` đủ 6 label vẽ và 4 tag.
2. **Một ảnh camera trước đã gán đủ**: Ảnh A01 (đủ 4 vạch làn gần ra xa, 3 polygon drivable, biển chỉ dẫn).
3. **Một ảnh fisheye đã gán**: Ảnh B02 (polygon crosswalk, drivable, polyline curb uốn cong nhiều điểm, 2 đèn viền bật `edge_zone = true`).
4. **Một đoạn XML có polyline lane**:
```xml
<polyline label="lane" occluded="0" points="771.46,561.35;694.63,274.50;696.30,268.05">
  <attribute name="position">R1</attribute>
  <attribute name="style">solid</attribute>
  <attribute name="color">white</attribute>
  <attribute name="kind">single</attribute>
</polyline>
```

## 7. Tự đánh giá
- **Lỗi bạn mắc nhiều nhất hôm nay (theo mã QC1-xx), và cách tránh ở Ngày 20**:
  Mắc lỗi QC1-07 (vùng lái dừng cụt ở cản sau xe thay vì xuyên qua theo quy tắc "đi được nếu đường trống", cạnh trái drivable cắt chéo qua vạch L1, và polygon crosswalk tràn lên vỉa hè) cùng với QC1-04 (vẽ vạch kéo quá dài vào vùng xe che) và QC1-11 (thiếu đoạn nét đứt gần camera trong fisheye).
  Cách tránh:
  - Khi gán vùng lái drivable: Luôn tuân thủ tuyệt đối quy tắc "đi được nếu đường trống", mở rộng hành lang làn xe chạy thẳng qua các phương tiện phía trước thay vì dừng cụt ở cản xe; bám sát vạch L1/R1 đã vẽ.
  - Khi gán crosswalk: Phóng to tối đa để bắt mép sơn ngựa vằn, không để polygon tràn lên vỉa hè.
  - Khi gán lane: Dừng ở điểm xa nhất còn thấy tâm vạch rõ ràng, không cố kéo dài vào thân/bóng xe; với vạch đứt trên fisheye, rà soát từ góc đáy ảnh lên để không sót các đoạn đứt ở cự ly gần.
- **Nhãn của bạn đi vào model ở đâu? Chọn một lỗi đã sửa và nói nó làm model học sai thế nào**:
  Nhãn polyline `lane` đi vào model giữ làn (Lane Keeping Assist - LKA) / phát hiện làn đường (ví dụ mô hình row-anchor như Ultra-Fast-Lane-Detection). Nếu vạch sơn bị gán sai `position` (ví dụ gán L1 lên camera hông hoặc gán nhầm vạch tim đường thành vạch mép), model điều khiển sẽ nhầm lẫn tâm làn xe đang chạy, dẫn đến việc tính sai góc đánh lái và xe có thể chệch làn hoặc lao sang làn đối diện.

## 8. (Tùy chọn, buffer) Từ polyline đến đích row-anchor
| y | 720 | 680 | 640 | 600 | 560 |
| --- | ---: | ---: | ---: | ---: | ---: |
| x của chúng tôi | 215 | 320 | 415 | 500 | 575 |
| x của bạn kia | 218 | 322 | 416 | 498 | 572 |

Trả lời 3 câu:
1. **Nếu bạn dừng polyline sớm 40 px, hàng nào đổi thành "không có làn"?**  
   Hàng $y = 560$ (hàng ở xa nhất tính từ đáy ảnh 720 lên) sẽ bị mất điểm neo và chuyển trạng thái thành "không có làn" (no lane), khiến model dự đoán làn bị đứt đoạn sớm.
2. **So với bạn được kiểm (cùng ảnh): ở hàng 720 và hàng 560, hai bạn lệch nhau bao nhiêu pixel? Lệch ở hàng nào ảnh hưởng LKA nhiều hơn (slide 25)?**  
   Ở hàng 720 lệch 3 px, ở hàng 560 lệch 3 px. Lệch ở hàng 720 (ngay sát đầu xe) ảnh hưởng nghiêm trọng hơn đến LKA vì hệ thống lái dựa trên điểm gần nhất để căn chỉnh tim đường tức thời, sai số gần xe sẽ khuếch đại góc lái khiến xe bị giật lái.
3. **Nếu polyline vẽ từ xa về gần (ngược quy tắc 2), cách tính trên có đổi không? Script nào sẽ gặp khó?**  
   Cách nội suy tọa độ $x$ theo các hàng $y$ cố định vẫn có thể tính được về mặt hình học thuần túy, nhưng thứ tự lưu điểm trong file XML bị đảo ngược khiến các thuật toán kiểm tra hướng vector (như `road_check.py` kiểm tra `points[0][1] < points[-1][1]`) sẽ báo lỗi nghiêm trọng QC1-04 (lỗi vẽ ngược), đồng thời các pipeline huấn luyện định hướng dòng xe (từ dưới lên trên) sẽ bị đảo lộn chiều gradient.
