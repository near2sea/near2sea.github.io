---
layout: post
title: 授权苹果健康
date: '2018-10-29 17:56:18 +0800'
comments: true
categories: ios
abbrlink: 62352
---

## 简述
> HealthKit框架提供了一个结构，应用可以使用它来分享健康和健身数据。HealthKit管理从不同来源获得的数据，并根据用户的偏好设置，自动将不同来源的所有数据合并起来。应用还可以获取每个来源的原始数据，然后执行自己的数据合并。

> HealthKit另外提供了一个应用来帮助管理用户的健康数据。健康应用为用户展示HealthKit的数据。用户可以使用健康应用来查看、添加、删除或者管理其全部的健康和健身数据。用户还可以编辑每种数据类型的分享权限。

<!-- more -->

## 使用HealthKit特别注意
1. 应用不应该将HealthKit收集的数据用于广告或类似的服务。*注意，在使用HealthKit框架应用中可以插播广告，但是你不能使用HealthKit中的数据来服务广告。*

2. 在没有用户的明确允许下，你不能向第三方展示任何HealthKit收集的数据。即使用户允许，你也只能向提供健康或健身服务的第三方展示这些数据。

3. 你不能将HealthKit收集的数据出售给广告平台、数据代理人或者信息经销商。

4. 如果用户允许，你可以将HealthKit数据共享给第三方用于医学研究。注意是用户允许

5. 必须明确说明，你和你的应用会怎样使用用户的HealthKit数据。

## 应用中使用了HealthKit 上 App Store 特别注意
1. 一定要添加隐私政策网址链接,并注明健康数据使用的隐私相关条例。例如：
    * 在App里您设置身高、体重时，根据您之前是否允许授权权限，XXX将依据您给的权限是否把信息写入苹果健康应用。
    * 在App里同步计步、睡眠、心率数据时，根据您之前是否允许授权权限，XXX将依据您给的权限是否把信息写入苹果健康应用。
2. 应用介绍中一定要注明：此版本支持你使用Apple健康应用程序
    * 审核详情请参考 App Store Review Guidelines中的HealthKit章节。

## HealthKit 存储理念

> 框架大量使用了子类化，在相似的类间创建层级关系。通常这些类间都有一些细微但是重要的差别。还有不少和它相关的类，需要正确搭配,才能一起工作。存储在HealthKit中的数据都是由对象和对象所属类型组成,这个概念一定要刻入脑海,你才能理解整个存储结构。所有对象都是基于[HKObject](https://developer.apple.com/documentation/healthkit/hkobject#//apple_ref/occ/cl/HKObject)，所有对象所属的类型都是基于[HKObjectType](https://developer.apple.com/documentation/healthkit/hkobjecttype#//apple_ref/occ/cl/HKObjectType)。下面简单介绍下这两个类:

* `HKObject`我们不能直接使用,而是使用它的子类。子类有:
    * 样本对象是某个特定时间断的数据。所有的样本对象都是`HKSample`的子类。它们都有下列属性：
        * `sampleType`: 样本类型。例如：一个睡眠分析样本、一个身高样本或者一个计步样本。
        * `startDate` : 样本的开始时间。
        * `endDate` : 样本的结束时间。



## *注意*
bugly异常分析中看到，但项目的Info.plist文件已经添加了NSHealthUpdateUsageDescription，但还是会报错。
```
NSHealthUpdateUsageDescription must be set in the app's Info.plist in order to request write authorization for the following types: HKQuantityTypeIdentifierBodyMassIndex, HKQuantityTypeIdentifierActiveEnergyBurned, HKWorkoutTypeIdentifier
```
处理方法：

1. 先把Info.plist中的key值为NSHealthUpdateUsageDescription删掉
2. 重新Info.plist中添加NSHealthUpdateUsageDescription并加上文字描述，OK!

