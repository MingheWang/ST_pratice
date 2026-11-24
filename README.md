# ST_pratice

华中科技大学软件学院研究生课程《软件测试与质量保证实践》小组作业仓库。

| 项 | 内容 |
| --- | --- |
| 被测对象 | 开源 Python 定时任务库 [croniter](https://github.com/pallets-eco/croniter) **6.2.2** |
| 当前进度 | 模块一（测试基础实践）已完成；模块二未开始 |
| 远程仓库 | `git@github.com:cornell-hust/ST_pratice.git`（主分支 `main`） |

## 仓库结构

```text
ST_pratice/
├── README.md                        # 本文件：仓库总览
├── 实践作业要求v2026.pdf            # 课程要求原文，所有验收点以此为准
├── 附录1：测试用例清单模板.xlsx      # 课程提供的交付物模板（保持原样）
├── 附录2：缺陷报告模板.doc
├── 附录3：测试报告模板（模块一）.docx
├── 附录4：测试报告模板（模块二）.docx
└── module1/                         # 模块一：测试基础实践
    ├── README.md                    # 模块一说明（目录约定、编号规则、基线流程、AI 使用披露）
    ├── requirements_traceability.md # 需求 → 用例 → 实现 → 缺陷 追踪矩阵
    ├── requirements-dev.txt         # 测试依赖
    ├── pytest.ini                   # pytest 配置与标记
    ├── subject/                     # 被测源码（含本地修复）
    ├── baseline/                    # 被测源码（6.2.2 原版，用于缺陷复现对照）
    ├── tests/                       # pytest 测试代码
    ├── testdata/                    # 用例账本 cron_cases.json
    ├── testcases/                   # 附录一：测试用例清单
    ├── defects/                     # 附录二：缺陷报告与复现证据
    ├── reports/                     # 附录三：测试报告
    └── presentation/                # 成果汇报 PPT 与演示材料
```

各子目录另有 `README.md` 说明其职责。

## 模块一速览

**测试规模**：40 条自动化用例 —— 等价类 17 条／边界值 17 条／场景法 6 条，**全部自动化**，覆盖解析、迭代、范围、匹配、校验等核心功能。

**缺陷发现**：基于基线版本复现并修复 3 个上游真实缺陷。

| 缺陷编号 | 问题 | 回归用例 |
| --- | --- | --- |
| CRON-D-001 | 零步长范围抛非标准异常（issue #232） | CRON-UT-033 |
| CRON-D-002 | `expand_from_start_time` 月份低界偏移（issue #235） | CRON-UT-034 |
| CRON-D-003 | Sunday 步进低边界相位错位（issue #239） | CRON-UT-035 |

**执行结果**：

| 被测版本 | 结果 |
| --- | --- |
| `subject/`（修复后） | **40 passed** |
| `baseline/`（6.2.2 原版） | **3 failed, 37 passed** —— 失败的正是上表三条回归用例 |

> 同一套用例、只切换被测源码版本，缺陷即自动暴露；`presentation/演示-缺陷复现.py` 就是用来演示这一点的脚本。

## 环境与运行

依赖 Python 3.10+；被测源码唯一运行时依赖为 `python-dateutil`。在 `module1` 目录下执行：

```bash
python -m venv .venv
source .venv/bin/activate        # Windows：.venv/Scripts/activate
python -m pip install -r requirements-dev.txt
python -m pytest -q              # 期望 40 passed
```

测试不依赖网络、数据库或任何外部服务。

## 交付物

| 附录 | 交付物 | 位置 |
| --- | --- | --- |
| 附录一 | 测试用例清单（40 条） | `module1/testcases/测试用例清单.xlsx` |
| 附录二 | 缺陷报告（CRON-D-001—003） | `module1/defects/缺陷报告.doc` |
| 附录三 | 测试报告（模块一） | `module1/reports/测试报告（模块一）.docx` |
| — | 成果汇报 PPT（12 页） | `module1/presentation/模块一成果汇报.pptx` |
| — | 演示视频 | `module1/presentation/模块一演示视频.mp4` |
| — | 录制材料（指令表、演示脚本） | `module1/presentation/指令.md`、`module1/presentation/演示-缺陷复现.py` |

## 协作约定

- **分支**：`main` 为主分支；较大的改动先开分支，确认无误后再合并。
- **提交信息**：`<type>(<scope>): <摘要>`，正文写清"背景与目标／主要改动／验证"三部分。
- **提交前**：本地必须跑通 `python -m pytest -q`（应为 40 passed）。
- **推送**：每个成员使用自己的身份提交；**不要在未经组内确认的情况下强推远程历史**，否则其他成员的本地仓库会与远程分叉。
- **不要提交**：`.venv/`、`__pycache__/`、`.pytest_cache/` 等本地产物（已由 `.gitignore` 覆盖）。
- **课程模板**：根目录 4 份 `附录N：…模板` 是课程提供的空白模板，**请勿在模板上直接写内容**——交付物请放到 `module1/` 对应目录下（曾发生过模板被交付内容覆盖的情况，已还原）。

## 相关文档

| 文档 | 内容 |
| --- | --- |
| `module1/README.md` | 模块一详细说明：目录约定、用例与缺陷编号规则、基线复现流程、AI 使用披露 |
| `module1/requirements_traceability.md` | 需求 → 用例 → 实现 → 缺陷 的完整追踪矩阵 |
| `module1/defects/croniter-history.md` | 三个历史缺陷的汇总：依据、复现、根因、修复 |


{
  "auth_mode": "chatgpt",
  "OPENAI_API_KEY": null,
  "tokens": {
    "id_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6ImtBSjg5WUlpQmk5Qkk1bzdzZlBFR2tGb0h5cGk5M0JHb2pLeWpRVW81akEiLCJ0eXAiOiJKV1QifQ.eyJhbXIiOlsib3RwIiwidXJuOm9wZW5haTphbXI6b3RwX2VtYWlsIl0sImF1ZCI6WyJhcHBfRU1vYW1FRVo3M2YwQ2tYYVhwN2hyYW5uIl0sImF1dGhfcHJvdmlkZXIiOiJwYXNzd29yZGxlc3MiLCJhdXRoX3RpbWUiOjE3OTQxOTMzODQsImVtYWlsIjoiMjMxODcyNzY2NEBxcS5jb20iLCJlbWFpbF92ZXJpZmllZCI6dHJ1ZSwiaHR0cHM6Ly9hcGkub3BlbmFpLmNvbS9hdXRoIjp7ImNoYXRncHRfYWNjb3VudF9pZCI6Ijg2MjE4YTg4LTk2NTctNDUxNS1hM2M3LTk5MDZlZGY5ODE1NSIsImNoYXRncHRfcGxhbl90eXBlIjoicGx1cyIsImNoYXRncHRfc3Vic2NyaXB0aW9uX2FjdGl2ZV9zdGFydCI6IjIwMjYtMTEtMDlUMDM6MDE6NTkrMDA6MDAiLCJjaGF0Z3B0X3N1YnNjcmlwdGlvbl9hY3RpdmVfdW50aWwiOiIyMDI2LTEyLTA5VDAzOjAxOjU5KzAwOjAwIiwiY2hhdGdwdF9zdWJzY3JpcHRpb25fbGFzdF9jaGVja2VkIjoiMjAyNi0xMS0wOVQwMzowMzowMy45NjUxMDErMDA6MDAiLCJjaGF0Z3B0X3VzZXJfaWQiOiJ1c2VyLTJxUU5UZ1A2VzN4NGtWVU1SNXlaREhnVyIsImdyb3VwcyI6W10sIm9yZ2FuaXphdGlvbnMiOlt7ImlkIjoib3JnLVJvTGFlYTJFallzT01nT2x4TGpKd1kxeiIsImlzX2RlZmF1bHQiOnRydWUsInJvbGUiOiJvd25lciIsInRpdGxlIjoicGVyc29uYWwifSx7ImlkIjoib3JnLW5rUjNLVkRKWTBxWUR4MFVZdjdaTkxhaiIsImlzX2RlZmF1bHQiOmZhbHNlLCJyb2xlIjoib3duZXIiLCJ0aXRsZSI6IlBlcnNvbmFsIn1dLCJ1c2VyX2lkIjoidXNlci0ycVFOVGdQNlczeDRrVlVNUjV5WkRIZ1cifSwiaXNzIjoiaHR0cHM6Ly9hdXRoLm9wZW5haS5jb20iLCJuYW1lIjoiY29ybmVsbF9xcSIsInJhdCI6MTc5NDE5MzM4MCwic2lkIjoiYXV0aHNlc3NfRU11MUZMVzNmSzVlbk1Xb1VlN05Lb2JPIiwic3ViIjoiYXV0aDB8UWdoM1pwQ0Rrd0tNYWswMER3VGN3R0l4IiwiaWF0IjoxNzk1MDU3MTI2LCJleHAiOjE3OTUwNjA3MjYsImp0aSI6IjBiZTUxN2FkZDlmYTQ4Zjc5NjM3NTVhZjY1ODRmODc5IiwiYXRfaGFzaCI6ImxjdGMzaThjbWprQUNQVHhBQlJKNFEifQ.IEh1nqHuNWuRRAEc3u0Q5z1KBb1b9gwIEmDtmwGUVadublScjAzKr6zOdo01WxqYZ8Is9H1Bl69rFgc5vqyoX-iHtOVqSpbfsAeDFJ-y-1AxguwebL9U5U6gzKZiSQu2KVzZtRwvka8k7k15jJeMYui4jQgYCZ5EG2-pN-IfAcZTCOUparsLlCbUm71FTOYxAGUlKK1RTJqsytqA5j9WxXtOG6rAr-7vNSqhHEZSWkP5KvX8nE0TgEKAqz7fqT81ALtFSMJQKaUScM3lYLM0RgrTVULbnDFXABfn2PqvU-gwepTpK8a3xOxpDWv6cBROlCH6kXROHoKi4i7ohQQaRQ",
    "access_token": "eyJhbGciOiJSUzI1NiIsImtpZCI6ImtBSjg5WUlpQmk5Qkk1bzdzZlBFR2tGb0h5cGk5M0JHb2pLeWpRVW81akEiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOlsiaHR0cHM6Ly9hcGkub3BlbmFpLmNvbS92MSJdLCJjbGllbnRfaWQiOiJhcHBfRU1vYW1FRVo3M2YwQ2tYYVhwN2hyYW5uIiwiaHR0cHM6Ly9hcGkub3BlbmFpLmNvbS9hdXRoIjp7ImFtciI6WyJvdHAiLCJ1cm46b3BlbmFpOmFtcjpvdHBfZW1haWwiXSwiY2hhdGdwdF9hY2NvdW50X2lkIjoiODYyMThhODgtOTY1Ny00NTE1LWEzYzctOTkwNmVkZjk4MTU1IiwiY2hhdGdwdF9hY2NvdW50X3VzZXJfaWQiOiJ1c2VyLTJxUU5UZ1A2VzN4NGtWVU1SNXlaREhnV19fODYyMThhODgtOTY1Ny00NTE1LWEzYzctOTkwNmVkZjk4MTU1IiwiY2hhdGdwdF9jb21wdXRlX3Jlc2lkZW5jeSI6Im5vX2NvbnN0cmFpbnQiLCJjaGF0Z3B0X3BsYW5fdHlwZSI6InBsdXMiLCJjaGF0Z3B0X3VzZXJfaWQiOiJ1c2VyLTJxUU5UZ1A2VzN4NGtWVU1SNXlaREhnVyIsInBvaWQiOiJvcmctbmtSM0tWREpZMHFZRHgwVVl2N1pOTGFqIiwidXNlcl9pZCI6InVzZXItMnFRTlRnUDZXM3g0a1ZVTVI1eVpESGdXIn0sImh0dHBzOi8vYXBpLm9wZW5haS5jb20vcHJvZmlsZSI6eyJlbWFpbCI6IjIzMTg3Mjc2NjRAcXEuY29tIiwiZW1haWxfdmVyaWZpZWQiOnRydWUsIm5hbWUiOiJjb3JuZWxsX3FxIn0sImlzcyI6Imh0dHBzOi8vYXV0aC5vcGVuYWkuY29tIiwicHdkX2F1dGhfdGltZSI6MTc5NDE5MzA1MzAzNywic2NwIjpbIm9wZW5pZCIsInByb2ZpbGUiLCJlbWFpbCIsIm9mZmxpbmVfYWNjZXNzIiwiYXBpLmNvbm5lY3RvcnMucmVhZCIsImFwaS5jb25uZWN0b3JzLmludm9rZSJdLCJzZXNzaW9uX2lkIjoiYXV0aHNlc3NfRU11MUZMVzNmSzVlbk1Xb1VlN05Lb2JPIiwic2wiOnRydWUsInN1YiI6ImF1dGgwfFFnaDNacENEa3dLTWFrMDBEd1Rjd0dJeCIsImlhdCI6MTc5NTA1NzEyNiwiZXhwIjoxNzk1OTIxMTI2LCJqdGkiOiIzN2E2Y2I1NjgxMDk0MWQ4OWM4OWFiMzc2OTUzOGE3NiIsIm5iZiI6MTc5NTA1NzEyNn0.NXL0Ko1tKgwoLmssmffT5JdJiJqU4gxDF6Hd6_3LZ-Py3g0xFpQiCZ3kjX1q0dZXWDy-E28cD6UPbs8-aJGKQ9Ua5XP-FlHVLZUm9Rpndp5uvFBxT3KGnIDCiT_hM5PoiCBnM8WjEhKuBDKsAGlYkdWQ3xLV7c-HGlropABs0NdZVIesv_SBWoA3GKFX3Z5tyEgb6ueyBtw5ln8qn_FkygoMgFFhbLko6NTEEHeiBx4XPtECjTk7D7L46g0G-EKDWO6gssD_sPnBhkIKnhdU7JreChAfz9DMV--UVEEFzJUOlZSczr-C3pwqCUOVj0uP-pxNoAcOo7O7hcdvVyd0cw",
    "refresh_token": "rt.1.AABjMBLy4UsdiWQ9V2x1N8mTLgreypq9j9gYZ_zfrdQCktdh-7hsI1kMi1DM1XtDRJDVHGATNLHayagCcvw1NGnAsx041oZQ5F53OB1uTdDXDNbWSYtaJ7HCc12o1D9rK1sgtG9edGkhRwe3w95ZLWtV5rldPKCT2dArJ8PEAqL9hgY_CGvzTjREDuIVG-2um3ojKvcbSJwBYw",
    "oauth_provenance": {
      "issuer": "https://auth.openai.com",
      "client_id": "app_EMoamEEZ73f0CkXaXp7hrann"
    },
    "login_owner_id": "3eOC8cPj_mCLpdJURoPus0WL5E2N_-eBOwyknu3eJgc",
    "account_id": "86218a88-9657-4515-a3c7-9906edf98155"
  },
  "last_refresh": "2026-11-19T02:58:46.710003Z"
}
