# Sequence Diagram Builder

Skill cho AI agent hỗ trợ kỹ thuật **Sequence Diagrams** trong BABOK v3 (§10.42):
sinh sequence diagram Mermaid từ mô tả một *usage scenario*, thể hiện các message
trao đổi giữa các object theo thứ tự thực thi. Output là file `.mermaid` import
được vào draw.io.

Tài liệu viết bằng tiếng Việt, giữ nguyên thuật ngữ BABOK tiếng Anh (object,
lifeline, message, synchronous/asynchronous, scenario…).

## Purpose

Theo BABOK §10.42.1, sequence diagram dùng để **mô hình hoá logic của một usage
scenario** bằng cách thể hiện thông tin (message) trao đổi giữa các object khi
scenario được thực thi. Skill này tự động hoá việc đó: từ mô tả một luồng/scenario
(thường refine từ một use case) sinh ra diagram theo ký pháp UML.

## Cách dùng

```text
Bạn: Vẽ sequence diagram cho luồng đăng nhập bằng OTP
AI:  [refine scenario → liệt kê object → sinh message → file .mermaid]
```

Các cụm từ kích hoạt: "vẽ sequence diagram cho…", "tạo Mermaid sequence cho
luồng…", "vẽ luồng [tên]", "sequence cho feature [tên]", "sơ đồ tuần tự".

## Cấu trúc

```text
mermaid-sequence-diagram/
├── SKILL.md     ← kỹ thuật (Purpose / Description / Elements /
│                  Usage Considerations) + quy trình tạo
└── README.md
```

`SKILL.md` được nạp vào context của agent.

## Cài đặt

Skill agent-agnostic (hoạt động với Claude, Antigravity/Gemini, Codex CLI,
Cursor… — mọi tool hỗ trợ thư mục skill dạng `SKILL.md`). Clone repo về đúng
đường dẫn skill của tool, hoặc cho agent đọc `SKILL.md` như một context document:

```bash
git clone https://github.com/huyhn-ba-po/mermaid-sequence-diagram-.git
```

## Phạm vi (theo Usage Considerations của BABOK)

Sequence diagram thể hiện tương tác theo thứ tự thời gian giữa các object trong
**một** scenario — không phải để mô hình hoá toàn bộ quy trình nghiệp vụ
(dùng Process Modelling / activity-diagram-builder) hay quan hệ tĩnh giữa các
lớp (class diagram). BABOK cũng lưu ý: không cần vẽ cho mọi use case, và nên giữ
message ở mức nghiệp vụ để không quá kỹ thuật.
