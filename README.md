# DMIT ping 测试完整指南：三大机房延迟数据 + 线路对比，下单前怎么测才准？附所有套餐价格与当前优惠

搜 DMIT 的人大多卡在同一个问题上——知道它贵，知道它做 CN2 GIA，但不知道对自己的网络环境延迟到底是多少。看别人测评没用，线路质量这种东西，你所在的城市、你用的运营商，差一个变量结论就能完全不同。

所以这篇文章要做两件事：第一，把 DMIT 三个机房的测试 IP 和测速工具整整齐齐地列出来，让你自己动手测；第二，把延迟背后的线路逻辑说清楚，让你知道数字代表什么意思，好不好，好在哪里。

---

## DMIT 是什么，为什么 ping 值这么受关注

DMIT 是 2018 年开始运营的 VPS 服务商，纽约注册，主要经营洛杉矶、香港、东京三个数据中心。做到现在没有靠大促出圈，靠的是一点：认真对待到中国大陆的网络路由。

这家做的是 CN2 GIA、CMIN2、AS9929 这些精品线路。这些词在其他商家嘴里是营销词汇，DMIT 是真的用在了线路上。路由追踪能看到 59.43 开头的 IP 段——那是 CN2 GIA 的标志，不是商家随便贴上去的标签。

正因为如此，DMIT ping 测试在国内 VPS 圈讨论热度一直不低。买之前不测，下单就是在赌，而 DMIT 的价格不低，赌不起。

---

## 三大机房测试 IP 与 Looking Glass

### 测试 IP 速查表

| 机房 | 线路系列 | 测试 IP | 说明 |
|------|----------|---------|------|
| 洛杉矶 LAX | Premium（CN2 GIA） | 154.17.2.2 | 三网 CN2 GIA 双向优化 |
| 洛杉矶 LAX | Eyeball（CMIN2） | 154.17.226.2 | 电信联通 AS9929，移动 CMIN2 |
| 香港 HKG | Premium（CN2 GIA） | 103.117.100.2 | 三网 CN2 GIA，延迟极低 |
| 香港 HKG | Eyeball（CMI） | 154.3.33.110 | 三网 CMI 优化 |
| 东京 TYO | Premium（CN2 GIA） | 169.197.142.2 | 三网 CN2 GIA 回程 |

### Looking Glass 路由工具

DMIT 官方提供 Looking Glass 工具，地址是 **lg.dmit.sh**，可以按机房选节点，查看路由路径。这个比 ping 更直观——路由追踪能告诉你数据包从机房出发怎么到你手里的，有没有绕路，走的是什么线路。

看路由重点盯一个东西：回程有没有出现 59.43 开头的 IP 段。出现了，就是 CN2 GIA 落地，是真的；没有，不管商家怎么写，都不是纯正 GIA。

---

## 怎么测才准：ping 测试操作步骤

### Windows 系统

1. 按 Win + R，输入 `cmd` 打开命令提示符
2. 输入命令：`ping 154.17.2.2 -n 20`（-n 20 表示发送 20 次，更准确）
3. 看最后一行：平均延迟多少 ms，丢包率是多少

### macOS / Linux

1. 打开终端
2. 输入：`ping -c 20 154.17.2.2`
3. 看输出结果里的 avg（平均值）和 packet loss（丢包率）

### 手机端

iOS 可以用 Network Analyzer 或 Ping & Net Tools；安卓随便搜一个 ping 工具都行，输入上面的测试 IP 跑一下。

### 测出来的数字怎么看

**延迟参考（以洛杉矶为例，国内用户）：**

- 120–150ms：很好，CN2 GIA 正常水平
- 150–180ms：正常范围，大部分场景够用
- 180–220ms：稍高，可能受当地网络环境影响
- 超过 250ms：建议换时间段再测，或检查自己的出口线路

丢包率才是真正的评判标准。延迟看起来好看，丢包 10%，网页转圈转到死，什么都白搭。DMIT 实测晚高峰丢包率极低，这一点在同价位里比较难得。

---

## 三机房延迟对比：数字背后的逻辑

### 香港机房

物理距离的极致优势。国内电信 ping 香港 Premium 节点，延迟在 10–30ms，基本跟国内机器没差别。

问题是代价。香港机房的带宽比洛杉矶贵不少，入门 TINY 套餐 $39.90/月，同等配置是洛杉矶的四倍。适合对延迟极度敏感的场景，比如游戏加速、实时 API、低延迟交易系统。

### 洛杉矶机房

延迟和带宽的最佳平衡点，也是 DMIT 销量最高的机房。

隔着太平洋，物理距离摆在那，延迟在 130–160ms 之间是正常水平。但 CN2 GIA 的价值不在低延迟，在稳定——晚高峰同样保持稳定，不会出现白天 120ms 晚上暴涨到 300ms 的情况。实测上海电信平均延迟 129ms，全国平均 158ms，丢包率基本维持在极低水平。

美国带宽也便宜，同样的价格，洛杉矶能买到更大的流量配额。

### 东京机房

延迟介于香港和洛杉矶之间，大概在 50–80ms。电信用户通常 64ms 左右，联通 52ms，移动 54ms，接近香港 GIA 的体验，但价格比香港便宜一档。

需要日本原生 IP 的场景，或者面向日本用户的业务，东京是合理的选择。

---

## DMIT 线路分级：Premium / Eyeball / Tier 1 有什么区别

搞清楚这三档，才能看明白为什么同样是洛杉矶，价格差这么多。

**Premium（Pro 系列）**：旗舰档。电信走 CN2 GIA，联通走 AS9929，移动走 CMI/CMIN2，回程三网全走 CN2 GIA。适合对延迟和稳定性有强需求的用户。

**Eyeball 系列**：性价比档。电信联通去程走 CN2/AS9929，三网回程走 CMIN2。比 Premium 便宜，但晚高峰的稳定性稍弱一些，不是不好，是差一个档次。

**Tier 1 系列**：国际线路，不含中国大陆专属优化。适合面向海外用户建站、做 CDN 源站、需要大流量配额但不需要大陆优化的场景。价格最便宜，流量配额也最大。

说句题外话：新手很容易在这里踩坑。香港 T1 不等于香港延迟，T1 是纯国际路由，电信联通回程会绕路到日本甚至美国，延迟比洛杉矶 Premium 还高。买之前先测试 IP，再确认线路。

---

## DMIT 完整套餐价格对比表

### 洛杉矶 - Premium 系列（CN2 GIA）

**AN4 平台：**

| 套餐 | CPU | 内存 | 硬盘 | 流量 | 带宽 | 价格 | 购买 |
|------|-----|------|------|------|------|------|------|
| TINY | 1核 | 2GB | 20GB | 1000GB | 1Gbps | $88.88/年 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=237) |
| Pocket | 2核 | 2GB | 40GB | 1500GB | 4Gbps | $159.98/年 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=238) |
| STARTER | 2核 | 2GB | 80GB | 3000GB | 10Gbps | $29.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=239) |
| MINI | 4核 | 4GB | 80GB | 5000GB | 10Gbps | $58.88/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=240) |
| MICRO | 4核 | 4GB | 160GB | 7000GB | 10Gbps | $74.99/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=241) |
| MEDIUM | 6核 | 8GB | 160GB | 15000GB | 10Gbps | $168.88/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=242) |
| LARGE | 8核 | 16GB | 320GB | 25000GB | 10Gbps | $338.88/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=243) |
| GIANT | 12核 | 24GB | 640GB | 50000GB | 10Gbps | $619.99/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=244) |

**AN5 平台（最新 EPYC 9005）：**

| 套餐 | CPU | 内存 | 硬盘 | 流量 | 带宽 | 价格 | 购买 |
|------|-----|------|------|------|------|------|------|
| TINY | 1核 | 2GB | 20GB | 1000GB | 1Gbps | $119.99/年 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=100) |
| Pocket | 2核 | 2GB | 40GB | 1500GB | 4Gbps | $203.90/年 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=137) |
| STARTER | 2核 | 2GB | 80GB | 3000GB | 10Gbps | $38.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=56) |
| MINI | 4核 | 4GB | 80GB | 5000GB | 10Gbps | $76.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=58) |
| MICRO | 4核 | 4GB | 160GB | 7000GB | 10Gbps | $99.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=81) |
| MEDIUM | 6核 | 8GB | 160GB | 15000GB | 10Gbps | $219.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=82) |

### 洛杉矶 - Eyeball 系列（CMIN2）

**AN4 平台：**

| 套餐 | CPU | 内存 | 硬盘 | 流量 | 带宽 | 价格 | 购买 |
|------|-----|------|------|------|------|------|------|
| TINY | 1核 | 2GB | 20GB | 1500GB | 2Gbps | $88.88/年 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=245) |
| Pocket | 2核 | 2GB | 40GB | 3000GB | 4Gbps | $159.98/年 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=246) |
| STARTER | 2核 | 2GB | 80GB | 5000GB | 10Gbps | $29.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=247) |
| MINI | 4核 | 4GB | 80GB | 10000GB | 10Gbps | $58.88/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=248) |
| MICRO | 4核 | 4GB | 160GB | 14000GB | 10Gbps | $74.99/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=249) |
| MEDIUM | 6核 | 8GB | 160GB | 30000GB | 10Gbps | $168.88/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=250) |
| LARGE | 8核 | 16GB | 320GB | 50000GB | 10Gbps | $338.88/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=251) |
| GIANT | 12核 | 24GB | 640GB | 100000GB | 10Gbps | $619.99/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=252) |

**AN5 平台（EPYC 9005）：**

| 套餐 | CPU | 内存 | 硬盘 | 流量 | 带宽 | 价格 | 购买 |
|------|-----|------|------|------|------|------|------|
| TINY | 1核 | 2GB | 20GB | 1500GB | 2Gbps | $119.99/年 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=189) |
| Pocket | 2核 | 2GB | 40GB | 3000GB | 4Gbps | $203.90/年 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=190) |
| STARTER | 2核 | 2GB | 80GB | 5000GB | 10Gbps | $38.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=191) |
| MINI | 4核 | 4GB | 80GB | 10000GB | 10Gbps | $76.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=192) |
| MICRO | 4核 | 4GB | 160GB | 14000GB | 10Gbps | $99.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=193) |
| MEDIUM | 6核 | 8GB | 160GB | 30000GB | 10Gbps | $219.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=194) |

### 洛杉矶 - Tier 1 系列（国际线路）

**AN5 VOLUME 大流量：**

| 套餐 | CPU | 内存 | 硬盘 | 流量 | 带宽 | 价格 | 购买 |
|------|-----|------|------|------|------|------|------|
| V2C2G | 2核 | 2GB | 40GB | 5000GB | 10Gbps | $14.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=169) |
| V2C4G | 2核 | 4GB | 80GB | 10000GB | 10Gbps | $23.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=170) |
| V4C4G | 4核 | 4GB | 120GB | 20000GB | 10Gbps | $36.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=171) |
| V4C8G | 4核 | 8GB | 160GB | 40000GB | 10Gbps | $52.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=180) |
| V8C16G | 8核 | 16GB | 240GB | 80000GB | 10Gbps | $119.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=172) |
| V12C24G | 12核 | 24GB | 320GB | 160000GB | 10Gbps | $199.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=173) |

**AN4 GENERAL + 低配年付：**

| 套餐 | CPU | 内存 | 硬盘 | 流量 | 价格 | 购买 |
|------|-----|------|------|------|------|------|
| WEE | 1核 | 1GB | 20GB | 1000GB | $36.90/年 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=71) |
| TINY | 1核 | 1GB | 20GB | 2000GB | $6.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=116) |
| STARTER | 2核 | 2GB | 40GB | 4000GB | $12.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=117) |
| MINI | 2核 | 4GB | 80GB | 8000GB | $21.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=118) |
| MICRO | 4核 | 4GB | 120GB | 16000GB | $32.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=119) |

### 香港 - Premium 系列（CN2 GIA）

| 套餐 | CPU | 内存 | 硬盘 | 流量 | 带宽 | 价格 | 购买 |
|------|-----|------|------|------|------|------|------|
| TINY | 1核 | 1GB | 20GB | 500GB | 1Gbps | $39.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=123) |
| STARTER | 1核 | 2GB | 40GB | 1000GB | 1Gbps | $79.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=124) |
| MINI | 2核 | 2GB | 60GB | 1500GB | 1Gbps | $119.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=125) |
| MICRO | 4核 | 4GB | 80GB | 2000GB | 1Gbps | $159.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=126) |
| MEDIUM | 4核 | 8GB | 160GB | 2500GB | 1Gbps | $179.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=127) |
| LARGE | 8核 | 16GB | 320GB | 3000GB | 1Gbps | $239.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=128) |
| GIANT | 8核 | 24GB | 640GB | 6000GB | 1Gbps | $499.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=129) |

### 香港 - Eyeball 系列（CMI）

| 套餐 | CPU | 内存 | 硬盘 | 流量 | 带宽 | 价格 | 购买 |
|------|-----|------|------|------|------|------|------|
| TINYv2 | 1核 | 1GB | 20GB | 1000GB | 1Gbps | $29.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=210) |
| STARTERv2 | 1核 | 2GB | 40GB | 2000GB | 2Gbps | $59.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=211) |
| MINIv2 | 2核 | 2GB | 60GB | 3000GB | 2Gbps | $89.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=212) |
| MICROv2 | 4核 | 4GB | 80GB | 4000GB | 4Gbps | $129.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=213) |
| MEDIUMv2 | 4核 | 8GB | 160GB | 6000GB | 4Gbps | $199.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=214) |
| LARGEv2 | 8核 | 16GB | 320GB | 12000GB | 4Gbps | $389.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=215) |
| GIANTv2 | 8核 | 24GB | 640GB | 24000GB | 4Gbps | $789.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=216) |

### 香港 - Tier 1 系列（国际线路，无大陆优化）

| 套餐 | CPU | 内存 | 硬盘 | 流量 | 价格 | 购买 |
|------|-----|------|------|------|------|------|
| WEE | 1核 | 1GB | 20GB | 1000GB | $36.90/年 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=197) |
| TINY | 1核 | 1GB | 20GB | 2000GB | $6.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=198) |
| STARTER | 1核 | 2GB | 40GB | 4000GB | $12.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=199) |
| MINI | 2核 | 2GB | 60GB | 8000GB | $21.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=200) |
| MICRO | 4核 | 4GB | 80GB | 16000GB | $32.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=201) |
| MEDIUM | 4核 | 8GB | 160GB | 32000GB | $49.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=202) |
| LARGE | 8核 | 16GB | 320GB | 64000GB | $99.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=203) |
| GIANT | 8核 | 24GB | 640GB | 128000GB | $199.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=204) |

### 日本东京 - Premium 系列（CN2 GIA）

| 套餐 | CPU | 内存 | 硬盘 | 流量 | 带宽 | 价格 | 购买 |
|------|-----|------|------|------|------|------|------|
| TINY | 1核 | 1GB | 20GB | 500GB | 1Gbps | $21.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=138) |
| STARTER | 1核 | 2GB | 40GB | 1000GB | 1Gbps | $39.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=139) |
| MINI | 2核 | 2GB | 60GB | 2000GB | 1Gbps | $79.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=140) |
| MICRO | 4核 | 4GB | 80GB | 4000GB | 1Gbps | $159.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=141) |
| MEDIUM | 4核 | 8GB | 160GB | 5000GB | 1Gbps | $259.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=142) |
| LARGE | 8核 | 16GB | 320GB | 8000GB | 1Gbps | $429.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=143) |
| GIANT | 8核 | 24GB | 640GB | 15000GB | 1Gbps | $799.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=144) |

### 日本东京 - Eyeball 系列（CMI）

| 套餐 | CPU | 内存 | 硬盘 | 流量 | 带宽 | 价格 | 购买 |
|------|-----|------|------|------|------|------|------|
| TINY | 1核 | 1GB | 20GB | 1000GB | 1Gbps | $25.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=221) |
| STARTER | 1核 | 2GB | 40GB | 2000GB | 2Gbps | $55.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=222) |
| MINI | 2核 | 2GB | 60GB | 3000GB | 2Gbps | $85.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=223) |
| MICRO | 4核 | 4GB | 80GB | 4000GB | 4Gbps | $119.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=224) |
| MEDIUM | 4核 | 8GB | 160GB | 6000GB | 4Gbps | $179.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=225) |
| LARGE | 8核 | 16GB | 320GB | 12000GB | 4Gbps | $369.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=226) |
| GIANT | 8核 | 24GB | 640GB | 24000GB | 4Gbps | $749.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=227) |

### 日本东京 - Tier 1 系列

| 套餐 | CPU | 内存 | 硬盘 | 流量 | 价格 | 购买 |
|------|-----|------|------|------|------|------|
| WEE | 1核 | 1GB | 20GB | 1000GB | $36.90/年 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=228) |
| TINY | 1核 | 1GB | 20GB | 2000GB | $6.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=131) |
| STARTER | 1核 | 2GB | 40GB | 4000GB | $12.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=132) |
| MINI | 2核 | 2GB | 60GB | 8000GB | $21.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=133) |
| MICRO | 4核 | 4GB | 80GB | 16000GB | $32.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=134) |
| MEDIUM | 4核 | 8GB | 160GB | 32000GB | $49.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=135) |
| LARGE | 8核 | 16GB | 320GB | 64000GB | $99.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=136) |
| GIANT | 8核 | 24GB | 640GB | 128000GB | $199.90/月 |  [立即选购](https://www.dmit.io/aff.php?aff=13832&pid=229) |

> 以上价格以官网实时显示为准，库存随时变化，部分热门套餐会缺货。

👉 [查看 DMIT 所有在售套餐与当前库存](https://www.dmit.io/aff.php?aff=13832)

---

## ping 数据好看，能说明什么？又说明不了什么？

说实话，ping 值是个门槛指标，不是最终指标。

看 DMIT ping 测试，你能判断的是：这个机房的物理路由有没有绕路，线路基础好不好。洛杉矶 CN2 GIA 跑出 130ms，说明路由是走直线的，没有乱绕。这是前提条件。

但光看 ping 不够。网络质量真正的考验是晚高峰——晚上 9 点到 11 点，大量用户同时在线，这时候普通线路就开始抽风了。有些商家白天 ping 值漂亮，一到晚高峰延迟翻倍，丢包飙升。DMIT 不超售这一点在这个场景下最明显——实测晚高峰延迟基本稳定，没有大幅跳动。

还有一个维度：带宽打满之后的处理方式。DMIT 流量用完不关机，而是降速。WEE 套餐限速到 50Mbps，TINY 到 MINI 限速 100–200Mbps，MICRO 以上 200–1Gbps。降速之后还能用，对大多数场景来说不至于直接断服务。

---

## 当前可用优惠码整理

这些是在官方活动页和产品页面可以查证的促销码，月付通常不适用，需要季付或年付才能触发：

- **LAX EB 系列**：`LAX-EB-LAUNCH-NON-MONTHLY-RECURRING-20OFF`，洛杉矶 Eyeball 季付及以上享 20% 永久折扣
- **东京 T1 系列**：`2025-TYO-T1-HI-GSL-NON-MONTHLY-30OFF`，东京 Tier 1 季付/年付享 30% 永久折扣
- **香港 T1 年付**：`HKG-T1-ANNUALLY-45OFF-RECUR`，香港 Tier 1 年付享 45% 永久折扣，附赠升级配置（更多 vCPU、双倍存储）

在结算页面的优惠码栏填入后点击应用，不会自动生效。用之前建议在官网核验是否仍然有效。

---

## 根据自己需求选套餐：直接给结论

**主要服务国内用户，对延迟有要求**：洛杉矶 Eyeball TINY（$88.88/年）或洛杉矶 Premium TINY（$88.88/年），CN2 GIA 或 CMIN2 线路，晚高峰稳定。

**对延迟极度敏感（游戏、实时 API）**：香港 Premium 系列，延迟 10–30ms，价格从 $39.90/月起。贵，但这个延迟在跨境线路里基本找不到更低的了。

**做外贸或国际站，需要流量大、带宽高**：洛杉矶 T1 大流量系列，$14.90/月起，2核2G配置月流量 5TB，超额后降速继续跑，不断服务。

**需要日本 IP 或面向日本用户**：东京 Premium TINY，$21.90/月，三网 CN2 GIA 回程，延迟 50–80ms。

**摸底测试，不确定要不要买**：洛杉矶 T1 WEE，$36.90/年，1G 内存 1 核，够测线路基本情况，3 天内不满意全额退。

---

## FAQ

**Q：DMIT ping 测试 IP 是多少？**

A：洛杉矶 Premium 系列测试 IP 是 154.17.2.2，Eyeball 系列是 154.17.226.2；香港 Premium 是 103.117.100.2；东京 Premium 是 169.197.142.2。路由工具用 lg.dmit.sh，可以选机房查看详细路由路径。

**Q：DMIT 的 ping 值正常应该是多少？**

A：洛杉矶到国内大部分城市在 130–180ms 之间，香港在 10–30ms，东京在 50–80ms。这是 CN2 GIA 线路的正常水平。超过 200ms 建议换时间段再测，排除本地网络波动。

**Q：CN2 GIA 和普通线路 ping 差别有多大？**

A：光看延迟数字差距不一定明显，但实际使用差在稳定性上。普通 163 骨干网晚高峰容易拥堵，ping 值波动大、丢包率高；CN2 GIA 是专用线路，晚高峰同样保持稳定，这是关键差别。

**Q：DMIT 流量用完了会怎样？**

A：不会关机，降速继续跑。WEE 套餐限速 50Mbps，TINY 到 MINI 限速 100–200Mbps，MICRO 以上 200–1Gbps，具体看所购套餐规格。

**Q：DMIT 退款政策怎么样？**

A：购买 3 天内、流量使用不超过 30GB，可以申请全额退款。30 天内按剩余时间比例退还。对新用户来说，先买低配测试，感觉对了再升级是个稳妥的方式。

**Q：IP 被封了怎么办？**

A：Premium 和 Eyeball 系列的 IP 被墙后，每 15 天可以免费换一次。超过频率是 $5 一次。目前支持用户自助换 IP，不需要提工单等客服。

---

测之前别光看别人的数据。你的运营商、你所在城市、你用的出口，这些变量叠在一起，结果可能跟别人的测评差很多。测试 IP 就在上面，自己 ping 一下，十秒钟的事，比看十篇评测都管用。

👉 [前往 DMIT 查看当前套餐与库存](https://www.dmit.io/aff.php?aff=13832)
