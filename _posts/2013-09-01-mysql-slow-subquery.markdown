---
layout: post
title:  "MySQL Slow Subquery using IN Syntax"
date:   2009-06-01 13:00:00
categories: mysql
---

I finally found the correct information today to figure out why some kinds of subqueries take forever to return a result in MySQL. If you are using a subquery with a MySQL `IN()` like this:
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
It takes a few minutes to return a result. If you take the same query and reorder it to put the subquery in the from with a join it comes back almost instanty :
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
It's interesting on MSSQL that the `IN()` subquery the first syntax is much faster.


[bugs.mysql.com/bug.php ( Thanks Jeremy Pointer )](http://bugs.mysql.com/bug.php?id=4040)
