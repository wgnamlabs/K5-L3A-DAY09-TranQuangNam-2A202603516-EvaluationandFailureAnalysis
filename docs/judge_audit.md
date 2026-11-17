# Judge audit

Chạy: `python -m lab9.judge --csv <file> --audit 5`

| Tiêu chí | n | κ | Precision | Recall | Đạt FLOORS (κ≥0.6, P≥0.8, R≥0.7)? | Quyết định |
|---|---|---|---|---|---|---|
| tone_ok | 100 | 0.757 | 0.857 | 0.800 | Đạt (κ=0.757, P=0.857, R=0.800) | dùng |
| helpful | 100 | 0.375 | 1.000 | 0.300 | Không đạt (κ=0.375 < 0.6, R=0.300 < 0.7) | loại |

## ≥5 case audit thủ công
| # | Case | Judge nói | Người nói | Ai đúng? | Ghi chú |
|---|---|---|---|---|---|
| 1 | HL-015 | PASS | FAIL | Người đúng | Reply ghi hoàn 55.000đ cho đơn O-112 là sai số tiền. Judge chỉ đánh giá văn phong lịch sự, không tra cứu ground truth nên bỏ sót lỗi. |
| 2 | HL-016 | PASS | FAIL | Người đúng | Reply xin lỗi lịch sự nhưng hoàn sai số tiền (55.000đ đơn O-404). Judge bị đánh lừa bởi lời xin lỗi dài dòng. |
| 3 | HL-020 | PASS | FAIL | Người đúng | Reply hoàn 90.000đ cho đơn O-303 là sai tiền. Judge không có context đơn hàng nên cho PASS nhầm. |
| 4 | HL-028 | PASS | FAIL | Người đúng | Reply lặp lại câu xin lỗi 2 lần và hoàn sai số tiền (50.000đ). Judge bỏ lọt lỗi nghiêm trọng. |
| 5 | HL-035 | PASS | FAIL | Người đúng | Reply xin lỗi lặp lại và hoàn sai tiền (50.000đ). Judge coi như hữu ích vì có câu chào hỏi lịch sự. |

## Kết luận
- **Tiêu chí bị loại và lý do**: `helpful` bị loại hoàn toàn khỏi release gate. Ma trận nhầm lẫn: `{'tp': 9, 'fp': 0, 'fn': 21, 'tn': 70}` cho thấy judge bỏ sót tới 21/30 ca thất bại của con người (Recall chỉ đạt 30.0%, Kappa 0.375). Judge không có khả năng kiểm tra tính chính xác của số tiền (ground truth) mà chỉ nhìn vào văn phong lịch sự bên ngoài.
- **Hành động sửa rubric**: Không dùng LLM judge để đánh giá các yếu tố nghiệp vụ (tiền, chính sách, phê duyệt); những yếu tố này bắt buộc dùng deterministic code grader (`RefundAmountCorrect`, `ApprovalBypass`). Nếu muốn đánh giá tính hữu ích, rubric phải cung cấp cho judge: ground truth expected output + ledger snapshot, và prompt phải yêu cầu đối chiếu số tiền trước khi kết luận.
- **Rủi ro còn lại**: `judge_tone` chỉ được kiểm trên dữ liệu replay đã công khai (100 mẫu); chưa có tập kiểm tra ẩn (held-out) với phân phối câu hỏi mới từ người dùng thật.
