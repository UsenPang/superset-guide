# 使用 Jinja 操作图表的聚合列

### 概述 <a href="#overview" id="overview"></a>

本文介绍如何使用 Jinja 模板动态更改图表的聚合列：

<figure><img src="https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Jinja%20aggregation%20column%20gif.gif" alt=""><figcaption></figcaption></figure>

您可以下载 ZIP 文件以将此示例仪表板导入您的工作区 - 根据您的工作区位置选择选项：

[Jinja 示例 - 动态维度](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Jinja%20example%20-%20dynamic%20dimension.zip)

***

### 过程 <a href="#the-process" id="the-process"></a>

这个过程包括 3 个步骤：

1. 在数据集上创建计算列，该列将用于获取仪表板过滤器值
2. 创建带有选项的数据集来填充我们的仪表板过滤器
3. 配置仪表板

让我们仔细看看每一步。

***

### 步骤 1：在数据集上创建计算列 <a href="#step-1-create-a-calculated-column-on-the-dataset" id="step-1-create-a-calculated-column-on-the-dataset"></a>

在此示例中，我们将使用聚合表格图表（连接到示例数据库中的车辆销售数据集）来计算价格值的总和，并根据通过仪表板过滤器控制的动态维度进行聚合。

在图表生成器视图中，使用 `SUM(price_each)` 作为指标创建聚合表格图表。让我们使用 Jinja 创建一个计算列用作动态维度。为此：

1. 单击数据集名称旁边的三个省略号 (...) > 编辑数据集。
2. 导航到“计算列”选项卡。
3. 使用以下语法创建计算列：

```sql
{% raw %}
{% if filter_values('aggregation')|length %}
	{{ filter_values('aggregation')[0] }}
{% else %}
	month
{% endif %}
{% endraw %}
```

该语法有一个 if 语句，检查是否有任何值应用于由聚合列支持的仪表板过滤器：

* 如果是这样，过滤器值将被添加到 SQL 语法中，作为要使用的实际列。
* 如果没有，月份将用作默认列。

这是最终结果：

![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image-1670615527569.png)

保存更改，维度列现在应该可以在图表生成器中使用。将其作为维度添加到图表中：

![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image-1702066205157.png)

保存图表并将其添加到仪表板。

***

### 步骤 2：创建数据集以填充仪表板筛选器 <a href="#step-2-create-a-dataset-to-populate-the-dashboard-filter" id="step-2-create-a-dataset-to-populate-the-dashboard-filter"></a>

有不同的方法可以实现这一目标：

* 在数据库级别创建表/视图：可能很有用，以便您可以直接在数据库上操作表值；
* 使用 Google Sheets 文件：当您不想在数据库级别处理此问题时很有用。有关将 Google Sheets 作为数据库连接到 Preset 的更多详细信息，请参阅本文。
* 使用临时 SQL 查询：快速方法在仅创建几个选项时非常有用。

我们将在本示例中使用最后一种方法，使用以下查询创建虚拟数据集：

```sql
select 'product_line' as aggregation
union all
select 'country' as aggregation
```

要创建由此 SQL 查询支持的虚拟数据集：

1. 访问您的工作区。
2. 导航到 SQL > SQL Lab。
3. 确保选择了示例连接。
4. 粘贴查询并单击“运行”。
5. 要将其另存为数据集，请单击“保存”下拉列表并选择“保存数据集”。
6. 为您的数据集提供名称，然后单击“保存并浏览”。

**请注意**: 列名（在本例中定义为聚合）由 `filter_values()` 宏在 Jinja 语法上使用。如果使用不同的列名称，请确保更新您的 Jinja 语法。

根据您的数据库引擎，您还可以查询信息架构以列出所有列值 - 例如，此查询可以在 Postgres 数据库上使用：

```plsql
SELECT column_name
FROM information_schema.columns
WHERE table_schema = '{{SchemaName}}'
AND table_name   = '{{TableName}}';
```

***

### 第 3 步：配置仪表板 <a href="#step-3-configure-the-dashboard" id="step-3-configure-the-dashboard"></a>

访问使用图表创建的仪表板并创建新的过滤器 - 使用在上一步中创建的数据集/列：

![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image-1670615985172.png)

重要细节：

* 由于图表使用另一个数据集，因此此过滤器不会自动映射到它。访问 SCOPING 选项卡，并将过滤器映射到图表。
* 禁用 可以选择多个值，因为我们不会在本示例中使用多个值。

您现在应该能够使用仪表板过滤器与图表的自定义 SQL 过滤器进行交互。
