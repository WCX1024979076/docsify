# 安排列表

## 待完成

1、PLCT实习生

~~BL808 SPI+LCD的调试？~~

~~BL808 HW_TIMER的实现？~~

~~BL808 FLASH的实现？~~

~~完成了GNU项目的minizip的迁移~~

**ESP32-C3的WIFI/GPIO/UART/SPI/WDG实现？**

~~提交个PR，userapps/minizip包~~

BL808 WIFI的实现？

RT Smart GNU项目的迁移，例如Sqllite、micropython、**python** 和mcurses？

补充`FreeRTOS-Wrapper`的`README_zh.md`

开源之夏的申请，ESP32 申请成功，三个任务：

- ~~使用`scons`编译而非`cmake`，进行重构；找到链接脚本错误并尝试解决中。。。 重构成功，成功解决了bin生成问题和链接脚本问题~~

- 实现WIFI驱动 太他妈难了，`ESP-IDF`这边提供了`LWIP`库，`RtThread`这边提供了`LWIP`库，尝试对`ESP-IDF`中`esp-wifi`库进行重构；~~这个进行增量增加依赖，缺少依赖增加相关函数来实现；有点难，建议抓编译日志，抓取参与的C文件或者通过cmake.txt来抓取分析。目前编译成功，但是离可以使用还有一段距离，报错指令缺失。~~目前可以链接，但是rt_mutex_take报错。

踩坑记录：

1、nvs_flash_init函数rtthread代码放到iram上，已解决

2、Og和Os编译参数，可能

3、tskTCB的问题，暂时绕过

4、xQueueSemaphoreTake中rt_mutex_take的问题？？？

5、xQueueReceiveFromISR和xQueueReceiveFromISR的锅？？？

- 实现BLE驱动

NimBle模块，RT-Thread已迁移

~~完成阶段1任务提交PR，计划要完成的事情：~~

- ~~`esptools.py`逻辑分离~~

- ~~上传`bootloader.bin`和`partition-table.bin`，用户可不安装`esp_tools`~~

- ~~更改`README.md`，完善描述，是否要废弃`idf.py`编译？已废弃~~

- ~~更改`action.yml`加入`scons`自动化测试~~

提交PR：

https://github.com/RT-Thread/rt-thread/pull/7821

https://github.com/RT-Thread-packages/esp-idf/pull/10

https://github.com/RT-Thread-packages/FreeRTOS-Wrapper/pull/37

文章：

https://club.rt-thread.org/ask/article/d0cfd78e7cdec07b.html

2、图优部分

读中文论文，学习思路？

跑bfs和sssp算法，pull模式和push模式相比是不是会有切换的点；而对非单调类算法，绝大多数情况是pull模式更有优势

看一下相关论文，在动态图处理领域你想做什么方向，作为你硕士阶段的研究点，可以找一篇经典论文作为基础（比如流图处理领域就是graphbolt和kickstarter），看论文后可以做一个总结，看看前沿的工作围绕哪些方向做的，做一个ppt之类的

阅读`To Push or To Pull.pdf`论文，大体读了一遍，没读懂

阅读新发的论文 * 2，学姐一篇 + 张老师一篇。

目前Tegra性能存在一些问题，猜测是`source_change_in_contrbution`导致的性能下降；不是由于活跃点集挂钩，必须激活一部分顶点来进行`Pull`，感觉可以切换优化？Tegra收敛判断还是有点问题，再想办法改改。

<details> 
<summary>实现`SSSP`算法和`BFS`算法</summary>

~~实现`SSSP`算法和`BFS`算法，`GraphBolt`能计算`SSSP`吗？我觉得不太可能吧，`SSSP`是取最小值`min`？`BFS`是取最小值层数`depth`；`SSSP`和`BFS`的连通性该如何解决？两种单调类算法`push`用小顶堆实现也不是不可以，每个顶点都开一个小顶堆，小顶堆的开销大概是`O(|E|)`；对于`pull`而言无需这么做，只需识别出受影响的顶点并且进行重计算即可。理论上可行，再想想。刚刚想了一下，需要每次迭代每个顶点都需要维护一个小顶堆，开销大概为O(T*|E|)，感觉差不多可行，明天可以试一下。实现遇到了一些问题，原子操作无法实现，需要借助锁来实现；另外`sourceChangeInContribution`没啥好实现的思路，再想想想想，感觉还是不行，再想想想想。难写难写，这`sourceChangeInContribution`是真不好实现，差不多了，初稿实现了，有一些小小的误差，但是问题不大。。。。实现了，下一步就是优化 & 读论文优化 & 实现`Pull`和传统（还是要实现`sourechange`。实现了SSSP和BFS的Tegra版本。~~

</details>


~~优化`Tegra`算法，添加`has_source_chang`和`force_active`~~

~~GraphBolt和Tegra的进一步结合优化？~~

~~原版GraphBolt和现版GraphBolt区别？~~

~~HPC论文提上日程？DDL 7月7日？废了~~

~~改进Lp算法的Tegra准确度问题？已改进~~

~~用机器学习算法预测`t1`和`t2`~~

3、未来图书馆空间预约系统

写前端，已新建项目并开始写

4、8月19日去微山湖游玩

具体后面做计划

5、外出旅游计划安排

下半年去四川游玩

六月份去南京游玩 / 研究生入学

## 暂缓

1、一生一芯的进一步优化 

减少寄存器数目

## 已完成

~~1、系统结构实验 1月6日之前 提前 采用超标量处理器设计~~

~~http://xzc.cn/wMYSERYGAE~~

~~2、系统结构考试~~

~~3、论文阅读~~

~~第一篇论文已基本读完~~

~~第二篇论文抓紧读~~

~~4、写一个简单的PDF翻译软件 利用Electron开发 暂缓~~

~~分为两个部分，基于开源Pdf.js以及留有插件空间，POST和GET，增加腾讯翻译和bing翻译，对代码进行混淆~~

~~增加数据存储功能，设置相应API ID和密钥保存。~~

~~两边对齐~~

~~模块式开发，支持模组！！！~~

~~数据清洗功能加强~~

~~5、迁移服务器~~

~~内容迁移~~

~~域名迁移~~

~~6、提醒省网档案补全~~

~~7、毕设开题报告书写~~

~~修改毕设开题报告~~

~~完成两篇外文文献的翻译~~

~~8、提醒班级省网优秀毕业生报备~~

~~9、本周完成成人教育论文的评价~~

~~10、本科毕业设计~~

~~准备毕业设计答辩PPT？~~

~~准备优秀毕业设计申请书？~~

~~继续优化GraphBolt & Tegra 提升性能？~~

~~毕业设计论文的撰写？~~

~~参考文献的整理？~~

~~论文的总排版？重点是图片和公式的排版？~~

~~灵敏度分析、切换性能开销、每轮迭代的执行模式和执行时间分析部分撰写？~~

~~摘要要重新书写？~~

~~终稿的提交？~~

~~初稿的更改？~~

~~初稿的查重？~~

~~front_curr_tegra的修正？有一点小问题，尝试解决中。。。。。。阿巴阿巴 阿巴阿巴 阿巴阿巴~~

~~感觉传统计算、GraphBolt和Tegra讲的不够清楚，再改改？~~

~~初稿计划于27日完成，拖延2天？~~

~~第一章 绪论~~

~~第二章 图计算~~

~~第三章 xxxx~~

~~第四章 xxxx~~

~~第五章 xxxx~~

~~本周要完成：~~

- ~~三种模式伪代码、公式的撰写~~

- ~~性能比较~~

- ~~模式介绍、切换的伪代码~~

- ~~切换框架~~

- ~~实验条件设置（算法、数据集）~~

~~机器学习建模？~~

~~优化算法的提出、实现与验证？~~

~~写论文，用公式等描述GraphBolt增量计算、传统计算、Tegra增量计算计算过程、优缺点~~

~~用公式整理GraphBolt切换的数学模型？~~

~~切换的伪代码撰写，以及活跃点集、change数组、聚合值数组和顶点数组是如何做到切换的？~~

~~补充三种计算模式的伪代码~~

~~三种计算模型的时间模型，切换方式的提出~~

~~改写Makefile，一键式测试~~

~~收集实验数据，测试理想状态下最优迭代时间和切换迭代时间对比，查看改进空间大小~~

~~尝试书写实现Tegra代码~~

~~中期检查？~~

~~校级中期检查？~~

~~继续阅读GraphBolt源代码~~

~~11、本科毕业~~

~~本科毕业总感觉该写点什么，石油大学真的让人又爱又恨，唉，总之祝母校人才济济，桃李满天下吧。我也该开启研究生新征程了，多少有点迷茫，唉。另外济青六日游个人感觉良好，感谢三位一路上的陪伴，祝前程似锦，一切顺利！！！~~

~~12、活塞气举系统~~

~~整理一份Word文档，关于梯度下降法~~

~~https://blog.csdn.net/qq_35240204/article/details/106864745~~

~~https://blog.csdn.net/weixin_42018112/article/details/88096070~~

~~登陆界面~~

~~https://ubuntu.tim-wcx.ltd/oil/~~

~~https://ubuntu.tim-wcx.ltd/oil_new~~

~~13、C#图书管理系统~~

~~要修复的问题~~

- ~~汇文数据库的Ping连接测试~~

- ~~IO控制和休眠实现~~

~~软著材料编写~~