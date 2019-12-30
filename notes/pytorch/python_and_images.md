python的图像操作扩展
===

### 关于图片加载和显示
使用plt自带的模块可以用以下方式进行读取
```
import matplotlib.image as mpimg # mpimg 用于读取图片
img = mpimg.imread('image/path') # 读取图片为np array格式
```
若使用PIL则读取方式变为
```
from PIL import Image
import matplotlib.pyplot as plt
pil_im = Image.open('1.jpg').convert("rgb") #读取图片为Image格式
#图片显示仍需要借助plt
plt.figure("dog")
plt.imshow(pil_im)
plt.show()
```
当然还可以用opencv进行读取，从时间上看其效率要高于`PIL`，但读取完是`numpy`格式，`torchvision.transforms`并不能直接对其进行操作。不过将其读取后转为`Image`的效率从时间上看也比直接用`PIL`读取要快得多。
```
import cv2 as cv
img = cv.imread("image/path", 1) #1为读取RGB模式，0为读取灰度模式，None则不指定模式
img_use = cv.cvtColor(img , cv.color_BGR2RGB) #opencv的图片默认是BGR格式，一般需要对其进行转换
img_show = cv.cvtColor(img_use, cv.COLOR_RGB2BGR)
cv.imwrite("save/path", img)
```

### 图片格式的变换
如果需要将`PIL`读取的`Image`格式变为`np.array`格式，可以通过如下操作完成
```
np_im = np.array(pil_im)
pil_im = Image.fromarray(np.uint8(np_im))
```
需要注意的是。如果读取的图像是png格式，则`Image.open`读取产生的图像是`PngImagePlugin`格式（不经过`convert`变换），这时候无法直接通过上述操作转换图片类型，解决方法之一是用`Image.crop()`进行一次剪裁，则图像变为`Image`格式，这时可以应用上述操作。

### 图片的变换
##### 图片的缩放
对于`opencv`读取的图片，即`numpy`格式，图片的剪裁可以直接用`array`的切片实现，而如果要实现缩放，`opencv`提供了相应的函数进行操作<br>
`img_re = cv.resize(img, (dst_width, dst_height), fx, fy, interpolation(INTER_LINEAR))`<br>
若需要对Image格式图片进行缩放，可以使用`trochvision.transforms`中的`Resize`进行。

参考连接：<br>
https://blog.csdn.net/formatfa/article/details/80353235<br>
https://blog.csdn.net/li_l_il/article/details/83218838

### 关于训练结果的对比输出
在对网络进行训练之后，我们除了获得树数值上的结果，也需要产生一些可视化的结果。而且为了方便对比，常常需要将多组数据的结果一块显示，这时候可以使用pil来生成组合图片。<br>
首先使生成一张所需大小的背景图片
```
import PIL.Image as pil
new_img = pil.new('RGB', (pic_num*image_size[0] + 600, (model_num + 1)*image_size[1] + 150), (255, 255, 255))
```
之后，将所需的图片贴在对应位置即可
```
new_img.paste(im, (600 + idx * original_width, 150 + original_height + w_idx * original_height))
```
如果需要添加文字，则用相应模块可以完成
```
from PIL import ImageDraw
import PIL.ImageFont as ImageFont

draw = ImageDraw.Draw(new_img) 
font = ImageFont.truetype('LiberationSans-Regular.ttf', 60)

draw.text((50,150 + original_height / 2 + w_idx * original_height),\
            args.net_list[index]+'\nepoch '+weights.split('_')[-1] , fill=(0,0,0), font=font)
```

参考链接：<br>
https://www.jb51.net/article/154638.htm<br>
https://blog.csdn.net/litao_243/article/details/79913379

### 关于色彩映射
在进行数据的可视化时，常用将数值进行色彩映射来达到更直观的显示效果，OpenCV自带该功能。可以使用`cv.applyColorMap(input_img, cv.COLORMAP_XXX)`来实现自动的色彩映射，常用的映射方式有`RAINBOW`，`JET`等，更多的映射方式见参考链接。

参考链接：<br>
https://blog.csdn.net/guduruyu/article/details/68925416
