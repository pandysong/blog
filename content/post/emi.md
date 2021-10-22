# 单独的地层

如果已经有“地”层，那么包地起不到什么作用

http://www.sigcon.com/Pubs/news/15_02.htm

反而保持一定的距离是有用的

# 双倍间距：

https://www.protoexpress.com/blog/7-pcb-design-tips-solve-emi-emc-issues/

模拟地和数字地之间必须提供两倍宽度。

```
差分信号的D-和D+之间保持平行 （倾斜匹配）。这些走线的长度必须相同。这些差分信
号在布线时彼此应足够靠近，以抑制共模噪声。
• 差分信号之间的间距必须小于等于一倍宽度。
• 任何差分对与任何其他信号之间的间距必须超过两倍宽度。
• 在差分对与其他信号之间可提供额外的地。
```

翻译：
http://murata.eetrend.com/article/2020-08/1003726.html

# 传输线阻抗匹配

时钟信号的阻抗如果不匹配的话，反射会比较大，导致辐射增大。

https://www.ti.com/lit/an/scaa031/scaa031.pdf?ts=1620014601050

我们可否用

在这个文章中：

https://www.nxp.com/docs/zh/application-note/AN4941.pdf

也提到的端接电阻的重要性

# 关于展频的科普文章

# 差分信号的一些误区


https://www.huaweicloud.com/articles/4bdfb91eeae7845088ac4bbfa9114ec3.html

正确的观点：
- 匹配线长比保持等距重要

# EMI失败案例分析

https://scholarsmine.mst.edu/cgi/viewcontent.cgi?article=2997&context=doctoral_dissertations
