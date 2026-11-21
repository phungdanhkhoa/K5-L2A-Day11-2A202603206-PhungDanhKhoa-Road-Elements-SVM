# Báo cáo cá nhân — Ngày 11
Họ tên: Phùng Danh Khoa               Mã học viên: 2A202603206  
Phiên bản CVAT (Help → About): v2.74.1  
Guideline: Road v1.0    Lần nộp (`revision` trong manifest): 1  
Thời điểm export (ngày giờ trên CVAT): 2026-11-20 15:14 UTC    Số lần nộp lại sau Rework: 0  

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
| --- | ---: | ---: | ---: | ---: | ---: | ---: | ---: |
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
Bạn kiểm lô của tôi: qc-verifier  Quyết định nhận được: Release  
| Mã lỗi | Số lỗi | Đồng ý? | Đã sửa? |
| --- | ---: | --- | --- |
| QC1-01 (tag camera) | 0 | Đồng ý | Đã rà soát chuẩn 100% tag camera (đặc biệt B11 là fisheye_rear) |
| QC1-02 (đủ L1/R1) | 0 | Đồng ý | Các cảnh báo lint ở A05, A06, A07, A09, A10, A14 đều là đoạn không có vạch sơn thực tế, đã đối chiếu và xác nhận miễn trừ |
| QC1-03 (position) | 0 | Đồng ý | Đã chỉnh toàn bộ polyline trên camera hông sang `position = other` |
| QC1-04 (chiều, tâm vạch) | 0 | Đồng ý | 100% vẽ từ gần ra xa, tâm vạch lệch $\le 5\text{ px}$ |
| QC1-05 (vạch đứt) | 0 | Đồng ý | Nối liền thành 1 polyline duy nhất, không tách rời |
| QC1-11 (fisheye arc & edge_zone) | 0 | Đồng ý | Polyline cong $\ge 4$ điểm, các box ngoài 80% bán kính đều bật `edge_zone = true` |

## 5. Kiểm file export
| File | Mở lại được? | Số shape ảnh 1 (job / file) | Số shape ảnh 2 (job / file) | Tag camera có trong XML? |
| --- | --- | --- | --- | --- |
| `khoaphung-front.zip` | Có (`road_check lint` đọc tốt) | 8 (A01: 4 lane, 3 drivable, 1 sign) | 7 (A02: 3 lane, 3 drivable, 1 curb) | Có (`camera = front`) |
| `khoaphung-fisheye.zip` | Có (`road_check lint` đọc tốt) | 4 (B01: 1 crosswalk, 1 drivable, 2 curb) | 7 (B02: 2 crosswalk, 1 drivable, 2 curb, 2 light) | Có (`camera = fisheye_front`) |

## 6. Ảnh chụp màn hình
1. **Tab Labels của project**: Project `d11-road-khoaphung` đủ 6 label vẽ và 4 tag.
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
  Lỗi hay gặp nhất là để thuộc tính mặc định của CVAT (ví dụ polyline mới vẽ tự nhận `L1 / solid / white / single` và tag camera tự nhận `front` cho cả ảnh fisheye). Cách tránh: Luôn mở bảng Details ngay sau khi vẽ/đặt tag để đối chiếu và sửa thuộc tính, đặc biệt là kiểm tra quy tắc `position = other` cho camera hai bên.
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
