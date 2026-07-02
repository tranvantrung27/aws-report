---
title: "Event 1"
date: 2026-05-30
weight: 1
chapter: false
pre: " <b> 4.1. </b> "
---

# Event CD AWS

### Thông tin sự kiện

- **Tên sự kiện:** Event CD AWS (AWS Community Day)
- **Thời gian:** 09:00 ngày 30-05-2026
- **Địa điểm:** Tầng 26, tòa nhà Bitexco, số 02 đường Hải Triều, phường Sài Gòn, thành phố Hồ Chí Minh
- **Vai trò:** Người tham dự

### Mục đích sự kiện
* **Kết nối và giao lưu:** Tạo cơ hội giao lưu, kết nối giữa cộng đồng phát triển AWS, sinh viên và các chuyên gia công nghệ điện toán đám mây tại Việt Nam.
* **Chia sẻ kiến thức thực tế:** Lắng nghe và tiếp thu các kiến thức thực chiến thông qua 5 chuyên đề chuyên sâu từ các diễn giả: từ lộ trình rèn luyện kỹ năng AWS (Cloud Quest, Floci), kinh nghiệm thực chiến từ cuộc thi Hackathon, phương pháp xây dựng sự tự tin, phát triển startup AI thực tế (Tử Vi Đại Việt), cho đến văn hóa DevOps trong vận hành dự án.
* **Tiếp cận xu hướng mới:** Cập nhật các xu hướng công nghệ mới như Generative AI (Amazon Bedrock), các giải pháp kiến trúc Serverless và kỹ thuật tối ưu hóa kiểm thử local.

### Danh sách Diễn giả & Chủ đề

1. **Huỳnh Thái Linh** — Chủ đề: *Level up your AWS skills with Cloud Quest and Floci*
2. **Huỳnh An Khương, Mai Quốc Anh, Nguyễn Trần Minh Quân** — Chủ đề: *Hackathon*
3. **Nguyễn Thị Quỳnh Như** — Chủ đề: *Why We Always Need Confidence*
4. **Nghĩa Trần** — Chủ đề: *Tự làm web Tử vi Đại Việt* (https://tuvidaiviet.com/)
5. **Trần Minh Quân** — Chủ đề: *The Hidden Iceberg of a Project: DevOps Before Disaster*

---

## Nội dung chi tiết các phần chia sẻ

### Phần 1: Level Up Your AWS Skills with Cloud Quest and Floci (Diễn giả: Huỳnh Thái Linh)

#### 1. Vấn đề người mới học AWS thường gặp
Người mới tiếp cận điện toán đám mây AWS thường gặp các rào cản tâm lý và kỹ thuật lớn:
* **Lo lắng về chi phí:** Thường xuyên phải kiểm tra hóa đơn (billing) vì sợ phát sinh chi phí ngoài dự kiến.
* **Quên xóa tài nguyên:** Rất dễ quên tắt hoặc xóa tài nguyên sau khi thực hành (ví dụ: EC2, RDS, NAT Gateway), dẫn đến các hóa đơn bất ngờ vào cuối tháng.
* **Sợ vượt quá hạn mức:** Lo sợ bị tính phí mặc dù đang sử dụng gói Free Tier.

{{% notice info "Thông điệp chính" %}}
**Thông điệp chính:** Nỗi sợ chi phí phát sinh là rào cản tâm lý lớn nhất cản trước người học thực hành và trải nghiệm AWS thực tế.
{{% /notice %}}

#### 2. AWS Cloud Quest – Môi trường học thực hành miễn phí
AWS Cloud Quest là nền tảng học tập dạng game hóa (gamification) 3D trực quan, hỗ trợ người mới làm quen với AWS một cách an toàn và thú vị.
* **Đặc điểm nổi bật:**
  * **Guided AWS Practice:** Thực hành có hướng dẫn chi tiết từng bước.
  * **Dễ tiếp cận:** Giao diện trực quan, thân thiện với người mới bắt đầu.
  * **Học qua nhiệm vụ:** Hoàn thành các thử thách để giải quyết bài toán thực tế cho thành phố ảo.
  * **Mô phỏng thực tế:** Thực hành trực tiếp trên tài nguyên AWS thật do hệ thống cấp sẵn (không tốn chi phí cá nhân).
* **Hệ thống Chứng nhận (Badges) kỹ năng:** Người học có thể chinh phục các vai trò khác nhau như:
  * *Cloud Practitioner*
  * *Solutions Architect*
  * *Serverless Developer*
  * *Machine Learning Engineer*
  * *Security Specialist*
* **Lợi ích mang lại:** Thiết lập lộ trình rõ ràng, tăng động lực học nhờ cơ chế trò chơi và không lo lắng về việc thiết lập môi trường phức tạp.

#### 3. Floci – Local AWS Testing
Floci là một công cụ mã nguồn mở gọn nhẹ đóng vai trò giả lập các dịch vụ AWS ngay trên máy cá nhân (Local AWS Service Emulator).

*(Chèn ảnh 4 minh họa tại đây)*

* **Mục tiêu chính:**
  * Giả lập dịch vụ AWS trên local.
  * Hoàn toàn không phát sinh chi phí đám mây trong quá trình phát triển.
  * Kiểm thử nhanh chóng các mô hình kiến trúc.
* **Lợi ích:**
  * Phát triển và kiểm thử ứng dụng AWS mà không cần deploy trực tiếp lên AWS thật.
  * Tiết kiệm chi phí vận hành và tăng tốc vòng đời phát triển (Fast feedback loop).
  * Hỗ trợ gỡ lỗi (debug) nhanh chóng và dễ dàng.

{{% notice tip "Thông điệp chính" %}}
**Thông điệp chính:** Hãy luôn kiểm thử ứng dụng trên môi trường local trước khi triển khai thực tế lên đám mây AWS.
{{% /notice %}}

#### 4. So sánh Floci và LocalStack
Buổi chia sẻ đã đưa ra so sánh khách quan giữa Floci và LocalStack dựa trên các tiêu chí cốt lõi:
* **Hiệu năng:** Floci cho tốc độ khởi động và phản hồi nhanh hơn khoảng **138 lần** so với LocalStack.
* **Tài nguyên hệ thống:** Tiết kiệm RAM và dung lượng lưu trữ hơn khoảng **11 lần** nhờ được tối ưu hóa bằng GraalVM Native Image.
* **Khả năng hỗ trợ dịch vụ:** LocalStack hỗ trợ phổ rộng dịch vụ hơn, trong khi Floci tập trung tối ưu hóa các dịch vụ cốt lõi (S3, SQS, SNS, v.v.).
* **Chi phí & Bản quyền:** Khác biệt về giấy phép sử dụng và mô hình định giá (Floci hướng tới mã nguồn mở và miễn phí hoàn toàn).

#### 5. Hạn chế của Floci
Mặc dù rất mạnh mẽ, Floci vẫn có một số giới hạn nhất định khi đối mặt với các kiến trúc phức tạp.

*(Chèn ảnh 6 minh họa tại đây)*

* **Kiến trúc ví dụ:** Các hệ thống xử lý dữ liệu lớn (Data Analytics) sử dụng kết hợp nhiều dịch vụ như *MSK (Kafka), Lambda, Kinesis, S3, Glue, Redshift, QuickSight*.
* **Ý nghĩa thực tế:** Floci chưa thể giả lập 100% tất cả các dịch vụ chuyên sâu của AWS. Với các pipeline phân tích dữ liệu phức tạp, việc sử dụng tài nguyên AWS thật (hoặc môi trường Staging cách ly) vẫn là bắt buộc.

{{% notice warning "Kết luận" %}}
**Kết luận:** Floci cực kỳ phù hợp cho học tập, phát triển và kiểm thử nhanh tại local, nhưng chưa thể thay thế hoàn toàn môi trường Production thực tế trên AWS.
{{% /notice %}}

#### 6. Lộ trình học tập thực hành hiệu quả (Effective Practical Learning Roadmap)
Diễn giả đề xuất lộ trình học tập tối ưu gồm 3 giai đoạn:
* **Giai đoạn 1: Mindset & Kiến trúc (Mind & Architecture)**
  * *Công cụ:* AWS Cloud Quest.
  * *Mục tiêu:* Xây dựng tư duy Cloud, hiểu kiến trúc AWS căn bản và làm quen với các dịch vụ cốt lõi.
* **Giai đoạn 2: Lập trình & Kiểm thử nhanh (Code & Fast Testing)**
  * *Công cụ:* Floci (local emulation).
  * *Mục tiêu:* Viết code, kiểm thử nhanh các tích hợp và thực hành thiết kế hệ thống với chi phí $0.
* **Giai đoạn 3: Triển khai thực tế & Vận hành (Real Deployment & Production)**
  * *Công cụ:* AWS Cloud thật.
  * *Mục tiêu:* Triển khai sản phẩm thực tế, tối ưu hóa chi phí, bảo mật và giám sát hiệu năng thực tế.

---

### Phần 2: Hackathon – Học nhanh, Làm thật, Trưởng thành từ áp lực (Diễn giả: Huỳnh An Khương, Mai Quốc Anh, Nguyễn Trần Minh Quân - The Ballers Team)

#### 1. Hackathon là gì?
Mở đầu bài trình bày, nhóm diễn giả đã định nghĩa Hackathon theo một góc nhìn hài hước nhưng vô cùng thực tế:
> *"HA! a tons of fun, bug fixes, back pain and sleep deprivation"* (Một tấn niềm vui, hàng tá lỗi cần sửa, đau lưng và thiếu ngủ trầm trọng).

Thực chất, Hackathon là sự kiện phát triển sản phẩm trong giới hạn thời gian cực ngắn (thường từ 24–48 giờ liên tục). Tại đây, các đội ngũ sẽ phải:
* **Giải quyết bài toán thực tế:** Đối mặt với các bài toán hóc búa từ thực tiễn.
* **Xây dựng Prototype/MVP:** Hoàn thiện phiên bản thử nghiệm khả dụng trong thời gian ngắn nhất.
* **Làm việc với tốc độ cao:** Đòi hỏi khả năng làm việc nhóm và ra quyết định cực nhanh.
* **Liên tục thử nghiệm:** Cải tiến liên tục ý tưởng để thích ứng với phản hồi của giám khảo hoặc mentor.

* **Mục tiêu cốt lõi:** Không phải tạo ra một sản phẩm hoàn mỹ 100%, mà là biến ý tưởng thành sản phẩm nhanh nhất để chứng minh tính khả thi của giải pháp và học hỏi qua thực hành.

#### 2. Vì sao nên tham gia Hackathon?
Nhóm diễn giả đặt ra câu hỏi: *"Why should you join a Hackathon?"* và khẳng định:

{{% notice info "Thông điệp từ đội ngũ" %}}
Hackathon mang lại những trải nghiệm thực chiến cực kỳ quý giá mà việc học lý thuyết hay làm đồ án thông thường khó có thể mang lại.
{{% /notice %}}

Các giá trị thực tế nhận được bao gồm:
* Làm việc hiệu quả dưới áp lực thời gian lớn.
* Rèn luyện kỹ năng làm việc nhóm (Teamwork) và giải quyết vấn đề (Problem-solving).
* Cơ hội tiếp xúc, học hỏi từ các Mentor giàu kinh nghiệm và đại diện doanh nghiệp.
* Làm đẹp Portfolio cá nhân với sản phẩm thực tế.
* Thấu hiểu sâu sắc quy trình phát triển sản phẩm từ ý tưởng sơ khởi đến MVP.

#### 3. Hành trình Hackathon của nhóm
Hành trình 3 ngày đêm đầy cảm xúc của *The Ballers Team* được tóm tắt qua từng giai đoạn:
* **Ngày 1 (Day 1) – Khởi động:**
  * Bắt đầu một cách vui vẻ với đồ ăn (Donuts và Pizza) được ban tổ chức cung cấp.
  * Tham gia các buổi workshop định hướng và lễ khai mạc.
  * Brainstorm để tìm kiếm và định hình ý tưởng.
  * Cùng nhau "cứu" chiếc laptop của thành viên bị quá tải RAM và bước vào chuỗi ngày thiếu ngủ.
* **Ngày 2 (Day 2) – Phát triển sản phẩm:**
  * Nhóm đã phải cân nhắc quyết liệt giữa nhiều hướng đi như *SynthHunter* và *The Great Divide*.
  * Cuối cùng, cả nhóm quyết định tập trung vào giải pháp khả thi nhất phù hợp với giới hạn thời gian của cuộc thi.

#### 4. Dự án SynthHunter – AI Voice Verification System
* **Bài toán thực tế:** Sự bùng nổ của AI tạo sinh (Generative AI) làm cho việc giả mạo giọng nói (Deepfake Voice) trở nên quá dễ dàng, dẫn đến nguy cơ cao về gian lận danh tính và vượt qua các hệ thống bảo mật sinh trắc học giọng nói.
* **Giải pháp:** Nhóm xây dựng **SynthHunter** — hệ thống xác thực giọng nói nhằm phân tích và phân loại các tệp âm thanh thành 3 trạng thái:
  1. *Giọng người thật (Human)*
  2. *Giọng do AI tạo ra (AI Generated)*
  3. *Cần xem xét thêm (Needs Review)*
* **Cơ chế phân tích:** Đo lường các đặc trưng âm học chuyên sâu như động lực học giọng nói (Speech Dynamics), hành vi bộ mã hóa (Encoder Behavior), và phân tích nhịp điệu thời gian (Temporal Rhythm Analysis) để phát hiện dấu hiệu tổng hợp bằng AI.

#### 5. Kiến trúc hệ thống
Để phục vụ việc triển khai nhanh chóng trong thời gian ngắn của cuộc thi, nhóm đã xây dựng một kiến trúc **Serverless** linh hoạt trên nền tảng AWS:
* **AWS API Gateway & AWS Lambda:** Tiếp nhận yêu cầu và xử lý logic nghiệp vụ bất đồng bộ.
* **Amazon S3:** Lưu trữ các tệp tin âm thanh đầu vào.
* **Amazon Bedrock:** Kết nối và khai thác sức mạnh của các mô hình ngôn ngữ lớn (LLMs).
* **Amazon DynamoDB:** Cơ sở dữ liệu NoSQL lưu trữ thông tin giao dịch và kết quả phân tích.
* **Quy trình vận hành:** Người dùng tải file âm thanh → Hệ thống lưu trữ tại S3 → Gọi mô hình AI xử lý → Phân tích và đưa ra đánh giá → Trả kết quả kết luận về cho người dùng.

#### 6. Những khó khăn gặp phải
Thực tế cuộc thi không hề dễ dàng khi nhóm đối mặt với:
* **Dữ liệu thực tế:** Tệp âm thanh từ nhà tài trợ cung cấp chứa tạp âm và không hoàn toàn là giọng người thật, gây khó khăn cho việc huấn luyện mô hình.
* **Tài liệu thay đổi:** Các tài liệu API và cấu trúc website thay đổi liên tục vào phút chót, buộc nhóm phải chỉnh sửa documentation gấp.
* **Hiệu năng:** Thời gian phản hồi của hệ thống tương đối dài và khó tối ưu hóa trong vòng 24–48 giờ.

> [!NOTE]
> **Bài học xương máu:** Trong các cuộc thi Hackathon, thách thức lớn nhất thường không nằm ở việc viết code mà là khả năng thích ứng và giải quyết các sự cố phát sinh ngoài dự kiến.

#### 7. Dự án Vortox
Bên cạnh SynthHunter, nhóm cũng giới thiệu dự án phụ mang tên **Vortox**.
* **Bài toán:** Rào cản lớn nhất của sinh viên khi tìm kiếm việc làm không hẳn là thiếu kỹ năng chuyên môn, mà là sự thiếu hiểu biết về toàn bộ quy trình tuyển dụng thực tế.
* **Giải pháp:** Vortox hỗ trợ hành trình ứng tuyển thông qua một quy trình khép kín: Sàng lọc CV → Đánh giá hành vi → Đánh giá kỹ thuật → Theo dõi tiến trình ứng tuyển trong một hệ thống thống nhất.

#### 8. Ngày 3 (Day 3) – Pitching (Thuyết trình)
Giai đoạn nước rút cực kỳ quan trọng:
* Hoàn thiện tối đa sản phẩm chạy được (Working Demo).
* Chuẩn bị Slide thuyết trình súc tích, chuyên nghiệp.
* Demo trực tiếp hệ thống trước hội đồng.
* **Tầm quan trọng:** Một ý tưởng tốt cần được truyền đạt thật mạch lạc và thuyết phục để thu hút sự công nhận từ ban giám khảo.

#### 9. Những bài học cốt lõi (Key Learnings & Lessons Learnt)
Nhóm *The Ballers* đúc kết 4 bài học quan trọng:
1. **Bắt đầu từ một vấn đề thực tế (Start with a real problem):** Luôn bắt đầu từ nhu cầu ("pain point") thực tế của người dùng thay vì bắt đầu từ công nghệ có sẵn. Sản phẩm giải quyết được vấn đề thực tế sẽ có giá trị cao hơn nhiều so với một sản phẩm chỉ để trình diễn công nghệ.
2. **Liên tục thử nghiệm (Experiment relentlessly):** Sẵn sàng thử nghiệm, thay đổi và điều chỉnh ý tưởng khi phát hiện hướng đi cũ không hiệu quả. Nhiều thử nghiệm nhỏ sẽ mang lại kết quả tốt hơn là cố gắng theo đuổi một kế hoạch hoàn hảo trên giấy.
3. **Kiên trì là yếu tố quyết định (Persistence matters):** Đối mặt với thiếu ngủ, lỗi hệ thống, thay đổi tài liệu vào phút chót... Kiên trì vượt qua khó khăn chính là chìa khóa phân biệt giữa đội về đích và đội bỏ cuộc.
4. **Tận dụng tối đa nguồn lực (Be resourceful):** Thích nghi nhanh, tận dụng tối đa các công cụ AI, dịch vụ đám mây AWS có sẵn để tăng tốc độ phát triển sản phẩm trong thời gian ngắn ngủi.

{{% notice info "Kết luận chung" %}}
Hackathon không đơn thuần là một cuộc thi lập trình mà là mô phỏng chân thực nhất quy trình phát triển sản phẩm thực tế dưới áp lực cực lớn. Đối với sinh viên và người mới vào ngành, đây là cơ hội vàng để bứt phá giới hạn bản thân, tích lũy cả kỹ năng chuyên môn lẫn kỹ năng mềm.
{{% /notice %}}

---

### Phần 3: Why We Always Need Confidence (Diễn giả: Nguyễn Thị Quỳnh Như)

#### 1. Let's Talk Confidence – Tự tin thực sự là gì?
Diễn giả mở đầu buổi chia sẻ bằng câu nói nổi tiếng của đại văn hào Mark Twain:
> *"Courage is resistance to fear, mastery of fear—not absence of fear."*
> (Tạm dịch: Dũng cảm là đối đầu và kiểm soát nỗi sợ hãi—chứ không phải là sự biến mất của nỗi sợ).

* **Thông điệp cốt lõi:** Nhiều người lầm tưởng người tự tin là người không bao giờ sợ hãi. Thực tế:
  * Ai cũng có những nỗi sợ riêng.
  * Ai cũng có lúc nghi ngờ năng lực của bản thân.
  * Sự tự tin thực sự chính là việc **dám hành động** ngay cả khi bản thân vẫn đang lo lắng và sợ hãi.

#### 2. Cái giá của sự tự ti trong cuộc sống sinh viên
Đằng sau vẻ ngoài bình thường của nhiều sinh viên là những rào cản vô hình kìm hãm sự phát triển:
* **Cơ hội bị bỏ lỡ (Missed Chances):**
  * Biết câu trả lời chính xác nhưng chần chừ không dám giơ tay phát biểu trong lớp học.
  * Có ý tưởng độc đáo khi làm việc nhóm nhưng lại chọn cách im lặng.
  * Nhìn thấy các cơ hội thực tập, cuộc thi lớn nhưng tự giới hạn bản thân vì nghĩ *"mình chưa đủ giỏi"*.
  * *Nỗi sợ kìm hãm:* Sợ nói sai, sợ bị đánh giá, sợ bị từ chối, và sợ trở thành tâm điểm của sự chú ý. Kết quả là cơ hội biến mất trước khi bản thân kịp hành động.
* **Tiếng nói bị kìm hãm bởi sự tự ti:**
  * Nhìn thấy lỗi sai trong dự án hoặc có đề xuất giúp bài thuyết trình của nhóm tốt hơn nhưng không dám lên tiếng góp ý.
  * Luôn bị bủa vây bởi các câu hỏi: *"Mình có nên nói không?"*, *"Lỡ mình sai thì sao?"*, *"Người khác sẽ nghĩ gì về mình?"*.
  * Sự im lặng này khiến nhiều ý tưởng tốt bị chôn vùi.
* **Áp lực vô hình (Invisible Pressure) & Tiềm năng bị che khuất (Hiding Potential):**
  * Người ngoài nhìn vào chỉ thấy một sinh viên trầm tính, ít nói, nhưng bên trong có thể là sự tự ti, lo âu, overthinking và sợ bị phán xét.
  * Nhiều bạn có năng lực chuyên môn rất tốt nhưng vì sợ thất bại nên không dám thử sức, khiến tiềm năng mãi mãi bị che khuất.

{{% notice info "Thông điệp đáng nhớ" %}}
Thiếu tự tin không chỉ làm mất đi cơ hội thể hiện của cá nhân, mà còn làm tập thể và thế giới mất đi những ý tưởng và giá trị tốt đẹp mà chúng ta có thể đóng góp.
{{% /notice %}}

#### 3. Khoa học đằng sau sự tự ti
* **Tư duy cố định và tư duy phát triển (Fixed vs Growth Mindset):** Diễn giả trích dẫn quan điểm của Carol Dweck:
  > *"Why waste time proving over and over how great you are when you could be getting better?"*
  > (Tại sao phải lãng phí thời gian để liên tục chứng minh mình giỏi đến mức nào trong khi bạn có thể tập trung để trở nên tốt hơn?)
  * Người thiếu tự tin thường sợ mắc lỗi, sợ thất bại vì xem đó là thước đo đánh giá năng lực của mình (Tư duy cố định).
  * Ngược lại, người có tư duy phát triển xem thất bại là trải nghiệm học hỏi và cơ hội để hoàn thiện bản thân.
* **Hội chứng kẻ mạo danh (Imposter Syndrome):**
  * Trạng thái tâm lý phổ biến ở sinh viên, người mới đi làm và đặc biệt là người làm trong lĩnh vực công nghệ.
  * Dù đạt được nhiều thành tích tốt nhưng bản thân luôn cảm thấy mình không đủ giỏi, coi thành công chỉ là do may mắn và luôn lo sợ người khác sẽ phát hiện ra sự "yếu kém" của mình.

#### 4. Sức mạnh của sự tự tin
* **Confidence Helps People Connect:** Sự tự tin giúp chúng ta dễ dàng xây dựng và mở rộng các mối quan hệ xã hội. Người tự tin thường giao tiếp cởi mở, hợp tác hiệu quả và chủ động kết nối.
* **Trong môi trường học tập & làm việc:** Giúp trình bày ý tưởng một cách mạch lạc, tích cực tham gia thảo luận phản biện, nâng cao năng lực lãnh đạo và mở rộng mạng lưới quan hệ (networking).

#### 5. Cách "Hack" sự tự tin hằng ngày
* **Quy tắc 5 giây (The 5-Second Rule):** 
  * Cú pháp: **5 → 4 → 3 → 2 → 1 → Hành động!**
  * *Ứng dụng:* Ngay khi xuất hiện cơ hội và bộ não bắt đầu sản sinh ra các suy nghĩ trì hoãn, lo sợ (như *"mình không làm được đâu"*), hãy đếm ngược và hành động ngay lập tức trước khi sự nghi ngờ kịp kiểm soát tâm trí.
  * *Ví dụ thực tế:* Giơ tay đặt câu hỏi trong lớp, chủ động bắt chuyện với người mới, đăng ký tham gia một giải chạy/Hackathon hoặc nộp CV vào công ty mong muốn.

#### 6. Kết luận
Diễn giả kết thúc bằng câu nói truyền cảm hứng của Brené Brown:
> *"Vulnerability sounds like truth and feels like courage."*
> (Tạm dịch: Sự chân thành thừa nhận những điểm yếu của bản thân chính là một dạng của lòng can đảm).

{{% notice tip "Kết luận cốt lõi" %}}
Tự tin không có nghĩa là hoàn hảo hay không sợ hãi. Tự tin là **dám hành động** dù vẫn còn lo lắng. Mỗi lần dũng cảm bước ra khỏi vùng an toàn là một viên gạch xây dựng nên sự tự tin bền vững. Đừng chờ đến khi đủ tự tin mới hành động, hãy hành động để trở nên tự tin hơn!
{{% /notice %}}

---

### Phần 4: Tự Làm Web Tử Vi Đại Việt – Xây dựng sản phẩm AI từ ý tưởng đến vận hành thực tế (Diễn giả: Nghĩa Trần)

#### 1. Ý tưởng sản phẩm & Cơ hội thị trường
Khác với các bài trình bày thuần kỹ thuật AWS khác, chia sẻ của anh Nghĩa Trần mang đến câu chuyện thực chiến của một Startup công nghệ: xây dựng và vận hành trang web tử vi bằng AI [Tử Vi Đại Việt](https://tuvidaiviet.com/).
* **Nhu cầu thị trường:**
  * Giới trẻ có tính tò mò cao về tử vi, muốn thấu hiểu bản ngã bên trong bản thân nhưng gặp rào cản từ các phương thức truyền thống.
  * *Chi phí tư vấn cao:* Các buổi tư vấn tử vi chất lượng truyền thống dao động từ 500.000 VNĐ đến vài triệu đồng.
  * *Nội dung khó tiếp cận:* Tài liệu cổ điển dùng nhiều thuật ngữ Hán Việt phức tạp khiến người trẻ khó tiếp thu.
* **Mục tiêu của Tử Vi Đại Việt:** Số hóa lĩnh vực tử vi giúp giải mã vận mệnh cá nhân (tính cách, tình cảm, công việc) một cách nhanh chóng, trực quan và dễ tiếp cận cho thế hệ trẻ với thông điệp: *"Giải mã vận mệnh – Thấu hiểu bản ngã bên trong bạn"*.

#### 2. Mô hình kết hợp độc đáo: AI + Chuyên gia kiểm duyệt
Đây là điểm sáng về tư duy phát triển sản phẩm của dự án:
> [!NOTE]
> Nhóm phát triển **không chọn cách** gửi prompt thô vào ChatGPT rồi trả kết quả ngay cho người dùng.

Thay vào đó, quy trình kiểm soát chất lượng đầu ra được thực hiện chặt chẽ:
* **Thử nghiệm và đánh giá nhiều mô hình AI:** Nhóm liên tục thử nghiệm các Foundation Models khác nhau để so sánh chất lượng phản hồi, độ chính xác và tính tự nhiên của ngôn ngữ nhằm chọn ra model phù hợp nhất cho từng tác vụ (tương tự phương pháp so sánh mô hình trên Amazon Bedrock).
* **Chuyên gia kiểm duyệt nội dung (Human-in-the-loop):** Sau khi AI tạo sinh ra nội dung lá số, các chuyên gia tử vi thực tế sẽ thẩm định, điều chỉnh thuật ngữ và đảm bảo các diễn giải có giá trị thực tế cao nhất trước khi hiển thị cho người dùng.

#### 3. Mô hình kinh doanh (Business Model)
Dự án áp dụng mô hình kinh doanh **Freemium** để vừa thu hút lượng truy cập vừa tạo dòng tiền bền vững:
* **Gói miễn phí:** Người dùng có thể tạo lá số cá nhân và xem các thông tin phân tích cơ bản.
* **Gói trả phí (Premium):** Mở khóa các báo cáo chuyên sâu, phân tích chi tiết vận hạn theo từng giai đoạn, so sánh các khía cạnh trong lá số và nhận dự đoán cá nhân hóa ở mức độ cao.

#### 4. Hành trình vận hành và Chuyển đổi hạ tầng lên AWS
Khi số lượng người dùng tăng trưởng mạnh mẽ, bài toán của startup dịch chuyển từ việc *"AI trả lời thế nào"* sang *"Làm sao để hệ thống chạy ổn định"*. Nhóm đã phải tối ưu hóa hàng loạt khâu vận hành thực tế như hosting, database, tích hợp cổng thanh toán, email tự động, monitoring, tác vụ chạy ngầm (background jobs) và quy trình CI/CD.

Để nâng cao khả năng mở rộng (scalability) và độ tin cậy, dự án đã lên lộ trình chuyển đổi hạ tầng sang AWS:
* **Hạ tầng ban đầu:** Next.js, TypeScript, React, MySQL và Prisma ORM.
* **Kiến trúc AWS mục tiêu:**
  * **Amazon ECS (Elastic Container Service):** Quản lý và vận hành các container ứng dụng một cách linh hoạt.
  * **Amazon RDS:** Cơ sở dữ liệu quan hệ được quản lý tự động, đảm bảo hiệu năng và độ ổn định cao.
  * **Amazon Bedrock:** Làm nền tảng quản lý và kết nối linh hoạt tới các mô hình AI tạo sinh.
  * **AWS Lambda & Amazon ElastiCache:** Tối ưu hóa xử lý bất đồng bộ và bộ nhớ đệm (caching) để giảm tải cho database.
  * **Amazon CloudWatch:** Giám sát hiệu năng và phát hiện lỗi hệ thống thời gian thực.

{{% notice info "Bài học cốt lõi" %}}
* **AI chỉ là một phần nhỏ:** Một startup AI thành công không chỉ cần mô hình AI tốt mà đòi hỏi trải nghiệm người dùng tối ưu, hệ thống vận hành ổn định và quy trình kiểm duyệt chất lượng nghiêm ngặt.
* **Luôn kiểm chứng đầu ra của AI:** Việc đánh giá nhiều model và kiểm duyệt bởi chuyên gia giúp tránh lỗi ảo giác (hallucination) của AI.
* **Tư duy sản phẩm thực tế:** Sự khác biệt lớn nhất giữa **làm demo AI** và **vận hành sản phẩm AI thực tế** là việc giải quyết đồng thời các bài toán về công nghệ, hạ tầng, chi phí, mô hình kinh doanh và trải nghiệm người dùng thực tiễn.
{{% /notice %}}

---

### Phần 5: The Hidden Iceberg of a Project: DevOps Before Disaster (Diễn giả: Trần Minh Quân)

#### 1. Tổng quan bài chia sẻ & Hình ảnh tảng băng trôi (Iceberg Analogy)
Bài trình bày sử dụng hình ảnh ẩn dụ tảng băng trôi (Iceberg) để mô tả một thực tế quen thuộc nhưng đau lòng trong phát triển phần mềm: Những vấn đề chúng ta nhìn thấy trong dự án thường chỉ là phần nổi của tảng băng.
* **Triệu chứng nhìn thấy:** Lỗi triển khai, deadline trễ, khách hàng phàn nàn, nhân sự kiệt sức... thường không phải là nguyên nhân gốc rễ, mà chỉ là kết quả của những lỗ hổng tích tụ từ rất sớm trong quy trình phát triển.
* **Thông điệp chính:** DevOps không đơn thuần là công cụ triển khai hay hệ thống CI/CD, mà là một giải pháp toàn diện về cả văn hóa lẫn kỹ thuật giúp phát hiện và giải quyết các vấn đề trước khi chúng biến thành thảm họa.

#### 2. Chi phí ẩn của một tính năng "đơn giản" (The Hidden Cost of a "Simple Feature")
Diễn giả mở đầu bằng một tình huống kinh điển: Một tính năng ban đầu được đánh giá cực kỳ đơn giản, chỉ mất khoảng 2 ngày phát triển. Tuy nhiên thực tế diễn ra:
* Yêu cầu liên tục thay đổi giữa chừng.
* Hiểu nhầm nghiêm trọng giữa các bên liên quan (Khách hàng, Product, Dev).
* Phải chỉnh sửa và làm đi làm lại nhiều lần, kéo dài thời gian hơn dự kiến rất nhiều.
* Kết quả là cả dự án rơi vào trạng thái mệt mỏi và kiệt sức vì một tính năng tưởng chừng rất nhỏ.

> [!IMPORTANT]
> **Bài học rút ra:** Một tính năng "đơn giản" thường không hề đơn giản. Chi phí lớn nhất của dự án không nằm ở việc viết code (coding) mà nằm ở khâu giao tiếp, làm rõ yêu cầu, kiểm thử và sự phối hợp giữa các bộ phận.

#### 3. Cấu trúc tảng băng dự án: Phần nổi và Phần chìm

| Phần của tảng băng | Các dấu hiệu / Nguyên nhân gốc rễ | Chi tiết ảnh hưởng |
| :--- | :--- | :--- |
| **Phần nổi (Visible Symptoms)** | • Trễ deadline<br>• Lỗi nghiêm trọng trên Production<br>• Triển khai (deployment) thất bại<br>• Khách hàng phàn nàn<br>• Nhân sự kiệt sức (burnout) | Các đội ngũ thường chỉ tập trung giải quyết hậu quả tức thời như sửa bug, tăng ca, đẩy nhanh tiến độ mà bỏ qua gốc rễ vấn đề. |
| **Phần chìm (Hidden Causes)** | • **Requirement Ambiguity** (Yêu cầu mơ hồ)<br>• **Communication Gaps** (Lỗ hổng giao tiếp)<br>• **Siloed Teams** (Nhóm làm việc cô lập)<br>• **Lack of Ownership** (Thiếu tinh thần trách nhiệm)<br>• **Manual Processes** (Quy trình thủ công)<br>• **Slow Feedback Loops** (Vòng phản hồi chậm) | • Khách hàng, Product và Developer hiểu sai lệch yêu cầu.<br>• Thông tin bị đứt gãy giữa các bộ phận Dev, QA, Ops.<br>• Các nhóm đổ lỗi lẫn nhau khi xảy ra sự cố.<br>• Deploy và kiểm thử thủ công gây sai sót, phát hiện lỗi quá muộn khiến chi phí sửa chữa tăng vọt. |

#### 4. Vai trò thực sự của DevOps
Để ngăn chặn thảm họa (Disaster) cho dự án, DevOps đóng vai trò là chiếc phao cứu sinh thông qua 3 trụ cột:
* **Văn hóa cộng tác (Collaboration Culture):** Phá bỏ bức tường ngăn cách giữa các nhóm Dev, QA, Operations và Product, thúc đẩy tinh thần trách nhiệm chung đối với sản phẩm cuối cùng.
* **Tự động hóa (Automation):** Giảm thiểu tối đa sai sót từ con người và tiết kiệm thời gian thông qua việc tự động hóa quy trình tích hợp/triển khai (CI/CD), kiểm thử tự động (Automated Testing), và quản lý hạ tầng bằng mã nguồn (Infrastructure as Code - IaC).
* **Vòng phản hồi nhanh (Fast Feedback Loops):** Tích hợp các hệ thống giám sát (Monitoring), ghi log (Logging) và cảnh báo tự động (Alerting) giúp phát hiện sự cố sớm và phản hồi liên tục.

{{% notice info "Kết luận cốt lõi" %}}
Khi dự án xuất hiện lỗi, trễ deadline hay khách hàng phàn nàn, đó chỉ là triệu chứng lâm sàng chứ không phải căn bệnh thực sự. Để cải thiện dự án tận gốc, cần:
1. Làm rõ và chuẩn hóa yêu cầu ngay từ ban đầu.
2. Tăng cường giao tiếp đa chiều giữa các team.
3. Xây dựng văn hóa chịu trách nhiệm chung đối với sản phẩm.
4. Tự động hóa quy trình để rút ngắn thời gian phản hồi.
{{% /notice %}}
