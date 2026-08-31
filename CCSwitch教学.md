# CC Switch 详细教学

> 图形界面管理 Codex / Claude Code / Gemini 的 API 供应商，一键切换，免手动编辑 `auth.json`、`config.toml`。**适合人用**，替代「手动写文件」那种只有 agent 才方便搞的方式。

## 是什么

CC Switch（cc-switch）是个跨平台桌面工具，把 API 地址、Key、模型、MCP 等统一收进一个图形界面，管理多个供应商并一键切换。底层它会自动写 `~/.codex/auth.json` + `config.toml`，你不用碰文件。

## 安装

- 下载：GitHub Releases `https://github.com/farion1231/cc-switch/releases` 或官网 `ccswitch.io`
- Windows：MSI 安装包，或便携版 ZIP（SmartScreen 提示时点「更多信息 → 仍要运行」）
- ⚠️ 只从官方渠道下载，任何要付费/充值的「CC Switch」站都是仿冒

## 场景一：导入 ChatGPT 订阅账号（拼车号 / OAuth 号）

这是用 ChatGPT 订阅（含拼车 OAuth 号）的正规 GUI 导入方式：

1. 顶部切到 **Codex** 标签
2. 进「**OAuth 认证中心**」登录 ChatGPT 账号，登录邮箱会显示在「已登录账号」列表
3. 添加供应商 → 选 **Codex OAuth 预设**
4. 多个号时点「添加其他账号」重复登录，用「选择账号」下拉框给供应商指定账号
5. 点供应商卡片「**启用**」

启用后：

- CC Switch 把请求路由到 `chatgpt.com/backend-api/codex`，Base URL 不用手填；
- token 过期前 60 秒自动后台刷新；
- 卡片底部显示配额百分比（<70% 绿、70–89% 橙、≥90% 红）、重置倒计时、手动刷新按钮；
- token 彻底失效时显示「会话已过期」，需到认证中心移除账号重新登录。

> ⚠️ 风险：Codex OAuth 反代走的是逆向的 OAuth 流程，可能违反 OpenAI 条款、导致账号被限，长期可用性无法保证。

## 场景二：添加第三方 API 供应商（中转站等）

1. Codex 标签右上角 **+** 打开「添加供应商」
2. 两种方式：
   - **预设模板**：DeepSeek / Kimi / MiniMax / GLM / SiliconFlow 等，填 API Key 即可（Base URL、模型映射自动配好）
   - **手动输入**：自定义填 API Key + 请求地址
3. 点 **Add** 创建，再点卡片「**启用**」

注意：

- Base URL 末尾**不要加斜杠**（否则拼出双斜杠报错）
- 只支持 Chat Completions 协议的供应商（DeepSeek / Kimi / MiniMax 等），要在「设置 → 代理」开**本地代理**（默认 `http://127.0.0.1:15721`），由 CC Switch 把 Codex 的 Responses 请求转成 Chat Completions
- 切换后**重启终端 / Codex** 才生效

## 切换供应商

- 主界面：点目标供应商卡片「启用」
- 系统托盘：托盘菜单直接选目标供应商，立即生效

## 常见问题

| 现象 | 原因 | 解决 |
|------|------|------|
| 「Device Code 已过期」 | 验证码超时（约 15 分钟） | 点「重试」重新获取 |
| 「用户拒绝授权」 | 浏览器没点授权 | 重新登录，浏览器里点「授权」 |
| 「请先登录 ChatGPT 账号」 | 创建供应商前没登录 | 先到 OAuth 认证中心登录 |
| 「会话已过期」 | token 完全失效 | 移除账号后重新登录 |
| 配额「查询失败」 | 网络/接口抖动 | 点「刷新」重试 |
| Sub2API 连不上 localhost | 本地部署地址不识别 | 改成实际 IP 后再测速 |
