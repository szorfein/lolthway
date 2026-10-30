---
title: "ImageMagick cheat sheet"
description: "My best commands for the cli tool imagemagick."
date: 2026-10-30
lang: en
category: notes
tags: ["ImageMagick", "CLI"]
cover: notes
---

## Round an Image

#### Check Image Dimensions

You can use the `file` command:

```sh
file ryvst.png
```

return for me `ryvst.png: PNG image data, 650 x 520, 8-bit/color RGBA, non-interlaced`.

#### Create the Mask (The "Round Rectangle")

To create a rounded rectangle with `20` on each side, we use `-draw`. The output file is saved as `mask.png`.

```sh
magick -size 650x520 xc:none -draw "roundrectangle 0,0,649,519,20,20" mask.png
```

The `roundrectangle` match with your image size with `-1`: `649,519`.

#### Apply the Mask to the Original Image

Now, we apply this mask to your original image (`ryvst.png`) and save it as `rounded.png`.

```sh
magick ryvst.png -alpha Set mask.png -compose DstIn -composite rounded.png
```

## References

- https://joeldare.com/rounding-image-corners-with-imagemagick
