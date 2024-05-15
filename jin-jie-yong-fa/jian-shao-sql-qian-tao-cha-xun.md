# 使用WITH语句简化查询

使用WITH可以为每次的查询创建虚拟表，减少嵌套查询，使sql看起来更清晰。

并且能够分步骤计算，将复杂的计算过程拆分为多个步骤，并逐步执行这些步骤以达到最终的计算结果。在ClickHouse中，你可以使用多个查询或者子查询来实现分步骤计算。以下是一个示例来说明如何使用分步骤计算：

假设你有两个表：`orders`（订单表）和`order_items`（订单项表），其中`orders`包含订单的基本信息，`order_items`包含订单的具体商品信息。

1.  **第一步：计算每个订单的总价**

    首先，你可以计算每个订单的总价，然后将结果存储到一个临时表或者使用WITH子句（CTE）：

    ```sql
    WITH order_totals AS (
        SELECT order_id, SUM(quantity * price) AS total_price
        FROM order_items
        GROUP BY order_id
    )
    SELECT * FROM order_totals;
    ```

    在这个步骤中，`order_totals`临时表包含了每个订单的总价。
2.  **第二步：根据订单总价计算折扣**

    接下来，你可以使用第一步计算的订单总价数据，根据某种规则计算订单的折扣，然后将结果存储到一个新的临时表或者使用WITH子句（CTE）：

    ```sql
    WITH order_totals AS (
        SELECT order_id, SUM(quantity * price) AS total_price
        FROM order_items
        GROUP BY order_id
    ),
    discounted_orders AS (
        SELECT order_id, total_price, total_price * 0.9 AS discounted_price
        FROM order_totals
        WHERE total_price > 1000
    )
    SELECT * FROM discounted_orders;
    ```

    在这个步骤中，`discounted_orders`临时表包含了根据某种规则计算折扣后的订单数据。
3.  **第三步：根据折扣后的订单计算总销售额**

    最后，你可以使用第二步计算的折扣后的订单数据，计算总销售额或者其他指标，并输出最终的计算结果：

    ```sql
    WITH order_totals AS (
        SELECT order_id, SUM(quantity * price) AS total_price
        FROM order_items
        GROUP BY order_id
    ),
    discounted_orders AS (
        SELECT order_id, total_price, total_price * 0.9 AS discounted_price
        FROM order_totals
        WHERE total_price > 1000
    ),
    sales_summary AS (
        SELECT SUM(discounted_price) AS total_sales
        FROM discounted_orders
    )
    SELECT * FROM sales_summary;
    ```

    在这个步骤中，`sales_summary`临时表包含了根据折扣后的订单数据计算得到的总销售额。

以上就是一个使用分步骤计算的示例。你可以根据实际需求和计算逻辑，结合使用临时表、WITH子句（CTE）和多个查询来实现分步骤计算。这种方法有助于将复杂的计算过程拆分为更简单、可管理的步骤，并提高代码的可读性和维护性。
