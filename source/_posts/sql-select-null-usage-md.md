title: sql-select-null-usage.md
date: 2019-05-08 11:01:40
categories: programming
tags: sql
---
## select null example

I saw a sql code snippet as this:
```sql
 select *
    from (
  select v.empno,v.ename,v.job,v.mgr,v.hiredate,
         v.sal,v.comm,v.deptno, count(*) as cnt
    from v
  group by empno,ename,job,mgr,hiredate,
           sal,comm,deptno
        ) v
  where not exists (
 select null
   from (
 select e.empno,e.ename,e.job,e.mgr,e.hiredate,
        e.sal,e.comm,e.deptno, count(*) as cnt
   from emp e
  group by empno,ename,job,mgr,hiredate,
           sal,comm,deptno
        ) e
  where v.empno     = e.empno
    and v.ename     = e.ename
    and v.job       = e.job
    and coalesce(v.mgr,0) = coalesce(e.mgr,0)
    and v.hiredate  = e.hiredate
    and v.sal       = e.sal
    and v.deptno    = e.deptno
    and v.cnt       = e.cnt
    and coalesce(v.comm,0) = coalesce(e.comm,0)
)
```

why we use `select null` here, I tried with `select *`, the result is the same.

## usage explaination

the above sql do one thing : find the row which exists in view `v` but not in view `e`. but why we use `select null` here?

the main reason is that we don't need really select any column from `v`, all we want is what the `where` clause does, we compare
`v.empno = e.empno` etc. After comparision, we exclude v's row which is in 'e' also.  you can  do `select 1` instead here, they have the same performance. but if you use `select *`, it would bring the data of view `e` although we don't display `e's` column in our
result. Then the performance would be worse than `select null` or `select 1`


here is a post [select null](http://bradsruminations.blogspot.com/2009/09/age-old-select-vs-select-1-debate.html) which has in-depth explaination.
