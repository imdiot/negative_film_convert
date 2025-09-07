# Negative Film Convert

<div align="center">
    <a href="https://github.com/imdiot/negative_film_convert/releases/latest/download/negative.film.convert_PS.ccx">
        <img src="./static/images/download.png" width="200" height="50" alt="logo">
    </a>
</div>

# 简介

Photoshop插件，用于负片胶片校色，也可以作为负片处理的一个初始点。

![](./static/images/screenshot.jpg)

# 要求

- ~~Photoshop 24.2 或以上版本。~~

- Photoshop 最低版本可能是 24.5 或 25.0，原谅我做不了那么多兼容性测试。

# 安装

## 自动

双击 `.ccx` 文件自动唤起 `Adobe Creative Cloud` 安装。

## 手动

1. 将 `.ccx` 文件扩展名改为 `.zip`，解压缩。
2. 将解压缩后的文件夹复制到一下路径：
   - Windows: `C:\Users\用户名\AppData\Roaming\Adobe\UXP\Plugins\External`
   - Mac: `~/Library/Application Support/Adobe/UXP/Plugins/External`

# 线性 TIFF

为了达到比较好的转换效果，最好使用无色彩调整的16位线性 TIFF 格式。

Gamma 对后续处理理论上没有影响，并不强制要求线性。但常见的 sRGB、BT.709 等的 Gamma 是个分段函数，在翻拍曝光不足，直方图靠近甚至进入分段函数线性区域的时候，还是会有肉眼可见的影响。所以还是无脑线性 TIFF 更方便些。

部分扫描仪可通过扫描软件直接生成16位线性 TIFF，可参照 ColorPerfect 提供的大部分扫描仪线性 TIFF 操作方法。[https://www.colorperfect.com/scanning-slides-and-negatives/creating-linear-scans/](https://www.colorperfect.com/scanning-slides-and-negatives/creating-linear-scans/)

Hasselblad 3F 文件本质为多帧16位线性 TIFF。直接改后缀名使用。或使用 Open Make Tiff 抽出数据帧和嵌入 ICC Profile。

RAW 可通过转换生成 16位线性 TIFF。

# RAW 转 线性 TIFF

转换方式举例，可按需选择。

## Open Make Tiff

[https://github.com/imdiot/open_make_tiff](https://github.com/imdiot/open_make_tiff)

MakeTiff 的开源替代品。

(可选依赖) [Adobe DNG Converter](https://helpx.adobe.com/tw/camera-raw/using/adobe-dng-converter.html)

## MakeTiff

[MakeTiff 介绍](https://www.colorperfect.com/MakeTiff/)

ColorPerfect 提供的 RAW 线性转换工具。使用简单，拖入窗口即可。

需要 ColorPerfect 许可证。

(可选依赖) [Adobe DNG Converter](https://helpx.adobe.com/tw/camera-raw/using/adobe-dng-converter.html)

## darktable

[https://www.darktable.org](https://www.darktable.org)

![](./static/images/darktable.jpg)

(可选) Adobe DNG Converter 基础转换

关闭所有色彩优化调整。

导入色彩档案文件 - 输入配置文件 - sRGB(不知道应该选什么的一律sRGB)

同步修改历史

选择合适的导出色彩空间导出16位 TIFF

## RawTherapee

[https://www.rawtherapee.com](https://www.rawtherapee.com)

![](./static/images/RawTherapee.jpg)

(可选) Adobe DNG Converter 基础转换

关闭曝光-色调曲线、白平衡、捕图加锐等所有色彩优化调整。

调整 ICM 设置(不知道应该选什么的一律sRGB)

同步配置

批量导出16位 TIFF

## LibRaw 或 Dcraw

[https://www.libraw.org](https://www.libraw.org)

[https://dechifro.org/dcraw](https://dechifro.org/dcraw)

1. (可选) Adobe DNG Converter 基础转换
2. 转换 TIFF
   -  LibRaw `dcraw_emu -v -r 1 1 1 1 -H 1 -o 0 -4 -T raw文件`
   -  Dcraw `dcraw -v -r 1 1 1 1 -H 1 -o 0 -4 -T raw文件`
3. (可选) exiftools 复制 EXIF 信息。

## 其他方式生成线性 TIFF

 - Capture One 等，但由于这种常规 RAW 解码目的是为了生成好看的图片，所以会加入很多色彩调整，生成的线性 TIFF 理论上反而不合适负片校色使用。

# 使用方法

## 单张校色

1. 去除无用区域

    - 方式一：直接裁剪掉无用区域。适合不需要无用区域的情况。
    - 方式二：配合复制图层、隐藏原始图层等功能，将无用区域处理为透明区域。适合需要保留无用区域的情况。
2. 点击转换。
3. 对图层组进行微调。
4. 导出。

## 多张合并校色

    与单张校色区别：
    - 校色流程与应用校色流程分离。
    - 数码化参数需要保持一致。
    - 对于整卷或同型号同时冲洗的整卷图像，有更好的一致性。
    - 多张合并相对于单张会有更少的颜色缺失，有更好的准确性。

1. 通过 Photoshop 中的联系表或其他功能，将多张图片合并为一张。分辨率不宜过大。
2. 去除无用区域

    - 方式一：直接裁剪掉无用区域。适合不需要无用区域的情况。
    - 方式二：配合复制图层、隐藏原始图层等功能，将无用区域处理为透明区域。适合需要保留无用区域的情况。
3. 点击转换。
4. 对图层组进行微调。
5. 通过 Photoshop 的动作录制、自动化批处理等功能，将校色图层组应用到各个图片。

# 图层功能

- multipliers: 基础的右侧缩放，对数码化时曝光不足的会有一定改善。
- log invert: LOG 转换。会根据计算使用相对合适的的曲线。
- normalization: 对齐通道。
- shadows wb: 暗部白平衡调整。
- highlights wb: 亮部白平衡调整。
- channel mixer: 一定程度上模拟负片色域。
  
# 注意事项

- 非开箱即用，需要根据实际情况调整。
- 剪裁掉非底片区域，减少对计算的干扰。
- 翻拍时尽量向右曝光
- 调整曝光尽量在 tone mapping 之前的对数域中调整，最好只使用曲线工具的直线和单点曲线调整。
- 因未考虑使用完全曝光区域，所以在图像中有完全曝光区域时会色彩不正确。
- 尽量使用颜色比较中性的图像作为基础的转换、调整图像。
- 可将同种、同时冲洗的多张图片合并为一张，以减少误差。
- 调整满意后，可使用 Photoshop 的导出图层、动作、批处理等批量转换。