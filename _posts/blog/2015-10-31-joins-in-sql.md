---
layout: post
title: "Joins in SQL"
author: annu_yadav
modified:
share: true
categories: blog
excerpt:
tags: []
image:
  feature:
date: 2015-10-31T10:16:00+0530
comments: true
---

An SQL Joins are used to combine rows from two or more tables, based on a common field between them.

Lets consider two tables suppliers and orders for explaining the joins.

###### Suppliers:
<dl>
       <table style="border-collapse:collapse; width:50%;">
             <tr style="background-color: #f2f2f2;">
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">supplier_id</th>
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">supplier_name</th>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">10000</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">IBM</td>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">10001</td>
                 <td style="border: 1px solid #ddd; padding: 7px 13px;">Hewlett Packard</td>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">10002</td>
                 <td style="border: 1px solid #ddd; padding: 7px 13px;">Microsoft</td>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">10003</td>
                 <td style="border: 1px solid #ddd; padding: 7px 13px;">NVIDIA</td>
             </tr>
       </table>
</dl>

###### Orders:
<dl>
       <table style="border-collapse:collapse; width:50%;">
             <tr style="background-color: #f2f2f2;">
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">order_id</th>
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">supplier_id</th>
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">order_date</th>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">500125</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">10000</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">2015/09/12</td>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">500126</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">10001</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">2015/09/13</td>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">500127</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">10004</td>
                 <td style="border: 1px solid #ddd; padding: 7px 13px;">2015/09/14</td>
             </tr>
       </table>
</dl>

##### INNER JOIN (or sometimes called simple join or Equi join):
The INNER JOIN selects all rows from both tables as long as there is a match between the columns in both tables. The 
output of the join are records which match in both the suppliers and orders tables, i.e. all suppliers who have some orders:

{% include image.html img="assets/images/img_innerjoin.gif" title="inner join" %}

Syntax:
{% highlight sql %}
SELECT column_name(s)
FROM table1
INNER JOIN table2
ON table1.column_name=table2.column_name;
{% endhighlight %}

or 

{% highlight sql %}
SELECT column_name(s)
FROM table1
JOIN table2
ON table1.column_name=table2.column_name;
{% endhighlight %}

Example:
{% highlight sql %}
SELECT suppliers.supplier_id, suppliers.supplier_name, orders.order_date
FROM suppliers
INNER JOIN orders
ON suppliers.supplier_id = orders.supplier_id;
{% endhighlight %}

the result is:

<dl>
       <table style="border-collapse:collapse; width:50%;">
             <tr style="background-color: #f2f2f2;">
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">supplier_id</th>
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">supplier_name</th>
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">order_date</th>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">10000</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">IBM</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">2015/09/12</td>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">10001</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">Hewlett Packard</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">2015/09/13</td>
             </tr>
       </table>
</dl>


##### LEFT JOIN (also known as LEFT OUTER JOIN)
The LEFT JOIN returns all rows from the left table (table1), with the matching rows in the right table (table2). The result 
is NULL in the right side when there is no match. This will return all rows from the suppliers table and only those rows 
from the orders table where the joined fields are equal. If a supplier_id value in the suppliers table does not exist in 
the orders table, all fields in the orders table will display as <null> in the result set.

{% include image.html img="assets/images/img_leftjoin.gif" title="inner join" %}

Syntax:
{% highlight sql %}
SELECT column_name(s)
FROM table1
LEFT JOIN table2
ON table1.column_name=table2.column_name;
{% endhighlight %}

Example: 
{% highlight sql %}
SELECT suppliers.supplier_id, suppliers.supplier_name, orders.order_date
FROM suppliers
LEFT JOIN orders
ON suppliers.supplier_id = orders.supplier_id;
{% endhighlight %}

Result:
<dl>
       <table style="border-collapse:collapse; width:50%;">
             <tr style="background-color: #f2f2f2;">
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">supplier_id</th>
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">supplier_name</th>
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">order_date</th>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">10000</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">IBM</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">2015/09/12</td>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">10001</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">Hewlett Packard</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">2015/09/13</td>
             </tr>
             <tr>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">10002</td>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">Microsoft</td>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">(null)</td>
             </tr>
             <tr>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">10003</td>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">NVIDIA</td>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">(null)</td>
             </tr>
       </table>
</dl>



##### RIGHT JOIN (also known as RIGHT OUTER JOIN)
The RIGHT JOIN returns all rows from the right table (table2), with the matching rows in the left table (table1). The 
result is NULL in the left side when there is no match. This will return all rows from the orders table and only those 
rows from the suppliers table where the joined fields are equal. If a supplier_id value in the orders table does not 
exist in the suppliers table, all fields in the suppliers table will display as <null> in the result set.

{% include image.html img="assets/images/img_rightjoin.gif" title="inner join" %}

Syntax:
{% highlight sql %}
SELECT column_name(s)
FROM table1
RIGHT JOIN table2
ON table1.column_name=table2.column_name;
{% endhighlight %}

Example:
{% highlight sql %}
SELECT orders.order_id, orders.order_date, suppliers.supplier_name
FROM suppliers
RIGHT JOIN orders
ON suppliers.supplier_id = orders.supplier_id;
{% endhighlight %}

Result:
<dl>
       <table style="border-collapse:collapse; width:50%;">
             <tr style="background-color: #f2f2f2;">
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">order_id</th>
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">order_date</th>
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">supplier_name</th>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">500125</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">2015/09/12</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">IBM</td>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">500126</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">2015/09/13</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">Hewlett Packard</td>
             </tr>
             <tr>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">500127</td>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">2015/09/14</td>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">(null)</td>
             </tr>
       </table>
</dl>


##### OUTER JOIN (or FULL OUTER JOIN)
The OUTER JOIN returns all rows from the left table (table1) and from the right table (table2). The OUTER JOIN keyword 
combines the result of both LEFT and RIGHT joins. This will return all rows from the orders table and suppliers table.
      
{% include image.html img="assets/images/img_fulljoin.gif" title="inner join" %}

Syntax:
{% highlight sql %}
SELECT column_name(s)
FROM table1
OUTER JOIN table2
ON table1.column_name=table2.column_name;
{% endhighlight %}

Example: 
{% highlight sql %}
SELECT orders.order_id, orders.order_date, suppliers.supplier_name
FROM suppliers
OUTER JOIN orders
ON suppliers.supplier_id = orders.supplier_id;
{% endhighlight %}

Result:
<dl>
       <table style="border-collapse:collapse; width:50%;">
             <tr style="background-color: #f2f2f2;">
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">order_id</th>
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">order_date</th>
                 <th style="border: 1px solid #ddd; padding: 6px 13px;">supplier_name</th>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">500125</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">2015/09/12</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">IBM</td>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">500126</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">2015/09/13</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">Hewlett Packard</td>
             </tr>
             <tr>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">(null)</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">(null)</td>
                 <td style="border: 1px solid #ddd; padding: 6px 13px;">Microsoft</td>
             </tr>
             <tr>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">(null)</td>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">(null)</td>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">NVIDIA</td>
             </tr>
             <tr>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">500127</td>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">2015/09/14</td>
                <td style="border: 1px solid #ddd; padding: 6px 13px;">(null)</td>
             </tr>
       </table>
</dl>
