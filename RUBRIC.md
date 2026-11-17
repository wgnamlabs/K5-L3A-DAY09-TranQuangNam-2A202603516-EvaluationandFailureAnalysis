# Rubric (100 điểm = Core 60 · Advanced 25 · Expert 15)

Hạn nộp: **23:59 cùng ngày** (commit cuối trên PR trước 23:59 mới được chấm). Core nên xong trên lớp; Advanced/Expert làm tiếp sau 17:00.

## Core — 60 (giảng viên chấm theo checklist + test công khai)
| Hạng mục | Điểm | Đạt đủ khi |
|---|---|---|
| M1 Taxonomy + eval set | 12 | ≥5 FM có bằng chứng trace; 5 anchor khớp key; provenance đủ; `--check` sạch |
| M2 Calibration | 12 | test stats xanh; kết luận đúng tone=dùng được, helpful=không; audit ≥5 bất đồng có hành động sửa rubric |
| M3 Trajectory | 12 | 2 grader đúng; ≥80% phát hiện, ≤10% báo nhầm; metric FM-08 hoạt động |
| M4 Bootstrap + gate | 15 | CI lệch ≤0.01; gate chặn `seeded_regression`, cho qua `fixed`; job `eval-gate` xanh; `eval` trong harness_map |
| Eval card + PRD v1.1 | 9 | số liệu khớp report có run_id; nêu giới hạn (MockLLM, 14 item, judge replay) |

## Advanced — 25 (bộ chấm ẩn `grade_hidden.py`, chạy trên repo của nhóm)
| Kiểm tra | Điểm |
|---|---|
| M6 `pass_at_k` / `pass_hat_k` đúng trên vector ẩn; từ chối (n, c, k) sai | 5 |
| M5 `fm_map.yaml` đúng mã FM; `evalset --v2` sạch (≥3 item/FM bị tấn công) | 6 |
| Spec v2: `pass_hat_5` blocking; slice `money_movement` + `adversarial` blocking | 4 |
| Gate chặn `slice_regression` và một slice regression **ẩn** | 6 |
| Gate chặn một lỗi FM-08 **ẩn** (reply nói sai số tiền) | 2 |
| Không báo nhầm trên `fixed`, `honest_refactor` và một refactor trung thực **ẩn** | 2 |

## Expert — 15 (bộ chấm ẩn)
| Kiểm tra | Điểm |
|---|---|
| M7 gate chặn mọi trap: 4 trap công khai + 2 trap ẩn | 9 |
| Chặn `flaky_regression` và một variant ẩn không để lại sự kiện override; baseline so với chính nó vẫn merge; A/A Δ = 0; sàn `pass_hat_5` ≥ 0.8 | 6 (nhân với tỉ lệ trap bị chặn) |

6 điểm cuối nhân với tỉ lệ trap bị chặn: các kiểm tra đó gate Core đã qua sẵn, nên chỉ tính khi `integrity_check` thực sự hoạt động.

## Trừ điểm / bonus
Trừ: sửa `lab8/` (−10), sửa tay `reports/` (−20), nới ngưỡng trên cùng eval-set version (−10), nhãn inline (−5), sửa variant/trap/bộ chấm để "qua" thay vì sửa gate (−15).
Bonus +5 (tối đa 100): port grader sang Inspect AI/DeepEval và so khớp kết quả.
