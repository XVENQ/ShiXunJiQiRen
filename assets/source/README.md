# 源工程托管说明（Source Files）

本目录**不存放**源工程文件，原因如下。

## 文件清单与托管策略

| 文件 | 实际大小 | 状态 | 托管方式 |
| --- | --- | --- | --- |
| `jiqiren-a-2.spp` | 168 MB | ❌ 不在本仓库 | Git LFS / Release 附件 / 网盘外链（三选一） |
| `机器人.max` | 16.3 MB | ⚠️ 可选 | 可直接入库（< 100 MB） |
| `jiqiren-sp-1w.spp` | 41 MB | ⚠️ 可选 | 中间版本，一般不必公开 |
| 教学视频（15 个 mp4） | ≈ 5.6 GB | ❌ 永不入库 | 课程资料，不属于作品 |

## 为什么 .spp 不进主仓库

1. **超过 GitHub 单文件 100 MB 硬限制**，直接推送会失败。
2. 即使改用 Git LFS，免费额度只有 1 GB。一个 168 MB 的工程，克隆 6 次就烧完，别人 fork 一下你的额度就没了。
3. `.spp` 是二进制容器，**diff 无意义**，每次保存都是整份重传。

## 推荐做法

### 方案 A：Release 附件（推荐）

```bash
# 先本地留一份，再作为 Release 附件上传
gh release create v1.0 \
  "jiqiren-a-2.spp#ROBOT-A2-Painter-Project.spp" \
  --title "ROBOT-A2 v1.0 源工程" \
  --notes "Substance 3D Painter 2022 工程，含完整图层栈"
```

Release 附件不计入仓库体积，单文件上限 2 GB，公开仓库免费。

### 方案 B：网盘外链

README 里写一行：

> 源工程（Painter 168 MB）：`https://pan.baidu.com/s/xxxx`  提取码 `xxxx`

缺点：链接会失效，需要偶尔维护。优点是零成本、无额度限制。

### 方案 C：Git LFS

如果确实要版本管理 `.spp`：

```bash
git lfs install
git lfs track "*.spp"
git add .gitattributes
```

`.gitattributes` 已预置相关规则，取消注释即可。

## 打开这份工程需要的环境

- Adobe Substance 3D Painter（2022 或更新版本）
- `.spp` 内含完整烘焙贴图与图层栈，不依赖外部文件
- 若提示缺失资源，检查 `resources/` 内的贴花素材是否随工程保存

## 关于 `.painter_lock`

`jiqiren-a-2.spp.painter_lock` 是 Painter 运行时生成的锁文件（78 字节），**没有任何价值**，已被 `.gitignore` 排除。
