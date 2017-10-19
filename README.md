<p align="center">
<img src="https://github.com/MQZHot/DaisyNet/raw/master/Picture/logo.png" alt="DaisyNet" title="DaisyNet" width="408"/>
</p>

<p align="center">
<img src="https://img.shields.io/badge/platform-iOS-yellow.svg">
<img src="https://img.shields.io/badge/language-swift-red.svg">
<img src="https://img.shields.io/badge/support-swift%204%2B-green.svg">
<img src="https://img.shields.io/badge/support-iOS%208%2B-blue.svg">
<img src="https://img.shields.io/badge/license-MIT%20License-brightgreen.svg">
</p>

* 对Alamofire与Cache的封装实现对网络数据的缓存，可以存储JSON，String，Data，接口简单明了.
* 封装Alamofire下载，使用更方便.
* 如有问题，欢迎提出，不足之处，欢迎纠正，欢迎star ✨✨✨✨✨✨✨✨✨✨✨✨✨✨✨✨✨✨✨✨

![image](https://github.com/MQZHot/DaisyNet/raw/master/Picture/get.gif) ![image](https://github.com/MQZHot/DaisyNet/raw/master/Picture/download.gif)

## 使用

### 1. 网络请求

* 缓存数据只需要调用`.cache(true)`，不调用或者`.cache(false)`则不缓存
* 调用`responseCacheAndString`可以先读取缓存数据，再读取网络数据
* 通过`isCacheData`属性可以区分缓存数据还是网络数据
```swift
DaisyNet.request(url, params: params).cache(true).responseCacheAndJson { value in
    switch value.result {
    case .success(let json):
        if value.isCacheData {
            print("我是缓存的")
        } else {
            print("我是网络的")
        }
    case .failure(let error):
        print(error)
    }
}
```

* 你也可以分别读取缓存数据和网络数据，如下代码
* 调用`cacheJson`方法获取缓存数据，调用`responseJson`获取网络数据

```swift
DaisyNet.request(url, params: params).cache(true).cacheJson { json in
        print("我是缓存的")
    }.responseJson { response in
    print("我是网络的")
}
```
* 如果你不需要缓存，可以直接调用`responseJson`方法
```swift
DaisyNet.request(url).responseString { response in
    switch response {
    case .success(let value): print(value)
    case .failure(let error): print(error)
    }
}
```

* 同理，如果你要缓存`Data`或者`String`，与`JSON`是相似的
```swift
/// 先读取缓存，再读取网络数据
DaisyNet.request(url).cache(true).responseCacheAndString { value in }
DaisyNet.request(url).cache(true).responseCacheAndData { value in }
```
```swift
/// 分别获取缓存和网络数据
DaisyNet.request(url).cache(true).cacheString { string in
        print("我是缓存的")
    }.responseString { response in
    print("我是网络的")
}
```
* 取消请求
```swift
DaisyNet.cancel(url, params: params)
```

* 清除缓存
```swift
/// 清除所有缓存
func removeAllCache(completion: @escaping (Bool)->())
/// 根据url和params清除缓存
func removeObjectCache(_ url: String, params: [String: Any]? = nil, completion: @escaping (Bool)->())
```

### 2. 下载

```swift
DaisyNet.download(url).downloadProgress { progress in
        /// 下载进度
    }.response { response in
    /// 下载完成
}
```
* 如果正在下载中退出当前界面，再次进入时可以通过以下方法获取下载进度，并改变UI
```swift
DaisyNet.downloadProgress(url) {
        print($0)
    }?.response(completion: { _ in
    print("下载完成")
})
```
* 获取下载状态
```swift
DaisyNet.downloadStatus(url)
```

* 获取下载百分比
```swift
DaisyNet.downloadPercent(url)
```

* 获取下载完成后文件所在位置
```swift
DDaisyNet.downloadFilePath(url)
```

* 删除某个下载
```swift
DaisyNet.downloadDelete(url)
```

* 取消某个下载
```swift
DaisyNet.downloadCancel(url)
```

## Author

* Email: mqz1228@163.com
* 简 书 : http://www.jianshu.com/u/9e39ec4000e9

## LICENSE

DaisyNet is released under the MIT license. See [LICENSE](https://github.com/MQZHot/DaisyNet/blob/master/LICENSE) for details.

