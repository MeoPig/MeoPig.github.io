---
title:  【数据可视化】学校生源分布可视化
date: 2021-05-15 22:47:29
tags:  数据可视化
---
# 本项目采用Flask框架基于echarts实现省市县地图三级下钻以及图表联动操作
运行软件：Pycharm
运行环境：python3.8
第三方包:   flask,pymysql,utils

## 实现效果:
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705202628543.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjA2OTM0,size_16,color_FFFFFF,t_70#pic_center)

**省市县三级下钻展示：**
市级：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705202708111.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjA2OTM0,size_16,color_FFFFFF,t_70#pic_center)
县级：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705202718979.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjA2OTM0,size_16,color_FFFFFF,t_70#pic_center)
**扇形图做总控制台，柱状图以及表格跟随联动：**
如点击扇形图里面的陕西，柱状图跟表格则会切换为陕西的数据
![在这里插入图片描述](https://img-blog.csdnimg.cn/20210705202902996.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQ1MjA2OTM0,size_16,color_FFFFFF,t_70#pic_center)

## 本项目已开源到Github
链接：[MapdrildownAndLinkage](https://github.com/MeoPig/MapdrilldownAndLinkage)
