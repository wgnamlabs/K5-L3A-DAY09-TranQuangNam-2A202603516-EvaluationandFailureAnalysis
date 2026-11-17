# Nộp bài (hạn 23:59 cùng ngày)
PR từ `starter/d09` → `main` của repo nhóm. Chấm commit cuối trước 23:59; commit sau hạn không tính.

## Core (nên xong trước 17:00)
- [x] `src/lab9/{stats,graders,evalset}.py` đã hoàn thành TODO M1–M4
- [x] `src/data/eval/eval_set_v1.jsonl` có `failure_modes`, `labels_v1.json` tách riêng
- [x] `docs/failure_taxonomy.md`, `docs/trace_case.md` (1 case + regression test), `docs/judge_audit.md`, `docs/eval_card.md`, `docs/PRD_v1.1.md`
- [x] `src/reports/baseline_v1.json`, `eval_fixed.json`, `eval_seeded_regression.json` (sinh bằng script)
- [x] `harness_map.yaml` có section `eval` (mẫu: `src/harness_map.eval.yaml`)
- [x] Job `eval-gate` xanh; dán output gate exit 1 của `seeded_regression` vào mô tả PR

## Advanced / Expert (tới 23:59)
- [x] `config/fm_map.yaml`, `config/metric_spec_v2.yaml` (slices, `pass_hat_5`, metric FM-08), `stats.pass_at_k/pass_hat_k`, `gate.slice_check`, `gate.integrity_check`
- [x] `src/data/eval/eval_set_v2.jsonl` + `labels_v2.json` (sinh bằng `evalset --v2 --build`)
- [x] `src/reports/baseline_v2.json` (sinh bằng script) → job `eval-gate-v2` chạy và xanh
- [x] Mô tả PR: output `lab9.mutation` (scorecard + `--aa`) và 4 dòng `exit 1` của các trap
- [x] `docs/eval_card.md` thêm mục v2: ngưỡng chọn từ A/A, slice nào bắt lỗi nào, và giới hạn cỡ mẫu (bug 1/3 seed không phát hiện ổn định với 5 seed)
