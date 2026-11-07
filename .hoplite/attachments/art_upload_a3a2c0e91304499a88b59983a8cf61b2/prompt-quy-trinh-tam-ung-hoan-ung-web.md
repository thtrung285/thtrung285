# Prompt — Xây dựng quy trình Tạm ứng / Hoàn ứng (Giải trình) trên Web

> **Cách dùng file này:** Copy toàn bộ phần từ mục **“BẮT ĐẦU PROMPT”** đến **“HẾT PROMPT”** và dán cho AI / dev / đơn vị làm web.
> Phần còn lại trong prompt đã bao gồm đầy đủ quy trình cũ trên Google Sheet và quy trình mới muốn làm trên web, để người nhận không phải hỏi lại từ đầu.

---

## BẮT ĐẦU PROMPT

Bạn là chuyên gia phân tích nghiệp vụ + thiết kế sản phẩm + lập trình web. Hãy giúp tôi chuyển quy trình **Tạm ứng – Hoàn ứng / Giải trình chi phí dự án** đang làm bán thủ công trên Google Sheet sang một hệ thống web.

**Yêu cầu quan trọng về cách làm việc:**

> **Đừng làm quá cứng nhắc theo đúng từng chữ trong prompt này.**
> Đây là quy trình tôi đang làm thật và cách tôi *hình dung* quy trình mới — không phải đặc tả đã hoàn hảo.
> Nếu bạn thấy điểm nào bất hợp lý, dễ sai số tiền, dễ tính trùng, thao tác thừa, rủi ro mất dữ liệu, hoặc có ý tưởng / luồng / cấu trúc tốt hơn, gọn hơn, an toàn hơn — **hãy chủ động đề xuất thay thế, giải thích rõ lý do, ưu – nhược điểm và ảnh hưởng tới số liệu.** Đừng im lặng làm theo một điểm sai.
> Chỉ có một nhóm nguyên tắc **không được tự ý thay đổi** vì liên quan trực tiếp đến tiền — tôi đã đánh dấu riêng ở Mục 7. Ngoài nhóm đó, mọi đề xuất cải tiến đều được hoan nghênh. Trước khi code, hãy trả phần nhận xét + đề xuất trước để tôi duyệt.

---

### 1. Bối cảnh

- Công ty: CTS Vina. Người vận hành chính hằng ngày là **kế toán**.
- Nhân viên / kỹ sư (gọi chung là NV) đi công trường / dự án, phát sinh khoản chi, báo cáo khoản chi + chứng từ cho kế toán qua Zalo / email.
- Kế toán nhập khoản chi, theo dõi tạm ứng của từng NV, tính số dư tạm ứng, và định kỳ tạo **Đề nghị hoàn ứng (ĐNHƯ)** — trong quy trình này còn gọi là **Đề nghị giải trình** — cho từng NV theo mẫu có sẵn.
- Mục tiêu lên web: bỏ các bước cập nhật tay dễ sai, sổ sách tự cập nhật, tạo ĐNHƯ tự lấy đúng số liệu, gửi được cho NV, và số liệu vẫn đồng bộ về Google Sheet.

**Thuật ngữ dùng trong prompt này:**

| Thuật ngữ | Nghĩa |
|---|---|
| Khoản chi | Một khoản NV đã chi cho dự án, do kế toán nhập vào hệ thống |
| Tạm ứng | Tiền công ty ứng trước cho NV |
| Đã ghi sổ / Chưa ghi sổ | Tạm ứng đã được ghi nhận chính thức vào sổ hay chưa. Chỉ tạm ứng đã ghi sổ mới được tính trong một số phép tính số dư (xem kỹ Mục 5 và 7) |
| Duyệt / Từ chối | Khoản chi đủ / không đủ điều kiện về giá tiền, loại chi, chứng từ… theo quy định công ty đặt ra |
| Đã giải trình / Chưa giải trình | Khoản chi đã / chưa được đưa vào một ĐNHƯ. Đã giải trình rồi thì không giải trình lại lần nữa |
| ĐNHƯ | Đề nghị hoàn ứng / Đề nghị giải trình |
| ĐNTƯ | Đề nghị tạm ứng |
| Sổ theo dõi thu chi | Bảng theo dõi theo từng NV: tạm ứng vào – khoản chi ra – số dư còn lại |
| Số tạm ứng trước | Trong một ĐNHƯ: tổng các khoản tạm ứng của NV đó **chưa ghi sổ** tại thời điểm tạo |
| Số dư tạm ứng | Số dư của NV đó tại thời điểm làm ĐNHƯ, tính theo nguyên tắc ở Mục 5.3 và 7 |

---

### 2. Quy trình CŨ — đang làm trên Google Sheet (bán tự động)

Hãy hiểu đúng quy trình cũ này trước khi thiết kế quy trình mới, vì logic tiền của quy trình mới kế thừa từ đây.

#### 2.1. Cấu trúc các tab

**a) Tab nhập khoản chi**
- Là nơi kế toán nhập mọi khoản chi NV báo cáo trên Zalo.
- Có các cột đáng chú ý:
  - Cột dropdown tick **hồ sơ thiếu (hoặc đủ)** — tick các chứng từ còn thiếu / đã đủ.
  - Cột **Duyệt** — Duyệt / Từ chối.
  - Cột **Giải trình** — đánh dấu khoản chi đã được làm ĐNHƯ hay chưa.
- Quy tắc Duyệt: nếu khoản chi đủ các điều kiện về giá tiền, loại chi, chứng từ… mà công ty đặt ra thì **Duyệt**, còn lại là **Từ chối**.
- Ý nghĩa cột Giải trình: khi chọn khoảng thời gian + NV để tạo ĐNHƯ, những khoản **đã đánh dấu giải trình thì mặc định bỏ qua**. Nói dễ hiểu: đã giải trình thì không giải trình nữa.

**b) Tab Nhân viên và Tab Dự án**
- Bản chất chỉ để **lọc khoản chi theo NV và theo dự án** cho dễ theo dõi, không có nghiệp vụ đặc biệt.
- Riêng tab Nhân viên còn một nhiệm vụ nữa: sau khi lọc thời gian ở đây, lúc tạo giải trình sẽ **giải trình theo đúng khoảng thời gian đã chọn** đó.

**c) Tab Tạm ứng**
- Ghi lại các khoản tạm ứng của từng NV, mục đích để tính số dư tạm ứng.
- Có cột kiểm tra **đã ghi sổ hay chưa**.
- Chỉ khi **đã ghi sổ** thì khoản tạm ứng đó mới được tính vào số dư tạm ứng.

**d) Tab Theo dõi thu chi**
- Dùng để tính và theo dõi **số dư tạm ứng theo từng NV** (mỗi NV một bảng / một khu vực riêng).
- Dữ liệu đưa vào bảng này **chỉ gồm**:
  - Các khoản chi **đã giải trình**, và
  - Các khoản tạm ứng **đã ghi sổ**.
- Thao tác “Cập nhật sổ thu chi” cũng áp dụng đúng điều kiện lọc trên.

**e) Mẫu ĐNHƯ có sẵn**
- File mẫu có sẵn (form mẫu), mỗi lần tạo là lấy dữ liệu thay vào mẫu.

#### 2.2. Cách tạo ĐNHƯ trong quy trình cũ

- Tạo **cho từng nhân viên một**.
- Lấy các khoản chi **chưa giải trình** và **nằm trong khoảng thời gian được chọn**.
- Dữ liệu thay vào mẫu gồm: Tên NV, Dự án, Ngày giải trình, danh sách các khoản chi, **Số tạm ứng lần trước**, **Số dư tạm ứng**. Trong đó:
  - **Số tạm ứng trước** = các khoản tạm ứng của NV đó mà **chưa ghi sổ**.
  - **Số dư tạm ứng** = số dư của NV đó **tại thời điểm làm ĐNHƯ**.
- **Dạng file xuất:** 2 lựa chọn — `Excel` hoặc `HTML`.
- **Nơi xuất:** 2 lựa chọn — tải **về máy**, hoặc tạo **theo cây thư mục trên Google Drive**.
- Nếu ĐNHƯ được xuất lên Drive: các khoản chi trong ĐNHƯ đó sẽ có công thức để **tự đánh dấu vào cột Đã giải trình** ở tab nhập khoản chi ban đầu, kèm **hyperlink dẫn tới thư mục Drive chứa ĐNHƯ đó**.

#### 2.3. Quy trình thao tác hằng ngày / mỗi kỳ trong quy trình cũ

1. Nhập khoản chi.
2. Cập nhật tạm ứng.
3. Cập nhật sổ thu chi.
4. Khi tạo ĐNHƯ: tự nhập Số tạm ứng trước, chọn NV, chọn khoảng thời gian.
5. Sau khi tạo xong: vào tab Tạm ứng, **đánh dấu tay là đã ghi sổ**.
6. Cập nhật lại sổ thu chi.
7. Xem xét, **so sánh tay số dư trong ĐNHƯ với số dư trong tab Theo dõi thu chi có khớp nhau không**.
8. Gửi ĐNHƯ cho từng NV.

#### 2.4. Tự đánh giá quy trình cũ

- Mức độ: **bán tự động**.
- Tạo ĐNHƯ thì đã tự động lấy được số liệu.
- Nhưng **tạm ứng và sổ thu chi vẫn phải cập nhật bằng tay** (kết hợp AppScript), nhiều bước lặp lại, dễ quên một bước là lệch số dư, và bước đối chiếu cuối cùng vẫn là so bằng mắt.

---

### 3. Quy trình MỚI — mục tiêu trên Web

Thiết kế hệ thống web thực hiện được toàn bộ luồng dưới đây. Nếu thấy luồng nào chưa tối ưu, hãy đề xuất luồng thay thế (theo yêu cầu ở đầu prompt).

#### 3.1. Nhập liệu hằng ngày

- Kế toán nhập khoản chi như hằng ngày (NV vẫn gửi khoản chi + chứng từ qua Zalo / email ở ngoài hệ thống).
- Kế toán nhập tạm ứng khi có phát sinh.
- Ngoài 2 việc nhập liệu trên, kế toán **không phải bấm “cập nhật” thủ công cho sổ thu chi nữa**.

#### 3.2. Sổ thu chi tự động — có thay đổi so với cũ

- Sổ thu chi **tự động cập nhật toàn bộ, theo thời gian thực**.
- Khác với cũ ở điểm này: sổ thu chi bản mặc định chỉ lấy:
  - Các khoản chi **đã giải trình**, và
  - **Toàn bộ tạm ứng, không phân biệt đã ghi sổ hay chưa**.
- Mục đích của thay đổi: theo dõi số dư **chuẩn xác / thực tế hơn**, không phải chờ ghi sổ mới thấy số.
- Nhưng vẫn phải có **một nút tick / toggle cho phép xem sổ thu chi ở chế độ KHÔNG lấy các tạm ứng chưa ghi sổ** (tức là chế độ cũ: chỉ tính tạm ứng đã ghi sổ), để đối chiếu khi cần.
- **Lưu ý khi thiết kế:** hai chế độ này sẽ cho ra hai số dư khác nhau. Hãy đề xuất cách đặt tên / ghi nhãn 2 số dư này cho thật rõ (ví dụ: số dư thực tế / số dư đã ghi sổ) để người xem không nhầm — và cảnh báo tôi nếu bạn thấy cách làm này dễ gây rối.

#### 3.3. Tạo ĐNHƯ trên web

Khi kế toán bấm **Tạo ĐNHƯ**, hệ thống cho chọn:

- Nhân viên
- Khoảng thời gian
- Tiêu đề ĐNHƯ
- Có lấy các khoản **bị Từ chối** hay không (mặc định: không lấy / chỉ lấy khoản Duyệt — có tùy chọn mới gộp cả khoản Từ chối)
- Loại file xuất: Excel hoặc HTML
- Địa chỉ / nơi xuất: về máy hoặc theo cây thư mục trên Drive

Sau khi xác nhận, web **tự động**:

1. Lấy các khoản chi **chưa giải trình**, thuộc NV + khoảng thời gian đã chọn, và thỏa điều kiện Duyệt / Từ chối ở lựa chọn trên.
2. Lấy **Số tạm ứng trước** = **tổng** các khoản tạm ứng **chưa ghi sổ** của NV đó (nếu có nhiều khoản thì cộng tổng, không lấy riêng từng khoản).
3. Lấy **Số dư** = số dư tính từ các khoản **đã giải trình và đã ghi sổ**.
   - Lý do phải tính như vậy: **để không bị tính trùng 2 lần tạm ứng**. Đây là nguyên tắc tiền quan trọng — xem thêm Mục 7.
4. Thay toàn bộ dữ liệu còn lại (Tên, Dự án, Ngày giải trình, danh sách khoản chi…) vào **form mẫu ĐNHƯ như cũ** và xuất file như cũ.
5. **Đánh dấu các khoản chi vừa dùng là Đã giải trình**, và cho phép truy cập thẳng tới **thư mục chứa ĐNHƯ / giải trình đó** (link Drive hoặc link trong hệ thống).
6. **Tự cập nhật sổ thu chi** — điều này được coi là đương nhiên vì các khoản chi đó giờ đã thành Đã giải trình, nhưng hãy đảm bảo cơ chế của bạn thực sự làm được, không phải chỉ ghi chú.

#### 3.4. Sau khi tạo ĐNHƯ — gửi cho NV và cập nhật phía NV

- ĐNHƯ được **gửi tới NV** tương ứng.
- Đồng thời **cập nhật danh sách Theo dõi thu chi riêng của NV đó**, với dữ liệu chỉ gồm:
  - Các khoản chi **đã giải trình**, và
  - Các khoản tạm ứng **đã ghi sổ**.

#### 3.5. Đồng bộ Google Sheet

- Toàn bộ số liệu trên web phải **tự động cập nhật sang Google Sheet**.
- Hãy đề xuất rõ kiến trúc đồng bộ bạn chọn: Sheet là kho gốc duy nhất (web ghi qua cầu nối / AppScript), hay web có kho riêng và Sheet chỉ là bản đồng bộ / báo cáo? Nêu rõ chiều đồng bộ, điều gì xảy ra khi mất kết nối / đồng bộ lỗi, và cách tránh sửa 2 nơi cùng lúc gây xung đột. Đừng để đồng bộ lỗi mà người dùng tưởng là đã lưu.

---

### 4. Yêu cầu UI / UX theo vai trò

Được phép đề xuất lại cách chia tab / màn hình nếu bạn thấy hợp lý hơn cách dưới đây — miễn giải thích rõ. Đây là mong muốn ban đầu:

#### 4.1. Phía Kế toán

- **Tab Khoản chi:** theo dõi các khoản chi **và** nhập khoản chi mới **ngay trong cùng tab**.
- **Tab Tạm ứng:** theo dõi tạm ứng **và** nhập khoản tạm ứng mới **ngay trong cùng tab**.
- **Tab Sổ thu chi / Số dư:** theo dõi số dư theo từng NV (kèm toggle 2 chế độ xem ở Mục 3.2).
- **Tất cả các tab** đều lọc được theo: **Nhân viên, Dự án, Khoảng thời gian**. Nếu thấy lọc theo dự án nên tách tab riêng thì cứ đề xuất.
- **Tab Danh mục:** thêm / sửa Nhân viên, Dự án.
- Nút / màn hình **Tạo ĐNHƯ** với đầy đủ lựa chọn ở Mục 3.3, có màn hình xem trước / kiểm tra số liệu trước khi chốt tạo (đề xuất thêm nếu chưa có).

#### 4.2. Phía Nhân viên

- **Tab Theo dõi thu chi của bản thân:** chỉ xem của chính mình, lọc được theo dự án / thời gian như phía kế toán.
  - Chi tiết từng khoản chi của NV chỉ nên hiện **sau khi khoản đó đã được đưa vào một ĐNHƯ / đã giải trình** (theo cách vận hành hiện tại của chúng tôi). Nếu bạn đề xuất cho NV xem sớm hơn, hãy nêu rõ rủi ro / lợi ích.
- **Tab nhận file ĐNHƯ:** danh sách ĐNHƯ của mình, bấm vào để xem / tải.
  - Có nút **gửi email file đó ngay**: mở form email đã có sẵn nội dung + file đính kèm ngay trên web, NV / người gửi điền / kiểm tra rồi bấm gửi thì email đi qua hệ thống email của **Mắt Bão** (Email Pro công ty đang dùng) với đúng nội dung + file đính kèm đã nhập ở web.
  - Có thêm **một nút tạo Đề nghị tạm ứng (ĐNTƯ)** và gửi tương tự như trên.
- NV **không** được xem dữ liệu của NV khác.

#### 4.3. Yêu cầu chung

- Hãy đề xuất rõ: dùng trên máy tính là chính hay cần tốt trên cả điện thoại? (Hãy hỏi lại tôi nếu điểm này ảnh hưởng lớn đến thiết kế.)
- Mọi thao tác liên quan đến tiền phải có xác nhận trước khi chốt, có lịch sử / log ai làm – lúc nào – thay đổi gì, và có đường lui khi tạo nhầm ĐNHƯ (hủy ĐNHƯ thì các khoản chi phải quay về Chưa giải trình — hãy thiết kế cơ chế này).

---

### 5. Mô hình dữ liệu & trạng thái — hãy thiết kế và trình bày rõ

Hãy tự xây dựng mô hình dữ liệu tối thiểu nhưng đủ cho các đối tượng: Nhân viên, Dự án, Khoản chi, Tạm ứng, ĐNHƯ / Đề nghị giải trình, Dòng sổ thu chi, File đã xuất, Email đã gửi.

Đặc biệt, làm rõ các **trạng thái độc lập** của một khoản chi / một khoản tạm ứng, đừng gộp chung thành một trạng thái duy nhất:

- Khoản chi: `Duyệt / Từ chối` — tách riêng với — `Chưa giải trình / Đã giải trình (thuộc ĐNHƯ nào, ngày nào, link thư mục nào)`
- Tạm ứng: `Chưa ghi sổ / Đã ghi sổ`
- ĐNHƯ: nháp / đã tạo thành công / đã gửi / đã hủy (đề xuất thêm nếu cần)

---

### 6. Đầu ra mong đợi từ bạn

**Đừng code ngay toàn bộ hệ thống.** Hãy trả lời theo thứ tự này:

1. **Nhận xét quy trình (quan trọng nhất):** Quy trình cũ + quy trình mới ở trên có điểm nào bất hợp lý, mâu thuẫn, dễ lệch tiền, dễ tính trùng, thao tác thừa, hoặc rủi ro về đồng bộ Drive / Sheet / Email không? Điểm nào nên giữ nguyên, điểm nào nên sửa — sửa như thế nào?
2. **Đề xuất quy trình mới tối ưu:** Sơ đồ luồng (flow) từng bước cho: nhập khoản chi → nhập tạm ứng → sổ thu chi tự động → tạo ĐNHƯ → gửi NV → đồng bộ Sheet. Ghi rõ ở mỗi bước dữ liệu nào thay đổi, trạng thái nào lật.
3. **Công thức tiền:** Viết thành công thức / giả mã (pseudo-code) rõ ràng cho: Số tạm ứng trước, Số dư trong ĐNHƯ, Số dư sổ thu chi ở cả 2 chế độ xem. Kèm **ít nhất 2 ví dụ số cụ thể** (có cả tạm ứng chưa ghi sổ) để chứng minh không tính trùng.
4. **Cấu trúc màn hình / UI:** Danh sách tab / màn hình cho Kế toán và cho NV, bộ lọc, nút chính, và luồng tạo ĐNHƯ từng bước trên màn hình.
5. **Cấu trúc dữ liệu + kiến trúc đồng bộ Google Sheet / Drive / Email Mắt Bão:** Đề xuất cụ thể, kèm cách xử lý khi một trong các kết nối này lỗi.
6. **Lộ trình làm:** Chia giai đoạn (ví dụ: bản mẫu để duyệt luồng → bản chạy thật với dữ liệu thật → phần gửi email / ĐNTƯ), giai đoạn nào cần tôi cung cấp mẫu ĐNHƯ, mẫu Sheet, cấu trúc thư mục Drive, thông tin email Mắt Bão.
7. **Danh sách câu hỏi cần tôi chốt:** Chỉ hỏi những điểm thật sự làm thay đổi thiết kế / số tiền. Đừng tự bịa số liệu, mẫu file, hay quy định Duyệt / Từ chối mà tôi chưa cung cấp.

---

### 7. Nguyên tắc tiền — KHÔNG được tự ý thay đổi (chỉ được cảnh báo nếu thấy sai)

1. **Đã giải trình thì không giải trình lại.** Một khoản chi chỉ thuộc về một ĐNHƯ.
2. **Số dư dùng trong ĐNHƯ phải tránh tính trùng tạm ứng 2 lần.** Vì vậy số dư trong ĐNHƯ lấy theo các khoản đã giải trình + tạm ứng đã ghi sổ, còn tạm ứng chưa ghi sổ được trình bày riêng ở dòng Số tạm ứng trước — không cộng gộp cả hai vào cùng một số dư.
3. **Chỉ đánh dấu Đã giải trình sau khi ĐNHƯ đã tạo thành công** (file đã xuất / đã lưu được). Nếu xuất file lỗi, không được đánh dấu.
4. Số liệu giữa ĐNHƯ, Sổ thu chi của kế toán, và Sổ thu chi NV nhìn thấy **phải đối chiếu khớp nhau** theo cùng một nguyên tắc tính — nếu 2 chế độ xem sổ (Mục 3.2) cho 2 số khác nhau thì phải ghi nhãn khác nhau, không được để cùng một tên “Số dư” mà hai nơi hai số.
5. Mọi thay đổi trạng thái Ghi sổ / Giải trình đều phải lưu vết (ai, lúc nào, thuộc ĐNHƯ nào).

Ngoài 5 nguyên tắc trên, nếu bạn có cách làm khác tốt hơn — **cứ đề xuất, tôi sẽ xem và quyết định.**

---

### 8. Các điểm tôi tự thấy có thể chưa hợp lý — bạn hãy kiểm tra kỹ

Đây là những điểm tôi nhờ bạn soi giúp, đừng mặc định là đúng:

- Sổ thu chi mặc định lấy **toàn bộ tạm ứng** nhưng số dư trong ĐNHƯ lại chỉ lấy **tạm ứng đã ghi sổ** — hai nơi lệch nhau có làm người dùng rối không? Có nên tách thành 2 chỉ số có tên riêng không?
- Gửi email qua Mắt Bão ngay từ web: cần SMTP / API / xác thực như thế nào cho an toàn? Có nên để NV tự gửi, hay kế toán mới là người gửi ĐNHƯ?
- Nút tạo ĐNTƯ ở phía NV: NV tạo xong gửi thẳng đi hay cần qua kế toán duyệt trước?
- Đồng bộ 2 chiều Web ↔ Google Sheet có nguy cơ trùng / xung đột không? Nên chốt **một nơi là nguồn gốc duy nhất (single source of truth)** — nơi đó nên là Web hay Sheet?
- Khoản **Từ chối** khi được gộp vào ĐNHƯ: có cộng vào tổng tiền hoàn ứng không, hay chỉ liệt kê để biết? Cần hiển thị tách riêng như thế nào?
- Nếu tạo nhầm ĐNHƯ: cơ chế hủy / làm lại nên như thế nào để không mất dấu vết kiểm toán?

Hãy trả lời từng điểm, hoặc đề xuất thêm điểm mới mà tôi chưa nghĩ tới.

## HẾT PROMPT

---

### Ghi chú ngoài prompt (cho người giữ file này)

- Quy trình cũ trong file này lấy nguyên từ mô tả của người vận hành ngày 08/11/2026 (`instruction.docx`).
- Các nguyên tắc đã chốt ở các phiên làm việc trước và đã được đưa vào prompt: số dư mở đầu / snapshot để tránh tính trùng 2 lần, khoản Duyệt mặc định khi tạo đề nghị — khoản Từ chối chỉ gộp khi có tùy chọn, chi tiết khoản chi chỉ hiện cho NV sau khi đã giải trình, và kiến trúc Sheet là kho dữ liệu cần đồng bộ.
- Khi gửi prompt này đi, nên đính kèm thêm nếu có: file Google Sheet mẫu hiện tại, file mẫu ĐNHƯ (Excel), cây thư mục Drive mẫu, và quy định chi tiết về điều kiện Duyệt / Từ chối (loại chi + chứng từ bắt buộc).
