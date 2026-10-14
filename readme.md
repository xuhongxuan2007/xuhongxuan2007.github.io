# 静态个人主页（Static Personal Homepage）

一个**单文件**的静态个人主页，纯手写 HTML / CSS / JavaScript，无任何框架和构建步骤。
通过 GitHub Pages 部署，可在互联网上直接访问。

线上地址：`https://<你的用户名>.github.io/`

---

## 一、项目简介

这是通识任务的第三个作业：用 Agent 辅助编写一个关于自己的静态网页，并部署到公网。

技术上刻意保持简单：

- **零依赖**：一个 `index.html` 装下全部结构、样式、脚本
- **零构建**：不需要 node / npm，改完直接刷新就是最新效果
- **响应式**：手机和电脑都能正常显示
- **数据与视图分离**：个人信息集中放在文件末尾的「配置区」，改数据不用碰样式

## 二、页面包含的板块

| 板块 | 内容 |
| --- | --- |
| 首屏 Hero | 姓名、打字机效果的个人标签、头像、跳转按钮 |
| 关于我 | 自我介绍 + 学校 / 城市 / 主力语言 / 当前目标 |
| 技能栈 | 6 项技能卡片，进入视口时进度条带动画 |
| 项目作品 | 项目卡片，附 GitHub 源码链接 |
| 联系方式 | 邮箱 / GitHub / 微信等，只展示愿意公开的 |
| 页脚 | 版权年份（脚本自动取当前年份） |

## 三、交互效果

- 背景：三团缓慢漂浮的渐变光晕 + 顶部渐隐网格
- 导航栏：滚动后自动加毛玻璃背景
- 首屏：打字机循环输出多行个人标签
- 头像：外圈 conic-gradient 旋转光环绕行
- 滚动：元素进入视口时淡入上移，技能进度条从 0 动画到目标值
- 右下角：回到顶部按钮（滚动超过 500px 才出现）
- 无障碍：监听 `prefers-reduced-motion`，用户关闭动效时自动禁用全部动画

## 四、如何修改成你自己的内容

打开 `index.html`，拉到 `<script>` 标签，最上方就是**配置区**，依次修改：

```javascript
const GITHUB_USERNAME = "YOUR_GITHUB_USERNAME";  // 你的 GitHub 用户名
const NAME            = "你的名字";               // 展示用的姓名/昵称
const TYPING_LINES    = [ ... ];                 // 首屏打字的几句话
const SKILLS          = [ ... ];                 // 技能与熟练度
const PROJECTS        = [ ... ];                 // 项目作品
const CONTACTS        = [ ... ];                 // 联系方式
```

**注意**：页面里凡是标着 <span style="color:#ffd166">黄色虚线下划线</span> 的文字（类名 `todo`），都是留给你手填的占位内容，
请务必替换成真实信息，不要直接把占位符交上去。

## 五、本地预览

直接双击 `index.html` 用浏览器打开即可。

如果想用本地服务器预览（更接近线上环境）：

```bash
# 在 first-html 目录下，任选一种
python -m http.server 8000
# 然后浏览器访问 http://localhost:8000
```

## 六、部署到 GitHub Pages

### 方式一：用户主页仓库（推荐，地址更短）

仓库名必须**严格等于** `<你的用户名>.github.io`，例如用户名是 `zhangsan`，仓库就叫 `zhangsan.github.io`。

```bash
cd /d D:\play_do\first-html
git init -b main
git add .
git commit -m "feat: 静态个人主页"
git remote add origin https://github.com/<你的用户名>/<你的用户名>.github.io.git
git push -u origin main
```

推送完成后访问 `https://<你的用户名>.github.io/` 即可（首次生效通常 1–2 分钟，偶尔要等更久）。

### 方式二：普通仓库 + 开启 Pages

1. 建一个任意名字的仓库，例如 `about-me`，把文件推上去
2. 打开仓库页面 → **Settings** → 左侧 **Pages**
3. **Source** 选择 `Deploy from a branch`
4. **Branch** 选 `main`，目录选 `/ (root)`，点 **Save**
5. 等 1–2 分钟，地址为 `https://<你的用户名>.github.io/about-me/`

### 关于 `.nojekyll`

本目录下的 `.nojekyll` 是空文件，作用是告诉 GitHub Pages **跳过 Jekyll 处理**，避免以 `_` 或 `.` 开头的文件被忽略。

### 推送失败：连接超时怎么办

如果 `git push` 卡住然后报 `Failed to connect to github.com port 443 ... Timed out`，
是网络访问 GitHub 受限，而不是命令写错了。

先确认代理已经开着，然后让 git 也走代理（浏览器通不代表 git 通，这是两套配置）：

```bash
# 把 7890 换成你代理软件的实际端口（Clash 常见 7890，v2rayN 常见 10809）
git config --global http.proxy  http://127.0.0.1:7890
git config --global https.proxy http://127.0.0.1:7890
```

推送完成后建议取消，避免以后在无代理环境下 git 一直报连不上代理：

```bash
git config --global --unset http.proxy
git config --global --unset https.proxy
```

验证 GitHub 是否可达：

```powershell
# Windows 自带的 curl
curl.exe -I https://github.com

# 或者用 PowerShell 原生命令
Invoke-WebRequest 'https://github.com' -UseBasicParsing -TimeoutSec 20 | Select-Object StatusCode
```

## 七、目录结构

```text
first-html/
├── index.html    # 页面本体：结构 + 样式 + 脚本
├── readme.md     # 本说明文档
└── .nojekyll     # 让 GitHub Pages 跳过 Jekyll 处理
```

## 八、后续可以做的改进

- [ ] 增加作品集详情页，点击项目卡片进入
- [ ] 接入深色 / 浅色主题切换按钮
- [ ] 增加中英文切换
- [ ] 用真实头像图片替换当前的首字母圆形头像
- [ ] 加上访问量统计（如不蒜子）

---

作者：pig sister
