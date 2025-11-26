# PerformanceAPI

Performance API 是一组用于衡量 Web 应用性能的标准 API。允许开发者读取页面加载、资源获取、用户交互及 JavaScript 执行等性能数据。运用 Performance API 有助于将用户的主观体验量化为客户指标，建立以用户为中心的数据监控，便于后续对优化的评估和方案的制定。

## 性能记录标准 PerformanceEntry

Performance API 中各类性能记录的接口都是继承自 PerformanceEntry，PerformanceEntry 包含了最基础的 `name`、`entryType`、`startTime` 和 `duration` 4 个属性及一个 `toJSON()` 函数，其中最常用的三类性能记录接口是：

* PerformanceResourceTiming：获取各类资源加载相关的数据，但需要注意多个属性都受因同源策略限制，当存在不同源时需要添加响应头 `Timing-Allow-Origin: *`；
  * 资源名称：`name`
  * 资源类型：`initiatorType`，（`document`、`script`、`link`、`stylesheet`、`img`、`media`、`font`、`xhr`、`fetch`）
  * DNS 寻址耗时：`domainLookupStart` 和 `domainlookupEnd`
  * HTTP 请求开始：`requestStart`
  * HTTP 响应耗时：`responseStart` 和 `responseEnd`
  * 资源加载总耗时：`duration`
  * 响应体体积：`encodeBodySize`
  * 传输体积：`transferSize`
  * ...
* PerformanceEventTiming：
* VisibilityStateEntry：

![NavigationTimingLevel2.drawio (1)](https://raw.githubusercontent.com/OSpoon/ImageStorage/2024/uPic/NavigationTimingLevel2.drawio%20(1).png)