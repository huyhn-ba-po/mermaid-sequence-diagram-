---
name: mermaid-sequence-diagram
description: |
  Hiện thực kỹ thuật Sequence Diagrams (BABOK v3 §10.42): sinh sequence diagram
  Mermaid (.mermaid) từ mô tả một usage scenario / luồng chức năng, thể hiện các
  message trao đổi giữa các object theo thứ tự thực thi, xuất file import được
  vào draw.io. Dùng khi user nói "vẽ sequence diagram", "vẽ Mermaid", "tạo
  sequence", "vẽ luồng", "diagram cho chức năng", "sequence cho feature", "sơ đồ
  tuần tự", "sơ đồ tương tác", "mermaid sequence", kể cả nói tắt "vẽ luồng đi".
  KHÔNG dùng cho: flowchart, class diagram, ERD, state diagram, gantt chart, hay
  activity diagram / process model (dùng activity-diagram-builder).
---

# Sequence Diagram Builder

Skill này hiện thực kỹ thuật **Sequence Diagrams** trong BABOK v3 (§10.42). Mọi
quy ước về thành phần, ký pháp và phạm vi áp dụng đều bám theo định nghĩa của kỹ
thuật này. Thuật ngữ kỹ thuật giữ nguyên tiếng Anh theo BABOK.

## Purpose (Mục đích)

> *"Sequence diagrams are used to model the logic of usage scenarios by showing
> the information passed between objects in the system through the execution of
> the scenario."* — BABOK v3, §10.42.1

Skill sinh một sequence diagram (định dạng Mermaid) từ mô tả của **một usage
scenario** — thường được refine từ một use case — để thể hiện logic của scenario
đó qua các message trao đổi giữa các object.

## Description (Mô tả)

Theo BABOK §10.42.2:

- Sequence diagram thể hiện cách các **process/object tương tác** trong một
  scenario; nó cho thấy **tương tác** (interaction), KHÔNG thể hiện quan hệ
  (relationship) giữa các object.
- Các object được vẽ là **box** xếp ngang ở đỉnh trang, từ **trái sang phải**;
  mỗi object có một **lifeline** là đường dọc kéo xuống dưới.
- Các **message** giữa các object là **mũi tên ngang**; thứ tự message đọc theo
  **top-down, trái → phải**, bắt đầu từ message đầu tiên ở góc trên bên trái.
- Ký pháp chuẩn theo **UML**. Sequence diagram đôi khi gọi là *event diagram*.
- Thường dùng để thể hiện cách các thành phần giao diện hoặc thành phần phần mềm
  tương tác; được refine từ **Use Cases and Scenarios**.

## Elements (Các thành phần)

Ánh xạ các element của BABOK §10.42.3 sang ký pháp Mermaid:

| BABOK element | Ý nghĩa (BABOK) | Mermaid |
|---|---|---|
| **Object / Participant** | Đối tượng gửi/nhận message; vẽ là box ở đỉnh | `participant A as <Tên>` |
| **Lifeline** | Vòng đời của object trong scenario (đường dọc) | tự sinh từ participant |
| **Activation box** (execution specification) | Khoảng thời gian một operation được thực thi | `activate A` / `deactivate A` (tuỳ chọn) |
| **Message — Synchronous Call** | Chuyển control sang object nhận; **sender chờ** response mới tiếp tục | `A->>B: <message>` |
| **Message — Asynchronous Call** (signal) | Gửi xong, object **tiếp tục xử lý** mà không chờ | `A-)B: <message>` |
| **Response / Return** | Dữ liệu trả về cho sender | `B-->>A: <message>` |
| **Self-message** | Object tự xử lý nội bộ | `A->>A: <message>` |
| **Destruction** | Kết thúc lifeline của object | `destroy A` (X) |

Phân chia giai đoạn (không thuộc UML chuẩn nhưng hỗ trợ readability): dùng
`Note over A, Z: TÊN GIAI ĐOẠN`.

## Quy trình tạo

Sinh diagram bằng cách điền lần lượt các element ở trên:

1. **Xác định scenario** — đọc use case / mô tả luồng, chọn **một** usage
   scenario (happy path). Đặt tên file theo scenario.
2. **Liệt kê object/participant** — các đối tượng trao đổi message. Sắp xếp
   trái → phải theo thứ tự tương tác: actor (người dùng) → giao diện → service →
   data store → hệ thống ngoài.
3. **Sinh message theo thứ tự thực thi** — đánh số liên tục; phân biệt rõ:
   - synchronous call (`->>`) khi sender chờ kết quả,
   - response/return (`-->>`) cho dữ liệu trả về,
   - asynchronous (`-)`) cho signal gửi-rồi-quên (vd: gửi notification),
   - self-message (`A->>A`) cho xử lý nội bộ.
4. **Phân giai đoạn** (nếu scenario dài) — chèn `Note over` để chia phase.
5. **Lắp ráp & verify** — ghép thành file `.mermaid` bắt đầu bằng
   `sequenceDiagram`, indent 2 spaces; kiểm tra ký pháp UML hợp lệ, numbering
   liên tục, mỗi request có response tương ứng (trừ asynchronous).

Output: file `.mermaid` tại `/mnt/user-data/outputs/<scenario>-sequence.mermaid`,
import được vào draw.io (File → Import).

## Usage Considerations

Theo BABOK §10.42.4 — dùng để quyết định khi nào nên / không nên dùng kỹ thuật:

### Strengths
- Thể hiện tương tác giữa các object theo **thứ tự thời gian** (chronological).
- Trực quan → cho phép stakeholder **validate logic** tương đối dễ.
- Một use case có thể được refine thành một hoặc nhiều sequence diagram để bổ
  sung chi tiết và hiểu sâu hơn về một business process.

### Limitations
- Tốn thời gian/công sức nếu tạo **đầy đủ** sequence diagram cho mọi use case —
  thường không cần thiết. → Chỉ vẽ cho scenario quan trọng/phức tạp.
- Trước nay dùng để mô hình hoá system flow nên có thể bị xem là **quá kỹ
  thuật** ngoài ngữ cảnh IT. → Giữ nội dung message ở mức nghiệp vụ, tránh chi
  tiết kỹ thuật low-level.

## Ràng buộc khi sinh diagram

- Chỉ vẽ **happy path** của scenario — không vẽ alt/else cho case lỗi (giữ
  diagram đúng vai trò "model the logic of a usage scenario", không phải tài
  liệu xử lý lỗi).
- Mỗi message ≤ 60 ký tự để không vỡ layout khi render / import draw.io.
- Mỗi synchronous call phải có response tương ứng; asynchronous signal thì không
  bắt buộc.
- Đánh số message liên tục, không reset giữa các giai đoạn.
