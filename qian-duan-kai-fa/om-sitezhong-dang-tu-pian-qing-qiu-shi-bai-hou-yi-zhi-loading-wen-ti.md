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

经过peter修改后，增加了isDataReady（数据是否获取结束）变量判断是改变loading状态。并且将没有完成加载的图片加入到队列中。下一次只检测这些没有完成的图片的状态。并且设置超时时间。

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

新的bug是因为可能会出现数据无法加载的情况。所以在refreshData方法的最后一行以及update文字的一行 添加isReadyData = true 标记数据已经加载完。代码如下

```
refreshData: function (e) { 
        var _this = this.self ? this.self : this;
        if (e.data && !e.data.error) {
            var tempDistributionsUpdated = {};
            for (var i = 0; i < e.data.length; i++) { 
                var item = _this.dictRefreshMap[e.data[i].name]; 
                if (item) {
                    if (item.pipelines) {
                        for (var j = 0; j < item.pipelines.length; j++) {
                            var pipline = _this.dictPipelines[item.pipelines[j]];
                            pipline.dictIdCom[e.data[i].name] = e.data[i].value == 1 ? true : false;
                        }
                    }
                    if (item.equipments) {
                        for (var j = 0; j < item.equipments.length; j++) {
                            var equipment = _this.dictEquipments[item.equipments[j]];
                            equipment.value = e.data[i].value;
                            equipment.update(null, null);
                        }
                    }
                    if (item.charts) {
                        for (var j = 0; j < item.charts.length; j++) {
                            _this.dictCharts[item.charts[j]].update(e.data[i].name, e.data[i].value);
                        }
                    }
                    if (item.gages) {
                        for (var j = 0; j < item.gages.length; j++) {
                            _this.dictGages[item.gages[j]].update(e.data[i].value);
                        }
                    }
                    //更新checkbox
                    if( item.checkboxs ){
                        for (var j = 0 ; j < item.checkboxs.length ; j++){
                            // if(!_this.options.bShowTimeShaft){
                                _this.dictCheckboxs[item.checkboxs[j]].update(e.data[i])
                            // }
                        }
                    }
                    if(item.buttons) {
                        for (var j = 0; j < item.buttons.length; j++) {
                           // console.log(e.data[i])
                            _this.dictButtons[item.buttons[j]].update(e.data[i])
                        }
                    }
                    if (item.tempDistributions) {
                        for (var j = 0; j < item.texts.length; j++) {
                            if (item.tempDistributions && item.tempDistributions.length > 0) {
                                if (!tempDistributionsUpdated[item.tempDistributions[j]]) {
                                    tempDistributionsUpdated[item.tempDistributions[j]] = [];
                                }
                                tempDistributionsUpdated[item.tempDistributions[j]].push(e.data[i]);
                                _this.dictTexts[item.texts[j]].update(e.data[i].constructor === Object && e.data[i].value ? e.data[i].value : '--', true);
                            }
                            else {
                                _this.dictTexts[item.texts[j]].update(e.data[i].constructor === Object && e.data[i].value ? e.data[i].value : '--');
                            }
                        }
                        _this.isDataReady = true; //新添加
                        continue
                    }

                    if (item.texts) {
                        for (var j = 0; j < item.texts.length; j++) {
                            _this.dictTexts[item.texts[j]].update(e.data[i].value);
                        }
                    }
                    if (item.rulers) {
                        for (var j = 0; j < item.rulers.length; j++) {
                            _this.dictRulers[item.rulers[j]].update(e.data[i].name, e.data[i].value);
                        }
                    }
                    
                }
            }
            // 防止同一个温度图多次绘制
            for (var tempId in tempDistributionsUpdated) {
                var tempUpdatedData = tempDistributionsUpdated[tempId];
                _this.dictTempDistributions[tempId].update(tempUpdatedData);
            }

            //TODO: to be removed;
            for (var pointName in _this.dictCharts) {
                var chart = _this.dictCharts[pointName];
                if (!chart.isRunning) {
                    chart.isRunning = true;
                    chart.renderChart(chart);
                }
            }
        } else {
            //new Alert(_this.container, Alert.type.danger, I18n.resource.code[e.data.error]).showAtTop(5000);
        }

        _this.isDataReady = true; //新添加
    },

```

