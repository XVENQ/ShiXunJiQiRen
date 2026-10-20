# 05 · GitHub 发布规范

> 这份文档解决的问题：作品集仓库**能打开、能看懂、下载得下来、不违反平台规则**。

## 一、先解决体积问题（最容易翻车的地方）

原始实训文件夹总大小约 **6.5 GB**，直接 `git push` 必定失败。GitHub 的硬规则：

| 限制 | 数值 | 后果 |
| --- | --- | --- |
| 单个文件上限 | 100 MB | 超过直接拒收 |
| 单文件警告线 | 50 MB | 提示建议用 LFS |
| 仓库建议体积 | < 1 GB | 超过克隆极慢，浏览体验差 |
| Git LFS 免费额度 | 1 GB 存储 / 1 GB 月流量 | 超额需付费 |

**本项目的处理策略：**

| 内容 | 体积 | 处理方式 |
| --- | --- | --- |
| 展示图 `renders/` | ≈ 25 MB | 正常入库（建议压到 2K / JPEG q92） |
| 模型 `assets/models/` | ≈ 25 MB | 正常入库 |
| PBR 贴图 `assets/textures/` | ≈ 55 MB（4K × 4） | 正常入库；单文件均 < 100 MB |
| Painter 工程 `.spp` | 168 MB | **不入库**。用 Git LFS + Release 附件，或外部网盘链接 |
| 3ds Max 工程 `.max` | 16 MB | 可选入库；若含贴图引用指向本地路径，建议不入库 |
| 教学视频 15 个 mp4 | ≈ 5.6 GB | **永不入库**。这是别人的课，不是你的作品 |
| 第三方素材图 | ≈ 25 MB | **不入库**。版权不属于你 |

### 如果一定要托管 .spp

方案 A：Git LFS（推荐给有钱/有额度的情况）

```bash
git lfs install
git lfs track "*.spp" "*.max"
git add .gitattributes
```

`.gitattributes` 已在本仓库预置，取消注释即可生效。

方案 B：发布到 Release（免费，但单文件仍受 2 GB 限制，且不能算仓库大小）

```bash
gh release create v1.0 assets/source/robot_a2_painter.spp#ROBOT-A2-Painter-Project.spp \
  --title "ROBOT-A2 源工程" --notes "Substance 3D Painter 工程文件（168 MB）"
```

方案 C：百度网盘 / OneDrive 外链，README 里写清楚「源工程另附」。

**推荐 C 或 B**。LFS 免费额度只有 1 GB，一个 168 MB 的工程克隆几次就烧完了。

## 二、命名规范

### 仓库名
`hardsurface-robot-portfolio` —— 全小写、连字符、能读出内容。
不要用 `zuopin`、`c#`、`3d`、`proj1`、`徐恩2546309838`。

### 目录名
全小写英文 + 连字符：`renders/`、`assets/models/`、`docs/`
不要出现中文、空格、括号（Git 跨平台会把中文变乱码，Windows 上尤其明显）。

### 文件名
```
<资产名>_<内容>_<版本>.<扩展名>
robot_hardsurface_final.obj
robot_hardsurface_lod1.fbx
01_hero_clay.png
```

### 学号 / 真实姓名
**必须从仓库里删掉**。`徐恩2546309838/` 这种目录名把姓名和学号绑在一起公开，属于个人信息泄露。作品集用项目名 + 代号（ROBOT-A2）即可。

## 三、README 呈现规范

这是别人点开仓库后 10 秒内看到的东西，决定他关不关页面。

1. **第一屏必须有图**：一张 Hero 渲染，宽度占满，不要放 logo 或 badge 堆砌。
2. **首段讲清楚**：这是什么、用什么软件做的、证明什么能力。三句话以内。
3. **有数据**：面数、UV 利用率、尺寸、耗时。招聘方看的是可验证的指标。
4. **图片用相对路径**：`renders/01_hero_clay.png`，不要用绝对路径或外链（外链会挂）。
5. **图片宽度用 HTML 控制**：`<img src="..." width="46%" />`，让三视图、特写能并排。
6. **不要写「谢谢观看」「请多多指教」**：作品集不是作业答辩。

### 推荐图片规格

| 用途 | 尺寸 | 格式 | 说明 |
| --- | --- | --- | --- |
| Hero / 封面 | 1600 × 2000 | PNG 或 JPEG q92 | 单张 < 3 MB |
| 三视图 | 900 × 1300 | PNG | 统一背景、统一光照 |
| 特写 | 1200 × 1200 | PNG | 方形 |
| 线框 | 1600 × 2000 | PNG | 深底亮线 |
| UV 布局 | 2048 × 2048 | PNG | 带统计标注 |
| 社交预览图 | 1280 × 640 | JPEG | 仓库 Settings → Social preview |

## 四、仓库设置清单

```bash
# 1. 初始化
git init
git add .
git commit -m "feat: ROBOT-A2 hard-surface asset portfolio"
git branch -M main
git remote add origin git@github.com:<user>/hardsurface-robot-portfolio.git
git push -u origin main
```

推送后在网页端补三样东西（很多人漏掉）：

- **Description**：`Hard-surface robot asset — 3ds Max · RizomUV · Substance Painter`
- **Topics**：`3d`, `hard-surface`, `3dsmax`, `substance-painter`, `rizomuv`, `game-asset`, `portfolio`
- **Social preview**：上传 1280×640 的封面图

## 五、许可与版权

- 模型和展示图用 **CC BY-NC 4.0**（署名-非商业性使用）。别用 MIT——MIT 是代码协议，不适用于美术资产，且等于允许别人商用你的模型。
- 第三方贴图素材（贴花 PNG）**不要上传**。素材站的授权通常只允许用于成品，不允许再分发原文件。
- 参考图（花瓣等平台）**只写来源，不存原图**。存了就是侵权。
- 如果模型参考了某个具体作品，README 里注明——这是基本职业道德，也能避免争议。

## 六、常见错误

| 错误 | 后果 | 正确做法 |
| --- | --- | --- |
| 把 6 GB 视频一起提交 | 推送失败，或被平台警告 | `.gitignore` 排除 `*.mp4` |
| `git add .` 直接提交 | 把 `.spp.painter_lock`、`autosave` 也带上 | 先检查 `git status` |
| 图片用绝对路径 | 网页上全部裂图 | 相对路径 |
| README 只有文字 | 没人看 | 首屏放图 |
| 文件名中文 | 部分系统乱码 | 全英文 |
| 提交学号姓名 | 隐私泄露 | 用项目代号 |
| 没有 LICENSE | 别人不知道能不能用 | 加 CC BY-NC 4.0 |
