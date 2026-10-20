# 贴图目录（Textures）

## 状态：✅ 已导出

单套 4K PBR 贴图集，来自 `jiqiren-a-2.spp`，输出模板 `PBR Metallic Roughness`，分辨率 4096 × 4096，填充 `无限膨胀 × 16`。

| 文件 | 分辨率 | 色彩空间 | 说明 |
| --- | --- | --- | --- |
| `robot_a2_BaseColor.png` | 4096² | sRGB | 军绿/锈黄涂装 + 青绿面板 + 贴纸 |
| `robot_a2_Roughness.png` | 4096² | Linear | 均值约 0.81，整体哑光 |
| `robot_a2_Metallic.png` | 4096² | Linear | 均值约 0.13，仅关节/管线为金属 |
| `robot_a2_Normal.png` | 4096² | Linear | OpenGL 规范（Blender/Unity 直连，UE 需翻绿通道） |

合计约 55 MB，单文件均 < 100 MB，可正常入库。

## 关于 Height

Painter 导出的 `Height` 通道**全图为纯白（均值 255，标准差 0）**，说明工程里没有画高度细节，属于空通道，已删除，不参与渲染。

需要凹凸信息时用 Normal 即可；后续补画高度后再导出并接 Displacement。

## 使用方式（Blender）

```
BaseColor → Principled BSDF / Base Color   (sRGB)
Roughness → Principled BSDF / Roughness    (Non-Color)
Metallic  → Principled BSDF / Metallic     (Non-Color)
Normal    → Normal Map 节点 → Normal       (Non-Color)
```

三个数据通道（Roughness / Metallic / Normal）**必须设为 Non-Color**，否则会被当成 sRGB 做伽马校正，材质全错。

## 体积说明

- 4K PNG 未做有损压缩，BaseColor 18 MB、Normal 25 MB 属于正常范围。
- 若仓库体积吃紧，可降到 2048²（约 12 MB 全包），代价是近景细节变软。

## 第三方素材声明

制作中使用的贴花素材（`PNG DISEÑO (x).png` 等）来自第三方素材站，版权不属于本项目：

- 原素材文件**未上传**
- 已烘焙进 `BaseColor`，属于成品的一部分
- 来源记录见 `../reference/README.md`
