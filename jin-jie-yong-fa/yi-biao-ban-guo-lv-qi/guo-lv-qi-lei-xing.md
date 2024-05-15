# 过滤器类型

### 概述 <a href="#overview" id="overview"></a>

在本文中，我们将探讨可应用于仪表板内图表的四种不同类型的过滤器。

***

### 过滤器类型 <a href="#filter-types" id="filter-types"></a>

在下面的“添加和编辑过滤器”窗口中，您会注意到两个选项卡：“设置”和“范围”。现在我们将重点关注“设置”选项卡（默认选择）。

<figure><img src="https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/New_Create_Filter.png" alt=""><figcaption></figcaption></figure>

第一部分由四个必填字段组成：

* 过滤器类型：选择过滤器类型；选项包括“值”、“数值范围”、“时间范围”、“时间列”和“时间粒度”（请参阅​​下面的单独说明）。
* 过滤器名称：为新过滤器输入简短的描述性名称。这通常与后续列字段中所做的选择相关。
* 数据集：选择应用于仪表板内图表的数据集。默认情况下，与仪表板图表关联的所有数据集都可供选择。当选择“值”、“数值范围”、“时间列”或“时间粒度”过滤器类型时，会出现此字段。
* 列：选择应应用过滤器的列。当选择“值”或“数字范围”字段类型时，会出现此字段。

让我们重点关注“过滤器类型”字段中的四种可用过滤器类型。

***

### 值过滤器类型 <a href="#value-filter-type" id="value-filter-type"></a>

此过滤器类型创建一个下拉菜单，其中填充了与所选列（列字段）关联的所有可用值。

例如：如果您选择“值”过滤器类型，然后在“列”字段中选择“platform”，则仪表板上会出现一个下拉过滤器菜单，使您能够按platform进行过滤。应用后，仪表板中的所有图表将仅显示与所选“platform”过滤器相关的数据。

配置可能如下所示：

![Settings\_for\_Value](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Settings\_for\_Value.png)

然后..... “过滤器名称”字段中标题为“Select a Plarform”的下拉过滤器如下所示：

![Select\_a\_Platform](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Select\_a\_Platform.png)

在上面的示例中，我们选择按 PS4 平台进行过滤。

要应用过滤器，请选择一个值，然后选择“应用过滤器”。仪表板中的图表将刷新并显示过滤后的数据。

***

### 数值范围过滤器类型 <a href="#numerical-range-filter-type" id="numerical-range-filter-type"></a>

此过滤器类型创建一个范围滑块，使您能够根据所选列中数据的可用数值指定起点和终点。

例如：如果您选择数字范围过滤器类型，然后在“列”字段中选择“eu\_sales”，则会出现一个范围滑块过滤器，使您能够选择开始和结束范围。应用后，仪表板中的所有图表将仅显示与"Select a Range"过滤器相关的数据。

配置可能如下所示：

![Settings\_for\_Numerical\_Range](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Settings\_for\_Numerical\_Range.png)

然后。。。范围滑块过滤器，标题为“过滤器名称”字段中的“Select a Range”，显示如下：

![Select\_a\_Range](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Select\_a\_Range.png)

在上面的示例中，我们选择指定从 16 到 26 的范围。

请注意，您可以通过将光标悬停在最左边的锚点（此处为“0”）上来查看最小滑块设置：

![Select\_a\_Range\_Minimum](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Select\_a\_Range\_Minimum.png)

您也可以通过将光标悬停在最右边的锚点（此处为“29”）上来查看最大滑块设置：

![Select\_a\_Range\_Maximum](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Select\_a\_Range\_Maximum.png)

要应用过滤器，请定义数字范围，然后选择“应用过滤器”。仪表板中的图表将刷新并显示过滤后的数据。

***

### 时间范围过滤器类型 <a href="#time-range-filter-type" id="time-range-filter-type"></a>

过滤器类型选择时间范围过滤器，选择该过滤器后，将启动预设的编辑时间范围功能。这与创建新图表时使用的时间范围字段相同。

有关如何编辑时间范围的更多信息，请参阅[时间配置](https://docs.preset.io/docs/time-configurations).

配置可能如下所示：

![Settings\_for\_Time\_Range](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Settings\_for\_Time\_Range.png)

在此示例中，我们指定了默认值“上个月”，并定义了过滤器名称“Select Time Range”。

此配置将如下所示：

![Select\_Time\_Range2](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Select\_Time\_Range2.png)

要应用过滤器，请选择并配置时间范围，然后选择“应用过滤器”。仪表板中的图表将刷新并显示过滤后的数据。

***

### 时间列过滤器类型 <a href="#time-column-filter-type" id="time-column-filter-type"></a>

此过滤器类型创建一个下拉菜单，使您能够选择新的仪表板级别时间列，<mark style="color:red;">该列将覆盖在图表级别定义的任何现有时间列</mark>。

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

何时使用此过滤器类型？

当所选数据集中存在多个时间列时，最好使用时间列过滤器类型。在许多情况下，数据集中只有一个时间列（默认选择），从而不需要此过滤器类型。

配置可能如下所示：

![Settings\_for\_Time\_Column](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Settings\_for\_Time\_Column.png)

“过滤器名称”字段中标题为“Select Time Column”的下拉过滤器显示如下：

![Select\_Time\_Column1](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Select\_Time\_Column1.png)

要应用仪表板级别时间列，请选择时间列选项，然后选择应用筛选器。仪表板中的图表将刷新并显示过滤后的数据。

***

### 时间颗粒过滤器类型 <a href="#time-grain-filter-type" id="time-grain-filter-type"></a>

此过滤器类型会创建一个下拉列表，使您能够选择时间粒度。应用后，所有时间戳都会根据所选时间粒度进行分组。

配置可能如下所示：

![Settings\_for\_Time\_Grain](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Settings\_for\_Time\_Grain.png)

“过滤器名称”字段中标题为“Select Time Grain”的下拉过滤器显示如下：

![Select\_Time\_Grain](https://cdn.document360.io/4749ddf8-aa05-4f3f-80e1-07a5d2d0f137/Images/Documentation/Select\_Time\_Grain.png)

在上面的示例中，我们选择指定时间粒度“分钟”。

要应用仪表板级别时间粒度分组，请选择一个时间粒度，然后选择“应用筛选器”。仪表板中的图表将刷新并显示分组数据。
