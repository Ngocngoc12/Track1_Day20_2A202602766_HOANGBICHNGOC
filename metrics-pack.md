# Metrics Pack — AI tạo slide bài giảng

## 00 — Phạm vi

| Thành phần | Nội dung |
|---|---|
| **Dự án** | Công cụ AI hỗ trợ giáo viên tạo, chỉnh sửa và xuất slide bài giảng đẹp, đúng chủ đề và phù hợp với học sinh. |
| **Persona** | Giáo viên phổ thông tự chuẩn bị slide cho các tiết dạy của mình. |
| **Core job** | "Tôi muốn biến nội dung và mục tiêu của một bài học thành bộ slide dễ dạy, dễ hiểu trong thời gian ngắn mà vẫn bảo đảm đúng kiến thức và phù hợp với học sinh." |
| **Use case** | Tạo slide bài giảng đẹp bằng công cụ AI. |

**Phạm vi đo:** Chỉ đo hành vi giáo viên tạo và duyệt slide cho một bài học. Không coi đăng nhập, mở app, nhập prompt hoặc AI sinh bản nháp là value cuối cùng.

---

## 01 — Core Action Card

### 1. Phân biệt bốn khái niệm

| Khái niệm | Câu trả lời |
|---|---|
| **Core job** | Biến nội dung và mục tiêu của một bài học thành bộ slide dễ dạy, dễ hiểu trong thời gian ngắn. |
| **Core action** | Giáo viên xem, chỉnh sửa cần thiết và **duyệt một bộ slide cho một bài học** để có thể dùng khi dạy. |
| **Core value** | Có một bộ slide dùng được cho tiết học mà không phải tự thiết kế từ đầu. |
| **Core value event** | `lesson_deck_approved` — bộ slide chuyển sang trạng thái được giáo viên duyệt cho một bài học. |

### 2. Core Action Card

| Thành phần | Câu trả lời |
|---|---|
| **Target user** | Giáo viên phổ thông đang chuẩn bị một tiết/bài học. |
| **Core job** | Chuẩn bị slide bài giảng rõ ràng, đẹp và đúng nội dung trong thời gian ngắn. |
| **Core action** | Hoàn tất xem, chỉnh sửa và duyệt một bộ slide cho một bài học cụ thể. |
| **Object** | Bộ slide/draft deck gắn với một bài học. |
| **Preconditions** | Giáo viên đã nhập chủ đề hoặc tài liệu bài học; hệ thống đã tạo bản nháp; bản nháp có mã bài học duy nhất. |
| **Completion rule** | Chỉ tính hoàn tất khi giáo viên lưu trạng thái `approved` cho bộ slide và bộ slide có tối thiểu: tiêu đề bài, mục tiêu học tập, phần nội dung chính và ít nhất một yếu tố trực quan. Việc tải lại trang hoặc tự động lưu không tạo thêm lần hoàn tất. |
| **Core value** | Giáo viên có bộ slide sẵn sàng để dùng hoặc xuất sang định dạng trình chiếu. |
| **Evidence of value** | `lesson_deck_approved`, sau đó có thể là `lesson_deck_exported`; giáo viên không cần bắt đầu lại từ trang trống cho bài học đó. |
| **Candidate event** | `lesson_deck_approved`. |

### 3. Tự kiểm 5 tiêu chí

| Tiêu chí | Kết quả | Giải thích |
|---|---|---|
| **1. Gần core value** | Đạt | Giáo viên đã có bộ slide được duyệt để sử dụng, gần hơn rõ rệt với việc dạy bài học. |
| **2. Có thể lặp lại** | Đạt | Mỗi bài học mới có thể tạo ra một lần duyệt slide mới. |
| **3. Có thể quan sát** | Đạt | Có trạng thái chuyển từ `draft`/`in_review` sang `approved`, gắn với `teacher_id` và `lesson_id`. |
| **4. Có ý nghĩa** | Đạt | Nhiều bộ slide được duyệt cho các bài học khác nhau phản ánh hệ thống đang giúp giáo viên hoàn thành công việc. Không chỉ phản ánh việc mở app. |
| **5. Có thể tác động** | Đạt | Team có thể cải thiện chất lượng dàn ý, mẫu thiết kế, gợi ý hình ảnh và quy trình chỉnh sửa để tăng tỷ lệ duyệt. |

**Kết luận Gate 1:** Đạt 5/5. Core action có actor là giáo viên, object là bộ slide của một bài học và completion rule quan sát được. Đây không phải "mở app", "hỏi AI" hay output tự động của hệ thống, vì phải có quyết định duyệt của giáo viên.

---

## 02 — Action Nature Card + cadence

| Thành phần | Câu trả lời |
|---|---|
| **Actor** | Giáo viên/account giáo viên thực hiện việc chuẩn bị slide. |
| **Intent** | Chuẩn bị bài dạy tiếp theo nhanh hơn nhưng vẫn có slide phù hợp với mục tiêu học tập. |
| **Trigger** | Chủ yếu do giáo viên chủ động khi có bài học hoặc tiết dạy sắp tới; lịch dạy là tác nhân bên ngoài hỗ trợ kích hoạt. |
| **Effort** | Cần nhập chủ đề/tài liệu, xem bản nháp, kiểm tra độ đúng, chỉnh sửa bố cục/nội dung và duyệt; thường mất vài phút đến vài chục phút tùy bài. |
| **Value timing** | Value xuất hiện ngay sau khi giáo viên duyệt được bộ slide; một phần giá trị được xác nhận khi slide được xuất hoặc sử dụng khi dạy. |
| **State** | Lưu lại bài học, bản nháp, phiên bản đã duyệt, mẫu thiết kế, nội dung chỉnh sửa và trạng thái xuất bản. |
| **Dependency** | Phụ thuộc vào nội dung/tài liệu bài học, chất lượng AI, lịch dạy và đôi khi là hình ảnh/tài nguyên bên ngoài. |
| **Repeat condition** | Có bài học/tiết dạy mới hoặc cần cập nhật bộ slide cho một bài học khác. Giáo viên có lý do quay lại vì công việc chuẩn bị bài tiếp theo. |

### Kết luận cadence

> Đối với **giáo viên chuẩn bị slide cho các bài học**, core action **duyệt một bộ slide dùng được cho một bài học** thường xuất hiện **theo dự án/bài học, thường 1–3 lần mỗi tuần**, vì **mỗi bài học hoặc tiết dạy là một nhiệm vụ chuẩn bị tương đối độc lập và phát sinh theo lịch dạy thực tế**. Do đó, nhịp đo phù hợp là **theo chu kỳ bài học và tổng hợp theo tuần**, ở cấp **teacher account**.

**Dạng hành vi:** Theo dự án, có lặp lại theo lịch dạy; không phải thói quen daily cố định.

**Kết luận Gate 2:** Đạt. Cadence được suy ra từ repeat condition và lịch chuẩn bị bài, không chọn chỉ vì dashboard có daily/weekly.

---

## 03 — Metric System

### 1. Activation metric

| Thành phần | Định nghĩa |
|---|---|
| **Start event** | `first_lesson_request_submitted`: giáo viên gửi yêu cầu tạo bộ slide đầu tiên sau khi tạo tài khoản. |
| **Activation event** | `lesson_deck_approved`: giáo viên duyệt bộ slide đầu tiên đạt completion rule. |
| **Time window** | Trong vòng 24 giờ kể từ `first_lesson_request_submitted`. |

**Activation rate:** số giáo viên có `lesson_deck_approved` đầu tiên trong 24 giờ / số giáo viên có `first_lesson_request_submitted`.

### 2. Engagement metric

| Góc đo | Metric | Lý do |
|---|---|---|
| **Frequency** | Số `lesson_deck_approved` trung bình trên mỗi giáo viên đã activated trong một tuần. | Phản ánh giáo viên có tiếp tục hoàn tất các bài học mới theo cadence tự nhiên hay không. |
| **Depth/quality** | Tỷ lệ bộ slide được duyệt mà không cần chỉnh sửa lớn sau bản nháp đầu tiên. | Cho biết mỗi lần dùng tạo ra value với ít công sức sửa lại hơn; không khuyến khích dùng lâu hoặc click nhiều. |

### 3. North Star Metric

> **NSM = Số giáo viên có ít nhất 2 bộ slide cho các bài học khác nhau được duyệt đạt completion rule trong mỗi 14 ngày.**

- **Unit of value:** giáo viên có nhu cầu chuẩn bị bài.
- **Quality threshold:** bộ slide đạt completion rule và được giáo viên duyệt.
- **Frequency:** ít nhất 2 bài học khác nhau trong 14 ngày.

NSM này đo việc giáo viên nhận được giá trị lặp lại, không chỉ đo số lượt hỏi AI hay số lượng bản nháp được sinh.

### 4. Leading indicators

1. **Tỷ lệ yêu cầu có dàn ý được giáo viên chấp nhận trong 10 phút đầu.** Tin rằng chỉ số này dự báo core action vì dàn ý phù hợp giúp giảm công sức chỉnh sửa trước khi duyệt.
2. **Tỷ lệ bản nháp được mở để xem toàn bộ nội dung và chuyển sang trạng thái chỉnh sửa.** Tin rằng giáo viên đã tiếp tục xử lý bản nháp thay vì bỏ qua, nên có khả năng tiến tới duyệt.
3. **Thời gian trung vị từ `lesson_request_submitted` đến `lesson_deck_approved`.** Nếu thời gian giảm mà chất lượng vẫn đạt, sản phẩm đang giúp hoàn thành core job nhanh hơn.

### 5. Counter-metrics

- **Tỷ lệ bộ slide bị giáo viên đánh dấu cần chỉnh sửa lớn hoặc bị bỏ sau khi xem.** Không được tăng khi số bộ slide duyệt tăng.
- **Chi phí AI trung bình trên mỗi `lesson_deck_approved`.** Không để tăng trưởng core action làm chi phí vận hành mất kiểm soát.
- **Tỷ lệ nội dung bị giáo viên đánh dấu sai kiến thức hoặc không phù hợp cấp lớp.** Không được xấu đi khi tối ưu tốc độ tạo slide.

**Kết luận Gate 3 — phần Metric:** Activation có đủ start event, activation event và time window; NSM có unit, quality threshold, frequency; có leading indicators và counter-metrics.

---

## 04 — Retention Definition

| Thành phần | Câu trả lời |
|---|---|
| **Unit** | Một teacher account đã hoàn tất activation. |
| **Cohort entry** | Giáo viên có `lesson_deck_approved` đầu tiên trong thời gian phân tích. |
| **Return event** | `lesson_deck_approved` cho một `lesson_id` khác với bài học dùng để vào cohort. |
| **Window** | Chu kỳ 7 ngày tiếp theo sau cohort entry, tổng hợp theo tuần; có thể đối chiếu thêm chu kỳ 14 ngày vì giáo viên thường chuẩn bị nhiều bài trong hai tuần. |
| **Threshold** | Ít nhất 1 return event trong mỗi chu kỳ 7 ngày tiếp theo. Với phân tích retention mạnh hơn, dùng thêm ngưỡng 2 bài học trong 14 ngày. |
| **Segment** | Giáo viên phổ thông đã activated và có ít nhất một bài học hợp lệ; loại trừ tài khoản nội bộ, bot và tài khoản kiểm thử. |

**Retention statement:**

> Weekly teacher retention là tỷ lệ giáo viên đã duyệt bộ slide đầu tiên trong cohort entry và tiếp tục duyệt ít nhất một bộ slide cho bài học khác trong mỗi cửa sổ 7 ngày tiếp theo; tính trên segment giáo viên phổ thông đã activated, loại trừ tài khoản nội bộ, bot và tài khoản kiểm thử.

Retention này khớp với cadence theo dự án/bài học và được đo ở cấp teacher account. Không dùng D7 như một con số trơ trọi không có cohort, return event, threshold và segment.

**Liên hệ NSM:** NSM 14 ngày dùng ngưỡng cao hơn là ít nhất 2 bộ slide được duyệt cho bài học khác nhau, còn retention tuần dùng ngưỡng tối thiểu 1 return event để quan sát việc quay lại theo cadence tự nhiên.

---

## 05 — Product Loop

### Loop chính: Project/progress loop của giáo viên

#### Chu kỳ 1 — Chuẩn bị bài học hiện tại

`Có bài học sắp dạy / cần chuẩn bị`  
→ `Gửi chủ đề và tài liệu bài học`  
→ `AI tạo dàn ý và bản nháp slide`  
→ `Giáo viên xem, chỉnh sửa và duyệt bộ slide`  
→ `Có bộ slide dùng được; bài học và phiên bản đã duyệt được lưu lại`  
→ `Giảm thời gian chuẩn bị bài và tạo tiền đề cho bài học tiếp theo`.

#### Chu kỳ 2 — Chuẩn bị bài học tiếp theo

`Lịch dạy xuất hiện bài học mới hoặc giáo viên cần cập nhật nội dung`  
→ `Tạo yêu cầu mới hoặc tái sử dụng mẫu/tài liệu đã lưu`  
→ `AI tạo bản nháp phù hợp với bài học mới`  
→ `Giáo viên duyệt bộ slide thứ hai`  
→ `Kho bài học, mẫu thiết kế và lịch sử chỉnh sửa tăng lên`  
→ `Lần chuẩn bị sau nhanh hơn và có lý do tự nhiên để tiếp tục dùng sản phẩm`.

**Reason to return nếu bỏ notification:** giáo viên vẫn quay lại vì lịch dạy phát sinh bài học mới hoặc cần cập nhật bài học; sản phẩm còn lưu mẫu, tài liệu và phiên bản đã duyệt giúp lần sau ít công sức hơn.

### Metric hypothesis

> Nếu loop này hoạt động, **NSM — số giáo viên có ít nhất 2 bộ slide cho các bài học khác nhau được duyệt trong 14 ngày** sẽ **tăng** trong **4 tuần**, vì mỗi lần duyệt tạo ra giá trị dùng được ngay, lưu lại đầu tư (mẫu, tài liệu, lịch sử chỉnh sửa) và tạo lý do tự nhiên để quay lại khi có bài học tiếp theo.

**Metric phụ theo dõi loop:** weekly teacher retention tăng; số `lesson_deck_approved` trên mỗi giáo viên activated tăng; tỷ lệ chỉnh sửa lớn và chi phí AI trên mỗi bộ slide duyệt không tăng quá mức.

**Kết luận Gate 4 — phần Loop:** Có ít nhất hai chu kỳ, reason to return đến từ bài học/lịch dạy và saved state, không phụ thuộc riêng vào notification, streak hoặc badge.

---

## 06 — Tracking nhanh

### Bảng event

| Tên event | Ý nghĩa | Thời điểm ghi nhận | Metric sử dụng |
|---|---|---|---|
| `first_lesson_request_submitted` | Giáo viên bắt đầu yêu cầu tạo bộ slide đầu tiên. | Sau khi hệ thống lưu thành công yêu cầu đầu tiên với `lesson_id` hợp lệ; không ghi khi chỉ mở form. | Activation start event; activation rate. |
| `lesson_outline_accepted` | Giáo viên chấp nhận dàn ý cho bài học để tiếp tục tạo/chỉnh sửa slide. | Khi trạng thái dàn ý chuyển từ `generated` sang `accepted` và được lưu thành công. | Leading indicator: tỷ lệ dàn ý được chấp nhận. |
| `lesson_deck_generated` | Hệ thống đã tạo xong bản nháp slide có thể xem. | Khi toàn bộ các slide bắt buộc của bản nháp đã được tạo và bản nháp ở trạng thái `ready_to_review`; retry không ghi bản sao nếu cùng `generation_id`. | Tỷ lệ bản nháp sẵn sàng; thời gian từ request đến draft. |
| `lesson_deck_review_completed` | Giáo viên đã xem và hoàn tất phiên kiểm tra bản nháp. | Khi giáo viên lưu kết quả review cho tất cả slide bắt buộc, không phải khi chỉ mở bản nháp. | Leading indicator: tỷ lệ đi từ draft đến review. |
| `lesson_deck_approved` | Giáo viên duyệt bộ slide đạt completion rule cho một bài học. | Khi trạng thái chuyển lần đầu từ `in_review` sang `approved`, có tiêu đề, mục tiêu, nội dung chính và yếu tố trực quan hợp lệ. | Core value event; activation; frequency; NSM; retention. |
| `lesson_deck_exported` | Giáo viên xuất bộ slide đã duyệt ra PowerPoint/PDF. | Sau khi file xuất thành công và hệ thống trả về file hợp lệ; không ghi khi người dùng chỉ bấm nút nhưng export lỗi. | Evidence of value phụ; depth/quality đối chiếu. |
| `lesson_deck_major_revision_marked` | Bộ slide bị giáo viên đánh dấu cần chỉnh sửa lớn sau khi xem. | Khi giáo viên lưu lựa chọn `major_revision_required`; mỗi phiên review của cùng một bản nháp chỉ ghi một lần. | Counter-metric: tỷ lệ chỉnh sửa lớn. |
| `lesson_deck_reused` | Giáo viên tái sử dụng mẫu hoặc tài liệu đã lưu cho một bài học mới. | Khi một saved template/material được áp dụng thành công cho `lesson_id` mới. | Leading indicator về saved state và product loop. |

### Acceptance criteria

1. **Hoàn tất mới ghi nhận value event:** Với mỗi cặp `teacher_id` và `lesson_id`, hệ thống chỉ ghi `lesson_deck_approved` khi bộ slide chuyển từ `in_review` sang `approved` và đạt đủ các trường bắt buộc. Bấm nút nhiều lần, tải lại trang hoặc autosave không tạo thêm event cho cùng lần duyệt.
2. **Retry/export không ghi sai event:** `lesson_deck_generated` chỉ được ghi khi bản nháp ở trạng thái `ready_to_review`; các lần retry của cùng `generation_id` không tạo bản ghi trùng. `lesson_deck_exported` chỉ được ghi sau khi file PowerPoint/PDF được tạo thành công, không ghi khi export lỗi hoặc người dùng mới bấm nút.
3. **Bài học khác nhau cho retention:** Khi tính return event, `lesson_deck_approved` chỉ được tính nếu `lesson_id` khác cohort entry của giáo viên; chỉnh sửa hoặc duyệt lại cùng một bài học không được tính là một bài học mới.
4. **Loại trừ dữ liệu không hợp lệ:** Các event từ tài khoản nội bộ, bot và tài khoản kiểm thử phải có cờ loại trừ để không đi vào activation, NSM hoặc retention.

**Kết luận Gate 5:** Có 8 event, mỗi event map tới ít nhất một metric hoặc một counter-metric; event được ghi khi hành vi đã xảy ra, không phải chỉ khi người dùng có ý định.

---

## Revision

- **Bản nháp ban đầu:** chọn "AI tạo slide" làm core action.
- **Điều chỉnh:** đổi thành "giáo viên xem, chỉnh sửa và duyệt bộ slide cho một bài học" vì output do AI tạo ra chưa chứng minh người dùng nhận được value.
- **Bản nháp ban đầu:** dự định đo daily active hoặc D7 retention.
- **Điều chỉnh:** chọn cadence theo dự án/bài học và đo theo tuần ở cấp teacher account; retention có cohort, return event, window, threshold và segment rõ ràng.
- **Lý do:** hành vi phụ thuộc vào lịch dạy và bài học mới, không phát sinh đều đặn mỗi ngày.
