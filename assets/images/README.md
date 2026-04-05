# 图片资源说明

本博客已自动生成以下 SVG 图片资源，你可以根据喜好选择使用。

## 头像

| 文件名 | 颜色风格 | 说明 |
|--------|----------|------|
| `bio-photo.svg` | 紫蓝渐变 | **当前使用** |
| `bio-photo-alt1.svg` | 橙红渐变 | 备选方案 1 |
| `bio-photo-alt2.svg` | 绿色渐变 | 备选方案 2 |

### 如何更换头像

在 `_config.yml` 中修改 `author.avatar` 配置：

```yaml
author:
  avatar: /assets/images/bio-photo.svg      # 紫蓝渐变（当前）
  # avatar: /assets/images/bio-photo-alt1.svg  # 橙红渐变
  # avatar: /assets/images/bio-photo-alt2.svg  # 绿色渐变
```

## 横幅

| 文件名 | 颜色风格 | 说明 |
|--------|----------|------|
| `placeholder.svg` | 蓝青渐变 | **当前使用** |
| `placeholder-alt1.svg` | 紫红橙渐变 | 备选方案 1 |
| `placeholder-alt2.svg` | 深色科技风 | 备选方案 2 |

### 如何更换横幅

在 `index.md` 中修改 `header.overlay_image` 配置：

```yaml
header:
  overlay_image: /assets/images/placeholder.svg      # 蓝青渐变（当前）
  # overlay_image: /assets/images/placeholder-alt1.svg  # 紫红橙渐变
  # overlay_image: /assets/images/placeholder-alt2.svg  # 深色科技风
```

## 使用自定义图片

如果你想使用自己的照片：

1. 将图片放到 `assets/images/` 目录
2. 推荐格式：JPG 或 PNG
3. 推荐尺寸：
   - 头像：200x200 像素（正方形）
   - 横幅：1200x400 像素（3:1 比例）

4. 修改配置文件中的路径，例如：
   ```yaml
   author:
     avatar: /assets/images/my-avatar.jpg

   header:
     overlay_image: /assets/images/my-banner.jpg
   ```

## SVG 图片的优势

当前生成的 SVG 图片具有以下优势：
- **矢量格式**：任意缩放不失真
- **文件小**：加载速度快
- **可编辑**：可以用任何矢量图形软件修改
- **渐变效果**：现代感强
