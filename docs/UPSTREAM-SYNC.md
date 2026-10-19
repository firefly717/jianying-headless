# 上游同步记录

记录 `develop` 与上游 `mcncarl/jianying-headless` 的同步状态。更新日期：2026-10-19。

## 当前状态

- 上游 `main`（`19d9bc7`）已完整包含在 `develop` 中。
- 本仓库 `main`（`bbc46d3`）落后于上游，未改动。
- `develop` 由上游 `main`、本地修复 `04273a6` 和上游 `feat/openverse-sfx` 合并（`7d59719`）组成。

## 上游分支处理

| 上游分支 | 状态 | 说明 |
| --- | --- | --- |
| `main` | 已包含 | 无需处理 |
| `feat/jianying-1160-profile` | 已在上游 `main` | 无需处理 |
| `fix/windows-ffmpeg-ci-source` | 已在上游 `main` | 无需处理 |
| `feat/openverse-sfx` | 已合并到 `develop`（`7d59719`） | 无冲突，新增 13 个测试通过 |
| `followup/issues-2-3-5` | **未合并，待决定** | 见下 |

## 待决定：`followup/issues-2-3-5`

上游提交 `ee0b0d5 docs: clarify official sources and cover dual xattr rejection`。

**文本冲突**：`docs/ISSUE-REMEDIATION.md`、`docs/VERIFICATION.md` 与 `develop` 的同一段落冲突。

**语义冲突（需要决定）**：

- 上游测试 `test_provenance_addition_remains_blocked_without_macl` 要求：仅新增 `com.apple.provenance` 时仍须拒绝。
- 本仓库提交 `04273a6` 放宽了这一条：新增 provenance 会记录并容忍，消失仍拒绝。对应测试为 `tests/test_issue_hardening.py` 中的 `test_os_added_provenance_is_recorded_and_tolerated`。
- 两者不能同时成立。合并前需二选一：
  - **A. 保留放宽**：改写上游的这条测试，并在文档中说明与上游的分歧。
  - **B. 采用上游拦截**：撤销 `04273a6` 中的 provenance 放宽。此时首页发布会在 provenance 新增时再次失败。

上游的另一条测试 `test_dual_attribute_addition_is_not_accepted`（`macl` 与 `provenance` 同时新增）在两种选择下都会拒绝，不冲突。

复现冲突的命令（不改动工作区）：

```bash
git merge-tree --write-tree develop origin/followup/issues-2-3-5
```
