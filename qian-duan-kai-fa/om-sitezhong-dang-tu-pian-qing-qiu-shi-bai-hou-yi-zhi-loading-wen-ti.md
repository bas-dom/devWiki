# 问题

在om-site在现场会出现 ** 打开模态框一直loading的情况  **

# 分析

原来的代码如下：

```
 var _this = this
 var interval = setInterval(function (e) {
     var isCompleted = true;
     for (var key in _this.dictImages) {
         if (!_this.dictImages[key].complete) {
             clearInterval(interval);
             _this.hideLoading();
         }
     }

 },300)
```

经过分析，可能会出现图片加载失败的可能性。则图片的状态会一直处于false状态。也就会一直循环检测。不隐藏loading标志。

# 解决方案

最初的解决方案是通过增加临时变量，loadtimes记录加载时间，如果超过则认定失败，直接关闭loading。该方案的问题在于只要图片加载超过3秒后，就会隐藏loading，如果后续还有图片并没有开始加载，都会结束。如果在网速比较慢的时候可能会出现很多图片还没出现，就直接显示页面，用户体验不好。

```
 var loadtimes = 0 ;
 var _this = this
 var interval = setInterval(function (e) {
     var isCompleted = true;
     loadtimes++ ;
     for (var key in _this.dictImages) {
         if (!_this.dictImages[key].complete) {
             clearInterval(interval);
             _this.hideLoading();
         }
         if(loadtimes >= 10){
            clearInterval(interval); 
            _this.hideLoading();
         }
     }

 },300)
```

经过peter修改后，增加了isDataReady（数据是否获取结束）变量判断是改变loading状态。

```
var _this = this;
var interval = 500;
var IMAGE_LOAD_TIMEOUT = interval * 4;
var loadingImage = {
    key: null,
    loadtime: 0
};

//500ms检查一次
var timer = setInterval(function (e) {
    var isCompleted = true;
    for (var key in _this.dictImages) {
        if (key in imagesFailLoad) { 
            continue;
        }

        if (!_this.dictImages[key].complete) {
            if (loadingImage.key === key) { 
                loadingImage.loadtime += interval;
                if (loadingImage.loadtime >= IMAGE_LOAD_TIMEOUT) {
                    imagesFailLoad.push(key);
                    break;
                }
            } else {  
                loadingImage = {
                    key: key,
                    loadtime: 0
                }
            }
            isCompleted = false;
            break;
        }
    }
    if (isCompleted && _this.isDataReady) {
        clearInterval(timer);
        _this.hideLoading();  
    }
}, 500);
```

新的bug是因为虽然添加了isDataReady。可能也会出现数据无法加载的情况，所以在refreshData函数中多添加了一个isDataReady = true

