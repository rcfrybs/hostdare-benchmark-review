# HostDare 跑分实测深度评测：UnixBench、磁盘 I/O、延迟全项测试，CN2 GIA 套餐性能到底怎么样？哪个方案性价比最高？（附最新优惠码与全套餐价格对比）

---

有一类 VPS 用户，买机器之前一定要先看跑分——不是要拿去比赛，就是想心里有个底，知道自己花的钱买到的是什么档次的性能。

HostDare 这个牌子在 CN2 GIA 圈子里流传已久，进入国内用户视野的核心理由只有一个：在价格便宜这件事上，它把 CN2 GIA 这条线路做到了极致。但"网络好"和"性能好"是两件事，跑分才能说话。

这篇文章把 HostDare 的跑分数据整理齐了，顺带把套餐、优惠码、谁适合买一起说清楚，省得你再挨个帖子去翻。

---

## **HostDare 是什么，为什么值得跑分测一测**

HostDare 成立于 2015 年前后，机房主要在美国洛杉矶（Quadranet/CERA Networks 数据中心），另外有日本大阪（软银线路）和保加利亚两个节点。面向中国用户，旗舰产品是 CN2 GIA 优化线路的 VPS，覆盖电信 CN2 GIA（AS4809）、联通 AS9929、移动 CMIN2（AS58807）三大运营商的优化回程。

用 KVM 虚拟化，全根权限，非托管型，是那种"你自己会装系统就能用"的产品。

值不值得跑分？当然值。毕竟光看套餐参数说"1核512MB"，你不知道这颗核是什么年代的 CPU、磁盘是真 NVMe 还是品牌 NVMe 但共享严重、网络带宽是名义带宽还是实测跑得出来的。

下面直接上数据。

---

## **HostDare 跑分实测数据**

### **CPU 性能：Geekbench 6 跑分**

以 NKVM1（日本大阪，1 核 1GB RAM，Intel Broadwell 2593 MHz）为例，Geekbench 6 实测结果如下：

| 测试项 | 得分 |
| --- | --- |
| 单核得分 | **719** |
| 多核得分 | **523** |

> 原始报告：[Geekbench v6 #15314175](https://browser.geekbench.com/v6/cpu/15314175)

单核 719 这个数字放到 Geekbench 6 里头，大概对应 Intel Broadwell 架构的正常表现——不算新，不算差，日常跑个 Web 服务、轻量脚本没有问题，硬解 4K 视频这种事就别指望了。

对于洛杉矶 CN2 GIA 系列（CSSD/CKVM），有用户用 UnixBench 测过 CKVM 入门款（756MB 内存配置），综合得分约在 **737 分**左右。入门款内存压着，这个分数已经说明硬件资源分配比较诚实，没有严重超售的迹象。

另外，CAMD 和 CSSD 系列分别用 AMD EPYC 和 Intel Xeon 处理器，实测中 AMD 的单核跑分通常略高，CAMD 系列在同等配置下 CPU 性能更占优势一点。

---

### **磁盘 I/O：NVMe 磁盘读写跑分**

用 fio 对日本 NKVM1（NVMe SSD，25GB 容量）进行测试，结果如下：

| 块大小 | 顺序读 | 顺序写 | 混合读写 |
| --- | --- | --- | --- |
| 4K | 207 MB/s | 207 MB/s | 415 MB/s |
| 64K | 965 MB/s | 970 MB/s | 1,936 MB/s |
| 512K | 1,131 MB/s | 1,191 MB/s | 2,323 MB/s |
| 1M | 1,135 MB/s | 1,211 MB/s | 2,347 MB/s |

大块顺序读写直接跑到 **1.1 GB/s 以上**，4K 随机的 IOPS 也在 51,000+ 左右。这个成绩放在这个价位的 VPS 里头，属于真正的 NVMe 该有的表现，不是挂着 NVMe 名字但实际上是 SATA 的那种。

对于洛杉矶节点，早期有测评显示磁盘速度约 158 MB/s（CKVM HDD 系列）。HDD 系列和 NVMe 系列差距明显，如果你的业务对磁盘 I/O 有要求，优先选 CSSD 或 CAMD 系列，不要选 CKVM。

---

### **网络延迟：CN2 GIA 到国内实测**

HostDare CN2 GIA 测速 IP：`185.186.146.8`

晚高峰三网延迟实测（洛杉矶至中国大陆）：

| 线路 | 平均延迟 | 最快节点 |
| --- | --- | --- |
| **全网平均** | 176.6 ms | 上海 128.7 ms |
| 电信 CN2 GIA | **171.3 ms** | 上海天翼 128.7 ms |
| 联通直连 | 179.3 ms | 杭州 156.6 ms |
| 移动直连 | 203.1 ms | 江苏 138.4 ms |
| 华东地区 | 171.6 ms | 上海 128.7 ms |
| 华南地区 | 171.8 ms | 广州 150.9 ms |
| 华北地区 | 179.3 ms | 石家庄 156.4 ms |

洛杉矶到国内 170ms 出头是这个距离的正常水平，CN2 GIA 的价值在于**稳定**，不是延迟低到不合理。高峰时段晚上 8 点，电信 CN2 GIA 依然稳在 171ms，相比走普通线路动不动飙到 300ms+ 的情况好得多。

路由方面，实测结果是电信和联通去程均走 CN2 GIA（AS4809），移动去程直连——三网都不绕路，这个配置在同价位的美国 VPS 里已经属于顶配。

---

### **网络带宽：iperf3 多点测试**

以日本 NKVM1 为例，iperf3 下载速度（8 线程）：

| 测试节点 | 延迟 | 下载速度 |
| --- | --- | --- |
| Singapore（10G） | 67.7 ms | 287 Mbits/s |
| Amsterdam（100G） | 222 ms | 377 Mbits/s |
| Los Angeles（10G） | 104 ms | 54.9 Mbits/s |
| NYC（10G） | 205 ms | 137 Mbits/s |

新加坡方向跑出 287 Mbps，考虑到 NKVM1 的套餐带宽是 500 Mbps，实测能跑到一半以上，表现正常。

---

## **跑分的结论是什么**

说白了，HostDare 的跑分表现处于这个价位段的**中上水平**：

- NVMe 磁盘是真 NVMe，不是挂牌货
- CPU 性能中规中矩，够用，不出众
- 网络稳定性是最大优势，CN2 GIA 三网优化是实打实的
- 不适合对 CPU 计算要求极高的场景（高并发计算、大规模编译）
- 非常适合建站、代理、轻量应用、面向中国用户的服务

有人说 HostDare 是"网络型选手"——跑分不是最亮眼，但只要你的核心诉求是国内访问快、稳，它就是合适的选择。

---

## **全套餐价格对比表**

HostDare 目前在售的 VPS 产品线共有以下几个系列，按用途从优化线路到普通线路排列。

### **🔥 CN2 GIA 优化线路 — CSSD 系列（Intel NVMe，旗舰推荐）**

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 年付价格 | 购买 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| CSSD0 | 1核 | 768 MB | 10 GB | 250 GB | 30 Mbps | $35.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=112) |
| CSSD1 | 1核 | 1 GB | 25 GB | 600 GB | 50 Mbps | $55.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=106) |
| CSSD2 | 2核 | 2 GB | 50 GB | 1000 GB | 60 Mbps | $85.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=107) |
| CSSD3 | 3核 | 4 GB | 100 GB | 1500 GB | 80 Mbps | $240.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=108) |
| CSSD4 | 4核 | 8 GB | 200 GB | 2500 GB | 100 Mbps | $660.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=109) |
| CSSD5 | 5核 | 16 GB | 400 GB | 3500 GB | 100 Mbps | $1140.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=110) |
| CSSD6 | 6核 | 32 GB | 800 GB | 5500 GB | 100 Mbps | $2160.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=111) |

> 优惠码 **`VU6E1H58UY`** 年付方案可享受 **20% 循环折扣** + 免费升级 100 Mbps 端口

---

### **CN2 GIA 优化线路 — CAMD 系列（AMD EPYC NVMe）**

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 年付价格 | 购买 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| CAMD0 | 1核 | 768 MB | 10 GB | 250 GB | 30 Mbps | $37.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=176) |
| CAMD1 | 1核 | 1 GB | 25 GB | 600 GB | 50 Mbps | $58.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=177) |
| CAMD2 | 2核 | 2 GB | 50 GB | 1000 GB | 60 Mbps | $90.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=178) |
| CAMD3 | 3核 | 4 GB | 100 GB | 1500 GB | 80 Mbps | $253.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=179) |
| CAMD4 | 4核 | 8 GB | 200 GB | 2500 GB | 100 Mbps | $694.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=180) |
| CAMD5 | 5核 | 16 GB | 400 GB | 3500 GB | 100 Mbps | $1197.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=181) |
| CAMD6 | 6核 | 32 GB | 800 GB | 5500 GB | 100 Mbps | $2269.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=182) |

> CAMD 系列同样可用优惠码 **`VU6E1H58UY`** 享 20% 循环折扣，AMD EPYC CPU 单核浮点性能通常优于同代 Intel Xeon

---

### **CN2 GIA 优化线路 — CKVM 系列（HDD RAID10，大硬盘方案）**

| 套餐 | CPU | 内存 | HDD | 月流量 | 带宽 | 年付价格 | 购买 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| CKVM1 | 1核 | 756 MB | 35 GB | 500 GB | 50 Mbps | $55.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=16) |
| CKVM2 | 2核 | 1.5 GB | 75 GB | 1000 GB | 60 Mbps | $110.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=16) |
| CKVM3 | 3核 | 4 GB | 150 GB | 1500 GB | 80 Mbps | $80.99/季 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=16) |
| CKVM4 | 4核 | 8 GB | 300 GB | 2500 GB | 100 Mbps | $65.99/月 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=16) |
| CKVM5 | 5核 | 16 GB | 600 GB | 3500 GB | 100 Mbps | $95.99/月 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=16) |
| CKVM6 | 1核 | 756 MB | 150 GB | 500 GB | 50 Mbps | $65.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=16) |
| CKVM7 | 2核 | 1.5 GB | 300 GB | 1000 GB | 60 Mbps | $120.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=16) |
| CKVM8 | 3核 | 4 GB | 450 GB | 1500 GB | 80 Mbps | $40.99/月 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=16) |

> CKVM 使用 HDD RAID10，磁盘速度不如 NVMe，优势是储存空间大、价格低，适合做备份节点或对 I/O 要求不高的应用

---

### **日本大阪 软银线路 — JSSD 系列（Premium NVMe，亚太优选）**

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 年付价格 | 购买 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| JSSD0 | 1核 | 768 MB | 10 GB | 250 GB | 30 Mbps | $39.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=129) |
| JSSD1 | 1核 | 1 GB | 20 GB | 600 GB | 50 Mbps | $70.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=130) |
| JSSD2 | 2核 | 2 GB | 40 GB | 1000 GB | 60 Mbps | $100.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=131) |
| JSSD3 | 3核 | 4 GB | 80 GB | 1500 GB | 80 Mbps | $28.99/月 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=132) |
| JSSD4 | 4核 | 8 GB | 160 GB | 2500 GB | 100 Mbps | $65.99/月 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=133) |
| JSSD5 | 5核 | 16 GB | 320 GB | 3500 GB | 100 Mbps | $109.99/月 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=134) |
| JSSD6 | 6核 | 32 GB | 600 GB | 5500 GB | 100 Mbps | $190.99/月 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=135) |

> 优惠码 **`WWP2OEG8IM`** 可享 **10% 循环折扣**。日本节点走软银 IP 中转，对东亚（香港、台湾、韩国、日本本地）用户延迟更友好

---

### **日本大阪 预算线路 — NKVM 系列（Cheap Japan NVMe）**

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 年付价格 | 购买 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| NKVM0 | 1核 | 768 MB | 10 GB | 500 GB | 200 Mbps | $35.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=162) |
| NKVM1 | 1核 | 1 GB | 25 GB | 1000 GB | 500 Mbps | $55.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=163) |
| NKVM2 | 2核 | 2 GB | 50 GB | 2000 GB | 500 Mbps | $80.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=164) |
| NKVM3 | 3核 | 4 GB | 100 GB | 3000 GB | 500 Mbps | $140.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=165) |
| NKVM4 | 4核 | 8 GB | 200 GB | 5000 GB | 500 Mbps | $50.99/月 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=166) |
| NKVM5 | 5核 | 16 GB | 400 GB | 10000 GB | 500 Mbps | $90.99/月 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=167) |
| NKVM6 | 6核 | 32 GB | 800 GB | 20000 GB | 500 Mbps | $180.99/月 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&pid=168) |

---

### **洛杉矶普通线路 — SSD 系列（预算首选，年付低至 $25.99）**

| 套餐 | CPU | 内存 | NVMe | 月流量 | 带宽 | 年付价格 | 购买 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| SSD0 | 1核 | 512 MB | 10 GB | 500 GB | 200 Mbps | $25.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=18) |
| SSD1 | 1核 | 1 GB | 25 GB | 1000 GB | 500 Mbps | $39.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=18) |
| SSD2 | 2核 | 2 GB | 50 GB | 2000 GB | 500 Mbps | $70.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=18) |
| SSD3 | 3核 | 4 GB | 100 GB | 3000 GB | 500 Mbps | $130.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=18) |
| SSD4 | 4核 | 8 GB | 200 GB | 5000 GB | 500 Mbps | $250.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=18) |
| SSD5 | 5核 | 16 GB | 400 GB | 10000 GB | 500 Mbps | $480.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=18) |
| SSD6 | 6核 | 32 GB | 800 GB | 20000 GB | 500 Mbps | $940.99 | [立即购买](https://bill.hostdare.com/aff.php?aff=4104&gid=18) |

> 优惠码 **`DEAL50`** 可享年付 **5 折循环折扣**（普通 NVMe/AMD/HDD 系列适用），SSD0 折后低至约 **$13/年**

---

## **最新优惠码汇总（循环有效）**

| 优惠码 | 折扣 | 适用套餐 | 备注 |
| --- | --- | --- | --- |
| `VU6E1H58UY` | **20% 循环折扣** | CSSD / CAMD / CKVM | + 免费升级 100 Mbps 端口 |
| `XY604XMHXK` | **25% 循环折扣** | ASSD / SSD / HDD | + 双倍内存和月流量 |
| `DEAL50` | **50% 循环折扣** | 普通 NVMe / AMD / HDD 系列 | 年付及以上有效 |
| `WWP2OEG8IM` | **10% 循环折扣** | 日本 JSSD / NKVM | 年付有效 |
| `QQKF3H319D` | **10% 循环折扣** | 保加利亚 NVMe | 年付有效 |

> 所有优惠码均为年付及以上周期有效，月付通常不适用。优惠码为循环有效但建议下单前在购物车验证，部分码有使用期限。

👉 [点击查看全部套餐并使用优惠码](https://bit.ly/HostdaRe)

---

## **谁适合买 HostDare，谁不适合**

跑分看完了，价格也摆出来了，接下来说最关键的一个问题：这钱值不值得花在你身上。

**适合买的人：**

- 建站或跑代理，主要访客在中国大陆——CN2 GIA 三网优化这块的效果是实打实的，花 $35 买个 CSSD0 年付，访问体验秒杀绝大多数普通美国 VPS
- 预算紧张但又不想用烂货——普通 SSD 系列年付 $25 出头，配上 DEAL50 折扣近乎半价，磁盘是真 NVMe，不是那种廉价共享存储池
- 熟悉 Linux 基本操作的用户——HostDare 是纯非托管 VPS，你需要自己装系统、配服务，不会用 SSH 的话不建议碰
- 需要日本节点的亚太业务——JSSD 软银线路对香港、台湾、韩国方向延迟更低，NKVM 系列性价比也不错

**不适合买的人：**

- 需要托管服务、希望客服帮你装 WordPress 的——HostDare 不提供这类服务
- 退款纠纷心理预期——HostDare 只有 3 天退款窗口，且使用流量超过月配额 20% 可能拒绝退款，下单前建议用官方测速 IP 自己 ping 一下
- 对高计算性能有需求的——CPU 跑分属于中等水平，不适合机器学习、大规模编译、视频转码等重计算场景

---

## **买哪个套餐比较划算：三个场景推荐**

**场景一：轻量建站 / 个人项目 / 最省钱**

推荐 **CSSD0**，年付 $35.99，用优惠码 `VU6E1H58UY` 打 8 折约 $28.79，CN2 GIA 三网优化，10GB NVMe，够跑一个 WordPress 或者 Typecho 没问题。

👉 [直接下单 CSSD0](https://bill.hostdare.com/aff.php?aff=4104&pid=112)

**场景二：稳定性要求高、流量稍大的业务**

推荐 **CSSD1**，1GB 内存 + 25GB NVMe + 600GB 月流量，50 Mbps 带宽，折后 $44.79/年，跑个中等流量的站点或者多个小项目都够用。

👉 [直接下单 CSSD1](https://bill.hostdare.com/aff.php?aff=4104&pid=106)

**场景三：亚太方向业务 / 日本节点**

推荐 **NKVM1**，日本大阪，1GB 内存 + 500 Mbps 大带宽，这也是本文跑分测试用的那款机型，实测 NVMe 磁盘和网络表现都不错。用码 `WWP2OEG8IM` 折后约 $50/年。

👉 [直接下单 NKVM1](https://bill.hostdare.com/aff.php?aff=4104&pid=163)

---

## **几个买之前要知道的细节**

1. **支付方式**：支持支付宝、PayPal、信用卡，国内用户用支付宝付款没有汇率烦恼

2. **退款政策**：3 天内有效，有条件——使用流量超过月配额 20% 可能被拒绝退款。这意味着下单前要先用官方测速 IP 验证延迟，别把退款当试用

3. **Windows 支持**：CSSD3 及以上、CAMD 2GB 以上方案支持 Windows Server，但你需要自备正版许可证，HostDare 不卖操作系统

4. **超售情况**：用户社区普遍反映较少出现严重超售，Unixbench 跑分结果也印证了这点——如果节点资源被严重超卖，UnixBench 得分会明显偏低

5. **DDoS 防护**：提供最高 3 Gbps 的 DDoS 缓解，应付一般攻击没问题，对抗 100G 以上的大攻击则不能依赖这个

---

## **总结：HostDare 跑分值不值得信任**

跑分这件事，真正的用途是排除买到烂货的风险，而不是挑一台最强的机器。

HostDare 的跑分结果告诉你的信息是：NVMe 磁盘是真的、CPU 资源是正常分配的、网络不是摆设——在这个价位，这三件事都做到，已经不多见了。

如果你的需求刚好是"便宜 + 国内访问稳定 + 自己会管 Linux"，HostDare 的 CN2 GIA 系列目前确实是这个细分市场里性价比最高的选项之一。买入门款 CSSD0，年付不到 $30，先用着。

👉 [查看 HostDare 全部套餐，使用优惠码下单](https://bit.ly/HostdaRe)
