# Image Resources Guide

This blog has automatically generated the following SVG image resources. You can choose to use them according to your preferences.

## Avatars

| Filename | Color Style | Description |
|----------|-------------|-------------|
| `bio-photo.svg` | Purple-Blue Gradient | **Currently in use** |
| `bio-photo-alt1.svg` | Orange-Red Gradient | Alternative option 1 |
| `bio-photo-alt2.svg` | Green Gradient | Alternative option 2 |

### How to Change Avatar

Modify the `author.avatar` configuration in `_config.yml`:

```yaml
author:
  avatar: /assets/images/bio-photo.svg      # Purple-blue gradient (current)
  # avatar: /assets/images/bio-photo-alt1.svg  # Orange-red gradient
  # avatar: /assets/images/bio-photo-alt2.svg  # Green gradient
```

## Banners

| Filename | Color Style | Description |
|----------|-------------|-------------|
| `banner-no-text.svg` | Solid Gradient | **Currently in use**, simple and clean |
| `placeholder.svg` | Blue-Cyan Gradient | Alternative option 1 |
| `placeholder-alt1.svg | Purple-Red-Orange Gradient | Alternative option 2 |
| `placeholder-alt2.svg` | Dark Tech Style | Alternative option 3 |

### How to Change Banner

Modify the `header.overlay_image` configuration in `index.md`:

```yaml
header:
  overlay_image: /assets/images/banner-no-text.svg      # Solid gradient (current)
  # overlay_image: /assets/images/placeholder.svg  # Blue-cyan gradient
  # overlay_image: /assets/images/placeholder-alt1.svg  # Purple-red-orange gradient
  # overlay_image: /assets/images/placeholder-alt2.svg  # Dark tech style
```

## Using Custom Images

If you want to use your own photos:

1. Put the image in the `assets/images/` directory
2. Recommended formats: JPG or PNG
3. Recommended dimensions:
   - Avatar: 200x200 pixels (square)
   - Banner: 1200x400 pixels (3:1 ratio)

4. Modify the path in the configuration file, for example:
   ```yaml
   author:
     avatar: /assets/images/my-avatar.jpg

   header:
     overlay_image: /assets/images/my-banner.jpg
   ```

## Advantages of SVG Images

The SVG images currently generated have the following advantages:
- **Vector Format**: No loss of quality at any scale
- **Small File Size**: Fast loading speed
- **Editable**: Can be modified with any vector graphics software
- **Gradient Effects**: Modern appearance
