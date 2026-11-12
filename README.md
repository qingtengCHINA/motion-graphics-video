# motion-graphics-video

把一段视频当作一件动态设计作品来做的 Skill:从一句需求出发,产出作品集级动态图形视频(15 / 30 / 60 秒或自定时长)。真实资料取证、数据驱动时间轴、TTS 旁白、分块渲染、对成片实测验收,交付 mp4 + 抽帧总览图 + 剧本与来源清单 + 工程目录。

纯 Markdown 工艺文件:不含后端、不收集任何 Key;模型与密钥都在你自己的 Agent 环境里。

- 版本:v2.1.0(2026-11-12)
- 许可:[CC0 1.0 Universal](LICENSE) —— 提示词与工艺以 CC0 释放,可自由使用、修改、再分发,无需署名
- 适用:任何能读 Skill 的 coding Agent(Qoder / Claude Code / Codex / Cursor 等)

## 安装

每条命令只下载一个 `SKILL.md` 文本到对应 Agent 的 skills 目录;装好后新会话自动识别。

**Qoder**

```bash
mkdir -p ~/.qoder/skills/motion-graphics-video && curl -fsSL https://raw.githubusercontent.com/qingtengCHINA/motion-graphics-video/main/SKILL.md -o ~/.qoder/skills/motion-graphics-video/SKILL.md
```

**Claude Code**

```bash
mkdir -p ~/.claude/skills/motion-graphics-video && curl -fsSL https://raw.githubusercontent.com/qingtengCHINA/motion-graphics-video/main/SKILL.md -o ~/.claude/skills/motion-graphics-video/SKILL.md
```

**Codex**

```bash
mkdir -p ~/.codex/skills/motion-graphics-video && curl -fsSL https://raw.githubusercontent.com/qingtengCHINA/motion-graphics-video/main/SKILL.md -o ~/.codex/skills/motion-graphics-video/SKILL.md
```

**Cursor**

```bash
mkdir -p ~/.cursor/skills/motion-graphics-video && curl -fsSL https://raw.githubusercontent.com/qingtengCHINA/motion-graphics-video/main/SKILL.md -o ~/.cursor/skills/motion-graphics-video/SKILL.md
```

**项目本地(随仓库提交,团队共用)**

```bash
mkdir -p .skills/motion-graphics-video && curl -fsSL https://raw.githubusercontent.com/qingtengCHINA/motion-graphics-video/main/SKILL.md -o .skills/motion-graphics-video/SKILL.md
```

**Windows PowerShell**(以 Qoder 为例,其余 Agent 替换路径)

```powershell
New-Item -ItemType Directory -Force -Path "$HOME\.qoder\skills\motion-graphics-video" | Out-Null; Invoke-WebRequest -Uri "https://raw.githubusercontent.com/qingtengCHINA/motion-graphics-video/main/SKILL.md" -OutFile "$HOME\.qoder\skills\motion-graphics-video\SKILL.md"
```

也可以直接 `git clone` 本仓库,把 `SKILL.md` 放进上述任一目录。

验证:对应路径下 `SKILL.md` 存在;在 Agent 里问「有哪些 skills」应列出 `motion-graphics-video`。

## 使用

装好后对 Agent 说一句话即可,例如:

> 使用 motion-graphics-video skill,制作一段 30 秒的动态图形视频:讲千问三年从 7B 到万亿参数,用真实网页与论文做证据,纪录片语气,深色克制。

Skill 开工前会确认三件事(需求、主模型、TTS 模型,推荐千问系列),然后按六阶段推进:

1. **研究** — `facts.md`:每条事实带来源 URL 与统计时点
2. **剧本** — `script.json`:朗读稿与屏幕文字分字段
3. **素材** — `anchors.json` + `sources.md`:真实截图与原文坐标
4. **制作** — `timeline.json`:旁白实测时长驱动逐帧时间轴
5. **渲染** — 分块 mp4:两遍 loudnorm −16 LUFS
6. **自检** — 总览图 + 实测报告:帧数 / 响度 / 回读

环境要求:Node ≥ 20、ffmpeg、Python ≥ 3.10、Playwright Chromium、中文字体,以及一个支持长程工具调用的模型与 TTS 密钥(只存项目 `.env`)。

## 硬性约束

- 密钥只存 `.env`,文档 / 日志 / 对话零回显
- 删除一律移入回收站,不用 `rm`
- 不绕过反爬、人机验证、付费墙;被拦就换来源并注明
- 每个上屏的数字、日期、引语都有来源;再创作画面标「示意」
- 自检数据来自对成片的实测;没测的项目写「未测」,不写「通过」

## 贡献

Issue 与 PR 欢迎:返工记录、故障回退、新的动效手法都可以沉淀进 `SKILL.md`。提交即表示同意以 CC0 释放。