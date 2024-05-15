# 使用 Jinja 检索过滤器值

### 概述 <a href="#overview" id="overview"></a>

在本文中，我们将学习如何使用 Jinja 模板来检索可直接在虚拟数据集的 SQL 查询中使用的筛选器选择：

![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image-1701462888539.png)

您可以下载 ZIP 文件以将此示例仪表板导入您的工作区：

[Jinja 示例 - 过滤值 Macro.zip](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Jinja%20Example%20-%20Filter%20Values%20Macro.zip)

### 过程 <a href="#the-process" id="the-process"></a>

这个过程包括 3 个步骤：

1. 创建虚拟数据集。
2. 从虚拟数据集创建图表并将其添加到仪表板。
3. 配置仪表板过滤器。

让我们仔细看看每一步。

***

### 第 1 步：创建虚拟数据集 <a href="#step-1-create-a-virtual-dataset" id="step-1-create-a-virtual-dataset"></a>

对于本示例，我们将使用示例数据库中的“车辆销售”表。

考虑下面的查询：

```plsql
WITH calculation as (
	SELECT count(*) as count, country
	FROM "Vehicle Sales"
	WHERE product_line in ('Classic Cars', 'Motorcycles')
	GROUP BY country
)
SELECT * FROM calculation
```

如果您使用此查询创建虚拟数据集，它将只有两列：计数和国家/地区。因此，您将无法创建本机仪表板过滤器来控制product\_line选项。这只是一个简单的示例用例，其中仪表板过滤器应应用于内部查询（而不是外部数据集查询）。

为了实现此实现，我们将使用逻辑语句和 `filter_values()` Jinja 宏。这是更新后的 SQL 查询：

```plsql
WITH calculation as (
	SELECT count(*) as count, country
	FROM "Vehicle Sales"
	WHERE 1=1
	{% raw %}
{% if filter_values('product_line')|length %}
		and product_line in {{filter_values('product_line')|where_in}}
  	{% endif %}
{% endraw %}
	GROUP BY country
)
SELECT * FROM calculation
```

* `filter_values()` 宏返回在仪表板过滤器中选择的所有选项的数组。 Product\_line 参数用于指定为我们将使用的过滤器提供动力的列。
* if 语句检查返回的数组是否已设置长度（如果返回 0，则不满足语句条件）。
* |where\_in 运算符自动将此数组格式化为 SQL 兼容格式（`["Classic Cars", "Vintage Cars"]` 变为 ('Classic Cars', 'Vintage Cars')）。

要在 SQL Lab 中执行此查询：

1. 访问您的工作区。
2. 导航到 SQL > SQL Lab。
3. 确保选择了示例连接。
4. 粘贴查询并单击“运行”。

![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image-1701460162276.png)

这是生成的 SQL 查询：

```plsql
-- 6dcd92a04feb50f14bbcf07c661680ba
WITH calculation as (
	SELECT count(*) as count, country
	FROM "Vehicle Sales"
	WHERE 1=1
	GROUP BY country
)
SELECT * FROM calculation
LIMIT 100001
-- 6dcd92a04feb50f14bbcf07c661680ba
```

单击“创建图表”以该查询构建图表。

***

### 步骤 2：从虚拟数据集创建图表并将其添加到仪表板 <a href="#step-2-create-a-chart-from-the-virtual-dataset-and-add-it-to-a-dashboard" id="step-2-create-a-chart-from-the-virtual-dataset-and-add-it-to-a-dashboard"></a>

您应该已重定向到图表生成器视图。对于此示例，我们可以使用具有默认配置的表格：

1. 单击右上角的“保存”。

![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image-1701460222605.png)

2. 命名您的数据集、图表并为其创建仪表板：\
   ![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image-1701460270906.png)
3. 单击“保存并转到仪表板”按钮。

***

### 步骤 3：配置仪表板过滤器 <a href="#step-3-configure-the-dashboard-filters" id="step-3-configure-the-dashboard-filters"></a>

仪表板将在您的浏览器中启动。配置过滤器：

1. 单击右箭头展开“过滤器”菜单。

![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image-1701460437304.png)

2. 单击“+ 添加/编辑过滤器”。

![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image-1701460470654.png)

3. 过滤器模式已启动。在“过滤器名称”字段中，输入过滤器的名称。
4. 在“数据集”字段中，选择实际包含具有所需选项的列的数据集（在本例中为“车辆销售”数据集）。
5. 在“列”字段中，选择包含所需选项的列（在本例中为“product\_line”）。![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image-1701462621962.png)
6. 接下来，导航到“范围界定”选项卡，并将过滤器映射到您的图表。此过程不会自动发生，因为为过滤器提供支持的数据集与为图表提供支持的数据集不同。![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image-1701462649585.png)
7. 单击“保存”。现在让我们看看过滤器的作用：

![](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/image-1701462888539.png)

\
