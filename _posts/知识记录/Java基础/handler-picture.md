title: "图片裁剪整理"
date: 2015-04-20 23:09:09
tags:
- java
- Nodejs
categories: Java基础
---


图片裁剪，核心思想是指定**起始点坐标**和相对于起始点坐标的**偏移向量**，这样就可以精确的指定被裁减图片中的一个矩形区域。

核心代码：
```java
String name = "D:/wiz/1.jpg";
String subpath = "D:/wiz/2.jpg";

ImageInputStream iis = ImageIO.createImageInputStream(new FileInputStream(name));
Iterator<ImageReader> it = ImageIO.getImageReadersByFormatName("jpg");
ImageReader reader = it.next();
reader.setInput(iis, true);
ImageReadParam param = reader.getDefaultReadParam();
Rectangle rect = new Rectangle(100, 200, 320, 525);
param.setSourceRegion(rect);
BufferedImage bi = reader.read(0, param);
ImageIO.write(bi, "jpg", new File(subpath));
```
<!--more-->
这样就可以将`D:/wiz/1.jpg`以(100,100)为起点，以(320,525)为向量裁剪出一个矩形。图片左上角为(0,0)，向右向下为正方向。
**假如超出图片范围**，则按**向量与图片边界的交点为向量终点**处理。

---

下面使用Nodejs的解决方案，使用的库是`GraphicsMagick`。

crop
Crops the image to the given width and height at the given x and y position.
```
gm("img.png").crop(width, height, x, y)

```
待丰富
