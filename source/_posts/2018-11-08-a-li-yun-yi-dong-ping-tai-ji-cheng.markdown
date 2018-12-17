---
layout: post
title: 阿里云移动平台集成
date: '2018-11-08 12:00:12 +0800'
comments: true
categories: ios
abbrlink: 27481
---

## 移动数据分析
* 获取[alicloud-ios-demo](https://github.com/aliyun/alicloud-ios-demo)工程源码例子
* 参考[移动数据分析API文档](https://help.aliyun.com/document_detail/30035.html)

* 代码示例

```objective-c
ALBBMANPageHitBuilder *pageHitBuilder = [[ALBBMANPageHitBuilder alloc] init];
// 设置页面refer
[pageHitBuilder setReferPage:@"pageRefer"];
// 设置页面名称
[pageHitBuilder setPageName:@"pageName"];
// 设置页面停留时间
[pageHitBuilder setDurationOnPage:100];
// 设置页面事件扩展参数
[pageHitBuilder setProperty:@"pagePropertyKey1" value:@"pagePropertyValue1"];
[pageHitBuilder setProperty:@"pagePropertyKey2" value:@"pagePropertyValue2"];
ALBBMANTracker *tracker = [[ALBBMANAnalytics getInstance] getDefaultTracker];
// 组装日志并发送
[tracker send:[pageHitBuilder build]];
```

* 页面埋点与页面事件将影响控制台【页面路径分析】、【关键漏斗】、【控件点击】、【页面留存】等指标的报表展现，页面路径如下图所示。
{% img fancybox fancybox /images/2018/ios/page_path.png %}