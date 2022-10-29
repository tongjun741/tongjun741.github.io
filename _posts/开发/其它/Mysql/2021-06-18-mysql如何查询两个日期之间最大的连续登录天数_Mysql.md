---
tags:
    - 其它
    - Mysql
---

mysql如何查询两个日期之间最大的连续登录天数_Mysql

前言

最近工作中遇到一个需求，是根据用户连续记录天数来计算的，求出用户在一段时间内最大的连续记录时间，例如在 2016-01-01 和 2016-01-28 之间，如果用户在3号和4号都记录了，那么连续记录天数为2，如果用户在6号-10号每日都记录了，那么最大连续记录天数为5.

拿到这个需求的时候，说实话有点懵，第一想到的就是在代码中去统计，会用到循环，想到那么多个用户，并且时间跨度也有点大，比如15年到16年，两年时间，想想就有点恐怖。

解决方案

然后就把这个需求跟朋友说了，朋友也觉得有点难搞，后来通过网上一篇文章有了一些小思路。但是看得也是一知半解的，虽然经常写 sql 语句，但也是常用的那些增删改查，像这样使用的方式根本没用过，过了会，朋友又扔给我一条 sql 语句，就在该文章的基础上进行了修改，以符合我的项目需求的语句。

SELECT *
FROM (SELECT *
   FROM (
       SELECT
        uid,
        max(days)   lianxu_days,
        min(login_day) start_date,
        max(login_day) end_date
       FROM (SELECT
           uid,
           @cont_day :=
           (CASE
           WHEN (@last_uid = uid AND DATEDIFF(created_ts, @last_dt) = 1)
            THEN
             (@cont_day + 1)
           WHEN (@last_uid = uid AND DATEDIFF(created_ts, @last_dt) < 1)
            THEN
             (@cont_day + 0)
           ELSE
            1
           END)                       AS days,
           (@cont_ix := (@cont_ix + IF(@cont_day = 1, 1, 0))) AS cont_ix,
           @last_uid := uid,
           @last_dt := created_ts                login_day
          FROM (SELECT
              uid,
              DATE(created_ts) created_ts
             FROM plan_stage
             WHERE uid != 0
             ORDER BY uid, created_ts) AS t,
           (SELECT
            @last_uid := '',
            @last_dt := '',
            @cont_ix := 0,
            @cont_day := 0) AS t1
         ) AS t2
       GROUP BY uid, cont_ix
       HAVING lianxu_days > 10
      ) tmp
   ORDER BY lianxu_days DESC) ntmp
GROUP BY uid;

查询出来的结果如下图所示:

![2016102384853157.jpg](https://yunqi-tech.oss-cn-hangzhou.aliyuncs.com/2016102384853157.jpg?x-oss-process=image/watermark,image_aW1wb3J0LmpwZw==,g_se,x_1,y_1)

如果要查看单个人的，那么将 sql 语句中的 uid !=0 改成具体的值即可。

总结

以上就是这篇文章的全部内容了，希望本文的内容对大家学习或者使用sql语句能有所帮助，如果有疑问大家可以留言交流。



https://m.aliyun.com/yunqi/ziliao/102918

