---
layout: post
title: "DI统计标准介绍文档"
subtitle:   "什么是DI"
date:       2023-02-02 0:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-04.jpg"
---

## 什么是DI
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
