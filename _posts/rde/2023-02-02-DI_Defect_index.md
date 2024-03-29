---
layout: post
title: "DI统计标准介绍文档"
subtitle:   "什么是DI"
date:       2023-02-02 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-04.jpg"
---

# 什么是DI
Defect Index，DI值是衡量软件质量高低的指标之一，是软件质量状态的客观数据呈现
## DI值组成及统计范围
- 1. 待解决DI值：SUBMITTED、 OPEN、  MORE INFO、IN PROGRESS、经办人确认中、POSTPONE、延期修复确认中、不修复确认中
- 2. 待验证DI值：Resolved
- 3. Total DI值：all DI
## DI计算规则
### 分值权重
- S类jira：每个10分
- A类jira：每个3分
- B类jira：每个1分
- C类jira：每个0.1分

## DI值计算公式
DI=S*10+A*3+B*1+C*0.1


# DI值达成率
DI值达成率 = 已关闭问题DI值 / 预关闭DI值 * 100%

# 效能平台
研发效能平台致力于构建研发数字化平台，通过数据闭环提升研发效能和交付质量，产品功能如下：
- 研发效能度量采集：提供了研发效能数据采集能力，通过完善的数据采集架构、数据采集方案、数据质量方案，获取完整的软件生命周期数据。 
- 研发效能度量分析：效能平台提供了研发效能度量分析能力，涵盖敏捷开发、持续交付等场景下的吞吐、速率、质量等研发效能度量模型的指标定义及规范，支持数据的下钻、关联分析、支持度量问题发现与解决的闭环机制。
- 研发效能度量展现：支持从管理视角洞察需求、编码、测试到发布的研发全流程数据的综合视图，支持按照项目生成质量周报，项目周报，可以面向各级角色给出不同的、有针对性的研发运营指导意见，从而有效支撑研发管理决策。
