# matplotlib

### 1.matplotlib.pyplot.imshow
Display an image, i.e. data on a 2D regular raster.
用于展示图片类数据
<https://matplotlib.org/3.1.1/api/_as_gen/matplotlib.pyplot.imshow.html>

    matplotlib.pyplot.imshow(X, cmap=None, norm=None, aspect=None, interpolation=None, alpha=None, vmin=None, vmax=None, origin=None,     extent=None, shape=<deprecated parameter>, filternorm=1, filterrad=4.0, imlim=<deprecated parameter>, resample=None, url=None, *,   data=None, **kwargs)
 
X:支持ndarray配列orPIL图片<br>
interpolation: <br><https://matplotlib.org/3.1.1/gallery/images_contours_and_fields/interpolation_methods.html><br>
对于Agg、ps和pdf后端，当大图像缩小时，interpolation = 'none' 工作得很好，而当小图像被放大时<br>,interpolation = 'nearest'则运行正常。
