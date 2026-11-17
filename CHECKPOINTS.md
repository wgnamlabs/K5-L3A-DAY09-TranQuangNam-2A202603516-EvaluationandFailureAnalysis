# Checkpoints (giảng viên đi bàn đối chiếu)

**M0 — Môi trường.** `python -m lab9.smoke` in `M0 PASS`. `python -m lab9.evalset --build` tạo 14 item cùng tệp nhãn riêng và trả exit 0 khi `failure_modes` còn rỗng.

**M1 — Taxonomy.** `docs/failure_taxonomy.md` có ≥5 FM, mỗi FM: định nghĩa 1 câu, layer (model/prompt · tool/schema · retrieval · orchestration/state · guardrail/approval), ≥1 trace/claim làm bằng chứng, grader dự kiến (code / judge / human). 5 anchor bắt buộc: giảng viên so với key riêng (`instructor/coding_sheet_anchor_key.csv`, chỉ có ở repo giảng viên), chấm theo mã FM và bước sai đầu tiên. `evalset --check` = 0 problem.

**M2 — Judge-the-judge.** `stats.py` qua test; `judge_tone` PASS floors (κ≈0.757, P≈0.857, R=0.8); `judge_helpful` **FAIL** — nhóm phải giải thích vì sao không được dùng judge này làm gate. Bảng audit ≥5 bất đồng: ai đúng, sửa rubric ra sao.

**M3 — Trajectory.** 2 grader TODO qua test. Trên variants: phát hiện ≥80% trace lỗi, báo nhầm ≤10% trên `fixed`. Thêm 1 metric trong `metric_spec.yaml` bắt FM-08 (reply nói số tiền khác ledger).

**M4 — Gate.**
```bash
python -m lab9.eval --shape swarm --out reports/baseline_v1.json
python -m lab9.eval --shape fixed && python -m lab9.gate --pr reports/eval_fixed.json --baseline reports/baseline_v1.json             # exit 0
python -m lab9.eval --shape seeded_regression && python -m lab9.gate --pr reports/eval_seeded_regression.json --baseline reports/baseline_v1.json  # exit 1
```
Tham chiếu: `pass_rate` Δ −0.271, CI [−0.500, −0.057] ⇒ BLOCK. `paired_bootstrap` lệch ≤0.01 so với tham chiếu.

Sau khi hoàn thành bài tập và commit baseline M4, workflow `eval-gate` chạy test học viên và kiểm tra eval set. Gate phải trả exit 0 cho `swarm` của PR và `fixed`. Với `seeded_regression`, gate phải trả exit 1 và ghi rõ `pass_rate` trong danh sách metric bị chặn. Cả ba dùng chung metric spec và baseline.

---
## Sau 17:00 (Advanced / Expert, hạn 23:59) — giảng viên chạy `instructor/grade_hidden.py`

**M5 — Eval set v2 + slice.** `evalset --v2 --build` sạch: mọi cơ chế trong pack đã map sang mã FM, mỗi FM bị tấn công ≥3 item. `slice_regression` có `pass_rate` trung bình vẫn trên sàn, CI chạm 0; chỉ slice `money_movement` chặn được nó.

**M6 — Nhiễu.** A/A (`--aa 66,77,88,99,110`) cho Δ = 0 trên MockLLM; nhóm giải thích vì sao với LLM thật dải nhiễu > 0 và sàn phải nằm ngoài dải đó. `pass_hat_5` blocking chặn `flaky_regression` (bug 1/5 seed) mà `pass_rate` trung bình không chặn ổn định.

**M7 — Chống gian lận eval.** Mỗi file trong `reports/traps/` khiến gate exit 1 với lý do `INTEGRITY` (hoặc ratchet với `--base-spec`). `lab9.mutation` = 3/3, 0 báo nhầm. Hỏi nhóm: trap ẩn nào có thể còn lọt (gợi ý: dòng trùng lặp, evaluator_failures trên run xanh).
