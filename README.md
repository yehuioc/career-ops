---
producer: codex
producer_role: controller
producer_evidence: 用户2026-10-02明确授权公开项目源码并先清理私人内容
review_owner: codex-controller
review_state: reviewed
canonical_status: reference
---

# 求职执行系统

本机求职工具，将公开岗位、已经审核的候选人事实、材料版本、人工批准和后续反馈放进同一状态链。它没有自动发送、海投或账号 Cookie 功能。

这是经过隐私清理的公开源码起点，不包含候选人简历、真实岗位样本、联系方式、投递包、私人验收报告或原工作区历史。原系统完成过本地业务链路验收；源码公开不代表发生了真实投递。

## 安装与运行

需要 Python 3.10+；主要采集与状态使用标准库，生成 PDF 另需 ReportLab，测试另需 pytest。运行 `python -m career_ops --help` 查看命令，`python -m career_ops status` 查看本机状态。

国内来源为 NCSS，海外来源为 Greenhouse 公共 Job Board API。平台页面存在与雇主独立确认分开，超时、关闭、未知和核验失败分别保存。使用这些接口须遵守提供方规则。

## 配置个人事实

公开默认事实文件为 `private/career-profile.json`。用 `--candidate` 指定已经审核的 `career-profile-v1` 文件；原简历与各项事实证据都必须保留真实文件、定位和 SHA-256，具体校验见 `career_ops/candidate.py`。不要把自己没有的经历填成事实。

默认只允许项目目录中的本地证据；确需另一个已授权资料目录时，设置 `CAREER_WORKSPACE_ROOT` 为自己的明确工作目录。真实事实文件、SQLite、材料与联系记录全部在忽略的 `private/`。

## 业务链路

`discover` 采集岗位，`verify` 核验来源，`assess` 对照事实并保留未知，`prepare` 生成审阅包，`report` 打开审阅入口。`shortlist` 是选择；`approve` 是绑定当前材料摘要的批准；`mark-applied` 仍需真实发送证据。生成文件或批准不等于已经投递。

`record-reply`、`funnel`、`next-actions` 留存后续反馈。私人资料和历史不上传，本仓库也不提供可直接冒用的求职事实。

## 验证与许可

运行 `python -m pytest -q`。合成测试保存在 `private/test-tmp/`。当前仅公开源码，未新增开源许可证；第三方依赖遵守各自许可。
