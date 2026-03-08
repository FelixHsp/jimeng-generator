---
name: jimeng-generator
description: 即梦 4.0 图片生成器，通过文本描述生成高质量图片，支持多图编辑与智能比例。
---

# 即梦 4.0 图片生成器

基于火山引擎即梦 AI 4.0 生成图片。一条命令完成：提交任务 → 等待完成 → 保存图片。

## 环境变量

| 变量 | 说明 |
|------|------|
| `VOLCENGINE_AK` | 火山引擎 Access Key（必需） |
| `VOLCENGINE_SK` | 火山引擎 Secret Key（永久凭证） |
| `VOLCENGINE_TOKEN` | 安全令牌（STS 临时凭证） |

永久凭证需 AK + SK；临时凭证需 AK + Token。

## 安装

```bash
npm install
```

## 基本用法

```bash
npx ts-node scripts/generate.ts "提示词"
```

脚本会自动提交 → 轮询 → 保存图片到 `./output/`。

## 完整用法

```bash
npx ts-node scripts/generate.ts "提示词" [选项]
```

### 选项

| 选项 | 说明 | 默认值 |
|------|------|--------|
| `--images <url,...>` | 参考图片 URL，逗号分隔，最多 10 张 | — |
| `--width <n>` | 输出宽度 | 自动 |
| `--height <n>` | 输出高度 | 自动 |
| `--size <n>` | 输出面积 | 自动 |
| `--scale <0-1>` | 文本影响程度 | 0.5 |
| `--single` | 强制单图输出 | false |
| `--out <dir>` | 输出目录 | ./output |
| `--no-save` | 不保存，只输出 URL | false |
| `--interval <ms>` | 轮询间隔 | 3000 |
| `--timeout <ms>` | 最大等待时间 | 180000 |
| `--debug` | 调试模式 | false |

## 使用示例

### 文生图

```bash
npx ts-node scripts/generate.ts "水墨山水画"
```

### 指定尺寸

```bash
npx ts-node scripts/generate.ts "赛博朋克城市" --width 2560 --height 1440
```

### 图片编辑

```bash
npx ts-node scripts/generate.ts "背景换成星空" --images "https://example.com/photo.jpg"
```

### 多图组合

```bash
npx ts-node scripts/generate.ts "合成一张合照" --images "https://a.jpg,https://b.jpg"
```

### 强制单图 + 高影响

```bash
npx ts-node scripts/generate.ts "精细插画风格的城堡" --single --scale 0.8
```

## 输出格式

成功时 stdout 输出 JSON：

```json
{
  "success": true,
  "taskId": "7392616336519610409",
  "prompt": "水墨山水画",
  "count": 1,
  "files": ["./output/1.png"],
  "urls": ["https://..."]
}
```

失败时 stderr 输出 JSON：

```json
{
  "success": false,
  "error": { "code": "FAILED", "message": "错误描述" }
}
```

## 即梦 4.0 特性

- **智能比例**：可在 prompt 中描述比例，模型自动适配最优宽高
- **多图输入**：最多 10 张参考图，支持图片编辑和多图组合
- **多图输出**：单次最多输出 15 张关联图片
- **4K 输出**：支持从 1K 到 4K 分辨率
- **中文增强**：显著提升中文生成准确率

## 推荐尺寸

| 分辨率 | 1:1 | 4:3 | 3:2 | 16:9 | 21:9 |
|--------|-----|-----|-----|------|------|
| 2K | 2048×2048 | 2304×1728 | 2496×1664 | 2560×1440 | 3024×1296 |
| 4K | 4096×4096 | 4694×3520 | 4992×3328 | 5404×3040 | 6198×2656 |
