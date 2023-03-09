# 本地金融数据库构建

## 一、标的和时间

- 索引包含标的、时间

- 使用为时间序列优化的数据库（TSDB）

- 数据包含“现在”的状态

## 二、设置等级

- 封装度逐级提高，每一级的正确依赖于前一级的正确

- 基础

    源数据：订阅数据库、自建爬虫等

    预处理后数据：列名统一、数据类型统一、去重、空值处理、去除非交易日或非交易日填充等

- 衍生

    常用数据：计算不同应用场景收益率、提供同一数据品种的不同时间频率等

    因子：计算该部分数据有开箱即用的体验，可专注于因子逻辑而不需前述数据处理

    统计量：回测的结果存储，有不同数据的多维评价指标；作汇报、分析与选择、报告撰写数据源

 ```mermaid
flowchart LR
subgraph 基础
direction LR
C0[(源数据)]-->C1[(预处理后数据)]
end
subgraph 衍生
direction LR
C2[(常用数据)]-->C3[(因子)]
C3[(因子)]-->C4[(统计量)]
end
A[(宿主机)]-->基础-->衍生-->C5[(产品)]
 ```

## 三、工程

- 新数据、因子入库，同时具有初始化该表的全量数据脚本与按最频繁时间频率更新的增量数据脚本

- 宿主机硬盘读写速度、局域网带宽、计算性能有较高优先级；金融数据库相比其他工业数据库并不大，存储空间排在优先级靠后的位置

- 同一等级数据仅依赖于前一级数据，同级数据互不影响，计算时进行并发处理

- 脚本有时间统计、日志存储功能，为后续优化迭代准备

- 集群部署、异地部署时有心跳检测

## 四、工具

- 计算器 [python](https://python.org)

- 数据库 [influxdb](https://github.com/influxdata/influxdb)

- 部署 [wireguard](wireguard.com)
