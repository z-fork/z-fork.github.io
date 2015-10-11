---
title: snowflake
layout: post
tags:
- python
- 分布式
---

### 分布式自增 id 算法

有关 snowflake 的由来网上已经有相当多的叙述, 若未通说过自行 google. 

#### snowflake

|| *timestamp* || *configured machine id* || *sequence number* ||
|| -  41bits - || -------  10bits ------- || ----  12bits ---- ||
