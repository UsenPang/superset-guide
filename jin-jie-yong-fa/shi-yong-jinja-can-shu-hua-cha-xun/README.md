# 使用 Jinja 参数化查询

### 什么是Jinja Templating？ <a href="#what-is-jinja-templating" id="what-is-jinja-templating"></a>

Jinja Templating 是 Python 的 Web 模板引擎。它使用基于文本的模板语言，可用于生成标记和源代码。Jinja 的加入增加了预设功能的灵活性，并具有多种用例，例如：

* 根据当前登录的用户数据（`user_id`和/或`username`）实施访问控制。&#x20;
* 直接在数据集的内部查询上应用仪表板筛选器。
* 当筛选器列的名称与当前查询中的筛选器列的名称不匹配时，使用筛选器组件筛选查询。
* 通过 URL 将筛选器约束应用于仪表板。
* 根据仪表板筛选条件动态修改 SQL 查询的特定元素（例如计算、聚合等）。
* 对操作员的高级控制。
* 个性化仪表板。

在 Superset上， 你可以在 SQL Lab、虚拟数据集和行级安全过滤器中使用 Jinja：

* 将预定义的宏添加到查询以返回动态数据。
* 执行逻辑语句（ if 、 for）。

### 语法 <a href="#syntax" id="syntax"></a>

#### Jinja 宏

Jinja 宏应添加到双大括号`{{ }}` 内。参数也添加到括号内：

```sql
{{ macro_name(parameters) }}
```

以下是可用的宏：

* `current_username()`
* `current_user_id()`
* `current_user_email()`
* `url_param()`
* `filter_values()`
* `metric()`

让我们仔细看看它们：



**当前用户名**

`current_username()`宏返回登录用户的用户名值。此值是全局值（在所有工作区中相同），无法修改。

**例：**

```sql
SELECT * FROM sales WHERE employee = ' {{ current_username() }} '
```



**当前用户 ID**

与 `current_username()` 宏类似，`current_user_id( )` 返回登录用户的 ID 值。请注意，ID 是工作区的本地 ID（一个用户在每个工作区的 ID 都不同），而且不能修改。

**例：**

```sql
SELECT * FROM sales WHERE employee_id = {{ current_user_id() }}
```



**当前用户电子邮件**

`current_user_email()`宏是通过 Jinja 检索用户特定信息的另一种选择。它返回登录用户的电子邮件地址：

```sql
SELECT * FROM sales WHERE employee = '{{ current_user_email() }}'
```



**URL 参数**

`url_param()` 宏可从仪表盘 URL 获取参数值。使用该宏时，需要指定参数名称。可以使用 & 在 URL 中指定多个参数。

**例：**

* 仪表板 URL：`http://192.168.88.129:8088/superset/dashboard/4/?location=US&month=8`

```sql
SELECT * FROM "Vehicle Sales"
WHERE country  = '{{ url_param('location') }}' -- location is the parameter name specified in the URL
AND month = {{ url_param('month') }} -- month is the parameter name specified in the URL
```



**过滤值**

`filter_values()` 宏会从仪表盘过滤器中返回信息。要使用该宏，需要指定驱动过滤器的列名。结果将是一个包含所有应用值的数组（`["first_value_applied", "second_value_applied", ...]`）。可以只返回特定位置的值（例如，使用 `[0]` 返回第一个应用值），或使用 `|where_in` 返回带引号的逗号分隔值。

**例：**

```sql
SELECT * FROM "Vehicle Sales"
WHERE country in ({{ filter_values('country')|where_in }})
-- country is the name of the column used on the dashboard filter.
-- |where_in would return the results in the correct SQL syntax -> 'USA', 'Belgium', 'Canada' instead of ['USA', 'Belgium', 'Canada']
```



**指标**

`metric()`宏可用于从数据集中检索指标 SQL 语法。这可用于不同的目的：

* 覆盖图表级别中的指标标签（不影响数据集）。
* 在计算中合并多个指标。
* 在 SQL 实验室中检索指标语法。
* 跨数据集重用指标。

此宏避免了复制/粘贴，允许用户将指标定义集中在数据集层中。

```sql
SELECT {{ metric('count') }} as count -- count is the key of the metric that is being used
FROM "Vehicle Sales"
```

在图表生成器中使用该宏时，默认情况下将在使用的数据集中搜索度量。不过，也可以指定数据集 ID（在 SQL Lab 中使用该宏时，或想从其他数据集中获取度量语法时）：

```sql
SELECT {{ metric('count', 3) }} as count -- count is the key of the metric that is being used. 3 is the dataset ID
FROM "Vehicle Sales"
```



#### 逻辑语句

逻辑语句可用于根据验证结果执行不同的操作，也可用于执行迭代循环。逻辑语句应加在大括号和百分号（`{% %}`）内。逻辑语句总是需要在代码块的开始和结束部分使用：

```sql
{% raw %}
{% if something %}
     SQL syntax
{% else %}
     SQL syntax
{% endif %}
{% endraw %}
```



**例：**

```sql
SELECT * FROM "Vehicle Sales"
WHERE country
{% raw %}
{% if filter_values('operation')[0] == 'Equals' %} -- execute in case the first value applied to this dashboard filter is "Equals"
     = '{{ filter_values('location')[0] }}'
{% elif filter_values('operation') == 'In' %} -- in case the first condition isn't met, check if the filter value is "In"
     in ({{ filter_values('location')|where_in }})
{% else %} -- execute in case none of the conditions above are met
     not in ({{ filter_values('location')|where_in }})
{% endif %}
{% endraw %} -- end block
```

\
