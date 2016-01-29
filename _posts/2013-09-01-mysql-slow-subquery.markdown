---
layout: post
title:  "MySQL Slow Subquery using IN Syntax"
date:   2009-06-01 13:00:00
categories: mysql
author: "Jeff R."
summary: "Fix for extremely slow queries in MySQL using  WHERE IN ()."
published: true
---

I found a solution queries that contain a subquery taking forever to return a result in MySQL. 

For example a query in MySQL that uses the `IN()` syntax with a subquery like the following takes a few long time to return a result.

{% highlight  sql %}
select o.ack,o.product
from orders o
where o.ack in (
select distinct ack
from orders
where status in ('Shipped','Cancelled')
group by ack
having DATE_ADD( DATE_SUB( now( ) , INTERVAL DAYOFMONTH( now( ) ) - 1 DAY ) , INTERVAL -3 MONTH ) max(status_date)))
order by o.ack, o.line
{% endhighlight %}

If the same query is reworked to put the subquery in the from with a join it comes back almost instantly.

{% highlight  sql %}
select o.ack,o.product
from orders o
inner join (select distinct ack
from    orders
where    status in ('Shipped','Cancelled')
group by ack
having    DATE_ADD( DATE_SUB( now( ) , INTERVAL DAYOFMONTH( now( ) ) - 1 DAY ) , INTERVAL -3 MONTH ) > max(status_date)) old_o
on o.ack = old_o.ack
order by o.ack, o.line
{% endhighlight %}

It's interesting on Microsoft MSSQL that the `IN()` with a subquery is just as fast for me.


[bugs.mysql.com/bug.php ( Thanks Jeremy Pointer )](http://bugs.mysql.com/bug.php?id=4040)
