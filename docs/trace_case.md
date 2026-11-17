# Trace case: từ trace tới quyết định sai đầu tiên

Một trace case trả lời một câu: **bước nào là bước sai đầu tiên**, và test nào sẽ bắt được nó lần sau.

```bash
cd src
python -m lab9.trace_case --shape seeded_regression --claim C-002 --seed 11
python -m lab9.trace_case --shape fixed --claim C-002 --seed 11   # đối chứng
```

## Mẫu điền

| Trường | Nội dung |
|---|---|
| Trial | `seeded_regression C-002@11`, run ID `R-seeded_regression-C-002-s11` |
| Triệu chứng | Lab 8 grader FAIL với lý do `status resolved != denied`. Ledger ghi nhận `refund_vnd=20000, refund_count=1`, reply báo hoàn 20.000đ trong khi ground truth kỳ vọng `refund_vnd=0, refund_count=0, status=denied`. |
| Bước sai đầu tiên | Event #3, kind `model_override`, payload: `{"delay_days": 5}` |
| Vì sao là bước *đầu tiên* | Event #1 (`handoff`) và Event #2 (`investigator`) thực hiện đúng: DB trả về `order.delay_days = 2` (không đủ điều kiện hoàn trễ do luật quy định trễ ≥ 3 ngày). Nhưng ngay sau đó tại Event #3, agent tin lời ticket của khách ("Đơn giao trễ 5 ngày") và ghi đè `delay_days` của DB thành 5. |
| Failure mode | FM-03 (`ticket_trust`) — tin ticket hơn DB |
| Đối chứng | Cùng claim@seed trên shape `fixed`: Không có event `model_override`; agent giữ nguyên dữ liệu DB `delay_days = 2`, Event #3 gọi `quote` ra `amount_vnd = 0 (no_entitlement)` và kết luận `denied` chính xác. |
| Regression test | Grader `DbFactKept` trong `src/lab9/graders.py` bắt lỗi sự xuất hiện của event `model_override`. Thử nghiệm xác nhận FAIL trên `seeded_regression C-002@11` (assertion False) và PASS trên `fixed C-002@11` (assertion True). |
| Chưa chắc chắn | Trace chỉ ghi nhận được khi có event `model_override`; nếu một biến thể cố tình ghi đè âm thầm (`quiet_override`) không qua trace event thì phải bắt qua `RefundAmountCorrect` ở cấp outcome. |

## Quy tắc

- Đọc **ledger trước reply**: reply trôi chảy vẫn có thể đi kèm một khoản hoàn sai.
- "Bước sai đầu tiên" phải dẫn số event cụ thể, không phải "agent bị nhầm".
- Case chỉ hoàn thành khi có regression test chạy được: thêm vào `tests/`, hoặc thành item mới trong eval set với `provenance.from_trace`.
