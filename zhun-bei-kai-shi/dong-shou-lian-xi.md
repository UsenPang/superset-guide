---
description: >-
  本教程针对想要在 Superset 中创建图表和仪表板的人员。我们将向您展示如何将 Superset
  连接到新数据库并在该数据库中配置表以进行分析。您还将探索已公开的数据并向仪表板添加可视化效果，以便您了解端到端的用户体验
---

# 动手练习

### 连接到新数据库

Superset 本身不用来存储数据，而是通过连接您现有的 SQL 数据库或数据存储来获取数据。

首先，我们需要建立数据库的连接，以便能够查询和可视化其中的数据。在右上角的 设置菜单下，选择测试连接，然后选择“+数据库”选项：

<figure><img src="../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

然后在数据库选项选择clickhouse connect :&#x20;

<figure><img src="../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

填写对应的连接信息

<figure><img src="../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

点击右下角的“连接”按钮保存配置。



### 注册数据集

数据集可以理解为一张表，数据集的创建方式有两种，一种是在数据集界面下创建，另一种是通过SQL创建；创建方式的不同数据集的类型也不同，通过第一种方式创建的数据集叫做物理数据，第二种方式创建的数据集叫做虚拟数据集。

#### 第一种方式

在数据集菜单下点击 “+数据集” ,会看到如下的界面，然后选择数据库和表

<figure><img src="../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>



#### 第二种方式

在SQL菜单下点击SQL工具箱，创建一个查询

<figure><img src="../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

然后点击保存按钮右侧的折叠图案，另存为数据集，为你的数据集起个名字

<figure><img src="../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

至此两种方式已经介绍完成，通过这两种方式你应该理解了物理数据集与虚拟数据集的区别。





### 创建图表

在图表界面点击创建图表，然后在弹出的页面中选择数据集以及图表模板，创建图表

<figure><img src="../.gitbook/assets/image (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

现在，您将看到一个强大的工作流程，用于探索数据和迭代图表。

<figure><img src="../.gitbook/assets/image (2) (1) (1).png" alt=""><figcaption></figcaption></figure>

* 左侧的数据集视图有一个列和指标列表，范围仅限于您选择的当前数据集。
* 图表区域下方的数据预览还为您提供有用的数据上下文。
* 使用“数据”选项卡和“自定义”选项卡，您可以更改可视化类型、选择时间列、选择要分组的指标以及自定义图表的美观程度。



点击右上角的保存按钮，添加到一个新的看板

<figure><img src="../.gitbook/assets/image (3) (1) (1).png" alt=""><figcaption></figcaption></figure>

可以看到将图表添加到了看板中

<figure><img src="../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

然后点击右上角的编辑仪表盘，然后，单击并拖动图表的右下角，直到图表布局到你喜欢的大小。

单击“保存”以保留更改。至此你已经学会了如何创建一个图表，并将它保存到仪表板。























