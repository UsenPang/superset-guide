# 使用 Jinja 操作图表的自定义 SQL 筛选器

### 概述 <a href="#overview" id="overview"></a>

在本文中，您将学习如何使用 Jinja 模板来动态更改应用于图表的自定义 SQL 过滤器：

<figure><img src="https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Jinja%20example.gif" alt=""><figcaption></figcaption></figure>

您可以下载 ZIP 文件以将此示例仪表板导入您的工作区：

[Jinja 示例 - 使用 Jinja 的阈值过滤器](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Jinja%20example%20-%20threshold%20filter%20using%20Jinja.zip)

***

### 过程 <a href="#the-process" id="the-process"></a>

这个过程包括 3 个步骤：

1. 将 Jinja 语法添加到图表的自定义 SQL 过滤器，以使用仪表板过滤器对其进行修改。
2. 创建一个包含选项的数据集来填充我们的仪表板过滤器。
3. 配置仪表板。

让我们仔细看看每一步。

***

### 第 1 步：将 Jinja 语法添加到图表的自定义 SQL 过滤器中 <a href="#step-1-add-jinja-syntax-to-the-charts-custom-sql-filter" id="step-1-add-jinja-syntax-to-the-charts-custom-sql-filter"></a>

在下面的图表中，我们使用折线图（连接到示例数据库中的车辆销售数据集）来计算 2003 年每个产品系列的价格总和：

![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image\(245\).png)

让我们使用 Jinja 来控制聚合结果的阈值。使用以下语法添加自定义 SQL 过滤器 。

请注意，由于我们要过滤指标，因此应该使用 HAVING 选项：

```sql
sum(price_each) >
{% raw %}
{% if filter_values('threshold')|length %}
	{{ filter_values('threshold')[0] }}
{% else %}
	0
{% endif %}
{% endraw %}
```

该语法有一个 if 语句，检查是否有任何值应用于由阈值列支持的仪表板过滤器：

* 如果是这样，过滤器值将添加到 SQL 语法中。
* 如果不是，则将使用 0 作为默认值。

这是最终结果：

![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image\(246\).png)

保存图表并将其添加到仪表板。

***

### 步骤 2：创建数据集以填充仪表板过滤器 <a href="#step-2-create-a-dataset-to-populate-the-dashboard-filter" id="step-2-create-a-dataset-to-populate-the-dashboard-filter"></a>

有一些不同的方法可以实现这一目标：

* 在数据库级别创建表/视图：可能很有用，以便您可以直接在数据库上操作表值；
* 使用 Google Sheets 文件：当您不想在数据库级别处理此问题时很有用。有关将 Google Sheets 作为数据库连接到 Preset 的更多详细信息，请参阅本文。
* 使用临时 SQL 查询：快速方法在仅创建几个选项时非常有用。

我们将在本示例中使用最后一种方法，使用以下查询创建虚拟数据集：

```sql
select 500 as threshold
union all
select 750 as threshold
union all
select 1000 as threshold
union all
select 1500 as threshold
union all
select 2000 as threshold
```

要创建由此 SQL 查询支持的虚拟数据集：

1. 访问您的工作区。
2. 导航到 SQL > SQL Lab。
3. 确保选择了示例连接。
4. 粘贴查询并单击“运行”。
5. 要将其另存为数据集，请单击“保存”下拉列表并选择“保存数据集”。
6. 为您的数据集提供名称，然后单击“保存并浏览”。

请注意，列名称（在本例中定义为阈值）由 `filter_values()` 宏用于 Jinja 语法。如果使用不同的列名称，请确保更新您的 Jinja 语法。

***

### 步骤 3：配置仪表板 <a href="#step-3-configure-the-dashboard" id="step-3-configure-the-dashboard"></a>

访问使用图表创建的仪表板并创建新的过滤器 - 使用在上一步中创建的数据集/列：

<div data-full-width="false">

<img src="https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image(251).png" alt="" height="372" width="960">

</div>

重要细节：

* 由于图表使用另一个数据集，因此该过滤器不会自动映射到它。访问 SCOPING 选项卡，并将过滤器映射到图表。
* 禁用 可以选择多个值，因为我们不会在本示例中使用多个值。

您现在应该能够使用仪表板过滤器与图表的自定义 SQL 过滤器进行交互。
