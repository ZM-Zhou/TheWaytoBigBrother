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

### 图片格式的变换
如果需要将PIL读取的Image格式变为np array格式，可以通过如下操作完成
```
np_im = np.array(pil_im)
pil_im = Image.fromarray(np.uint8(np_im))
```
需要注意的是。如果读取的图像是png格式，则`Image.open`读取产生的图像是`PngImagePlugin`格式（不经过`convert`变换），这时候无法直接通过上述操作转换图片类型，解决方法之一是用`Image.crop()`进行一次剪裁，则图像变为`Image`格式，这时可以应用上述操作。

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