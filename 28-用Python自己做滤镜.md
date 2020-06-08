# 用Python自己做滤镜

### PIL内置滤镜滤镜效果

| 滤镜名称                      | 含义               |
| ----------------------------- | ------------------ |
| ImageFilter.BLUR              | 模糊滤镜           |
| ImageFilter.CONTOUR           | 轮廓               |
| ImageFilter.EDGE_ENHANCE      | 边界加强           |
| ImageFilter.EDGE_ENHANCE_MORE | 边界加强(阀值更大) |
| ImageFilter.EMBOSS            | 浮雕滤镜           |
| ImageFilter.FIND_EDGES        | 边界滤镜           |
| ImageFilter.SMOOTH            | 平滑滤镜           |
| ImageFilter.SMOOTH_MORE       | 平滑滤镜(阀值更大) |
| ImageFilter.SHARPEN           | 锐化滤镜           |

### 自定义滤镜

```
from PIL import Image, ImageFilter


class Linmous(ImageFilter.BuiltinFilter):
    name = "Linmous"
    # fmt: off
    filterargs = (5, 5), 15, 0, (
        -1, -1, -1, -1, -1,
        -1, 2, 4, 2, -1,
        -1, 4, 6, 4, -1,
        -1, 2, 4, 2, -1,
        -1, -1, -1, -1, -1,
    )
    # fmt: on


img1 = Image.open("youngGirl.jpg", "r")

img = img1.filter(Linmous)
img.show()
```