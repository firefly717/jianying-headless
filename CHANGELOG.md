# 变更日志

本文件遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/1.1.0/)，版本号遵循[语义化版本](https://semver.org/lang/zh-CN/)。

## [未发布]

### 修复

- 原生 MP4 导出不再因 ffprobe 的元数据显示方式被误拒。原生写出器会在 `moov` 中写入与 `ftyp` 同名的 `major_brand` 等元数据，本机 ffprobe 9.0.2 会把它们合并显示为 `isom;isom`，旧校验因此报 "not a standard MP4 container"。现在主品牌直接从文件开头的 `ftyp` 盒子读取（`engine/native_export.py` 的 `container_brand`）。判定标准未放宽：主品牌仍须为 `isom`、`mp41` 或 `mp42`，`qt  ` 仍被拒绝；帧数、时长、音视频流等检查不变。已在 macOS 26.5.2、剪映 11.5.0 上用 2 秒样例验证导出通过。

### 变更

- 首页发布（`publish`）的扩展属性检查放宽，允许系统在写入 quarantine 时新增的 `com.apple.provenance`。这类新增会记入审计的 `os_managed_attribute_changes`，不会删除任何属性。以下情况仍然拒绝：provenance 消失；`com.apple.macl` 与 `com.apple.quarantine` 的出现、消失或变化；其他任何属性的变化。此前在 macOS 26.5 上因该属性新增而无法发布，现在可以完成首页登记。相关说明同步更新于 `docs/MACL-ADAPTER.md`、`docs/ISSUE-REMEDIATION.md` 与 `skills/yichen-jianying-edit/references/headless-macos.md`。
- `headless_draft.py` 中 `native_export.py`、`jy14_headless.py` 的源码哈希固定值随上述改动更新。

### 迁移

- 无需迁移步骤。已生成的构建目录不需要重建，`verify-build` 仍可通过。
