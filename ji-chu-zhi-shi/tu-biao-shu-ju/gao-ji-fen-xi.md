# 高级分析

我们可以看到在“高级分析”这一栏中，有滚动窗口、时间比较、重新采样这三个可选选项。

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

在这一部分将详细讲解这三个选项有什么作用，以及应用这三个选项之后的变化是怎样的。所有的讲解都是基于下面的指标来讲解。

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

上面这个指标筛选了2023-01-01\~2023-12-31之间的数据，并以月为时间粒度来计算“每月产生单据的数量”这个指标。



**时间比较**

滚动窗口

滚动窗口中，有一个滚动函数下拉框，在下拉框中有mean（平均值）、sum（总和）、std（标准）、cumsum（累加）这几个选项，默认为空，即啥也不选。



时间比较

时间偏移表示相对当前指标中时间的偏移值，而计算类型是指当前指标时间与偏移指定时间后的指标的计算方式。

在下图中时间偏移为 1 year ago（一年之前）即为2022年，计算类型为Actual value(实际值)，生成了一条虚线折线图来表示。

<figure><img src="../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

在下面这个图中计算类型为“差异”，即2023年与2022年指标的差值。

<figure><img src="../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>



下图中计算类型为比率，即2023年与2022年指标的比值。

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

下图中计算类型为Percentage change(百分比变化)，即（2023年指标-2022年指标的比值）/2022年指标，可以通过这个计算类型来计算这年相对上一年的增长比例。

<figure><img src="../../.gitbook/assets/image (5) (1) (1).png" alt=""><figcaption></figcaption></figure>





**重新采样**

