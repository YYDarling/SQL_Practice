# SQL Practice

[GitHub - YYDarling/SQL_Practice](https://github.com/YYDarling/SQL_Practice)

[https://github.com/YYDarling/SQL_Practice](https://github.com/YYDarling/SQL_Practice)

![shot 2023-07-07 at 11.03.37.jpg](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/shot_2023-07-07_at_11.03.37.jpg)

[图解SQL面试题：经典50题](https://zhuanlan.zhihu.com/p/38354000)

[SQL经典50题 - ikventure - 博客园](https://www.cnblogs.com/ikventure/p/15933952.html)

[SQL面试必会50题](https://zhuanlan.zhihu.com/p/43289968)

# General Learning 50:

# Easy

## 1. Query

- 查询姓“孟”老师的个数
    
    ```sql
    --查询姓“孟”老师的个数--
    select count(教姓名)
    from teacher
    where 教师姓名 like '孟%';
    
    ```
    

## 2. Sum/Count(Distinct)

- 查询课程编号为“0002”的总成绩
    
    ```sql
    --查询课程编号为“0002”的总成绩
    --*
    --分析思路
    --select 查询结果 [总成绩:汇总函数sum]
    --from 从哪张表中查找数据[成绩表score]
    --where 查询条件 [课程号是0002]
    --*/
    select sum(成绩)
    from score
    where 课程号 = '0002';
    
    ```
    
- 查询选了课程的学生人数
    
    ```sql
    --查询选了课程的学生人数
    
    /*
    这个题目翻译成大白话就是：查询有多少人选了课程
    select 学号，成绩表里学号有重复值需要去掉
    from 从课程表查找score;
    */
    select count(distinct 学号) as 学生人数 
    from score;
    ```
    

## 3. Group

- 查询各科成绩最高和最低的分， 以如下的形式显示：课程号，最高分，最低分
    
    ```sql
    --查询各科成绩最高和最低的分， 以如下的形式显示：课程号，最高分，最低分
    
    /*
    分析思路
    select 查询结果 [课程ID：是课程号的别名,最高分：max(成绩) ,最低分：min(成绩)]
    from 从哪张表中查找数据 [成绩表score]
    where 查询条件 [没有]
    group by 分组 [各科成绩：也就是每门课程的成绩，需要按课程号分组];
    */
    
    select class_id, max(score) as h_score, min(score) as l_score
    from score
    group by class_id
    
    ```
    
- 查询每门课程被选修的学生数
    
    ```sql
    --查询每门课程被选修的学生数
    
    /*
    分析思路
    select 查询结果 [课程号，选修该课程的学生数：汇总函数count]
    from 从哪张表中查找数据 [成绩表score]
    where 查询条件 [没有]
    group by 分组 [每门课程：按课程号分组];
    */
    
    select class_id, count(student_id)
    from score
    group by class_id
    
    ```
    
- 查询男生、女生人数
    
    ```sql
    --查询男生、女生人数
    
    /*
    分析思路
    select 查询结果 [性别，对应性别的人数：汇总函数count]
    from 从哪张表中查找数据 [性别在学生表中，所以查找的是学生表student]
    where 查询条件 [没有]
    group by 分组 [男生、女生人数：按性别分组]
    having 对分组结果指定条件 [没有]
    order by 对查询结果排序[没有];
    */
    
    select sex, count(*)
    from student
    group by sex
    ```
    

## 4. Group with condition

- 查询平均成绩大于60分学生的学号和平均成绩
    
    ```sql
    --查询平均成绩大于60分学生的学号和平均成绩
    
    /* 
    题目翻译成大白话：
    平均成绩：展开来说就是计算每个学生的平均成绩
    这里涉及到“每个”就是要分组了
    平均成绩大于60分，就是对分组结果指定条件
    
    分析思路
    select 查询结果 [学号，平均成绩：汇总函数avg(成绩)]
    from 从哪张表中查找数据 [成绩在成绩表中，所以查找的是成绩表score]
    where 查询条件 [没有]
    group by 分组 [平均成绩：先按学号分组，再计算平均成绩]
    having 对分组结果指定条件 [平均成绩大于60分]
    */
    
    select student_id, avg(score)
    from score
    group by student_id
    having avg(score) > 60
    ```
    
- 查询至少选修两门课程的学生学号
    
    ```sql
    --查询至少选修两门课程的学生学号
    
    /* 
    翻译成大白话：
    第1步，需要先计算出每个学生选修的课程数据，需要按学号分组
    第2步，至少选修两门课程：也就是每个学生选修课程数目>=2，对分组结果指定条件
    
    分析思路
    select 查询结果 [学号,每个学生选修课程数目：汇总函数count]
    from 从哪张表中查找数据 [课程的学生学号：课程表score]
    where 查询条件 [至少选修两门课程：需要先计算出每个学生选修了多少门课，需要用分组，所以这里没有where子句]
    group by 分组 [每个学生选修课程数目：按课程号分组，然后用汇总函数count计算出选修了多少门课]
    having 对分组结果指定条件 [至少选修两门课程：每个学生选修课程数目>=2]
    */
    
    select student_id, count(class_id) as selecting_course_num
    from score
    group by student_id
    having count(class_id) >= 2;
    ```
    
- 查询同名同姓学生名单并统计同名人数
    
    ```sql
    --查询同名同姓学生名单并统计同名人数
    
    /* 
    翻译成大白话，问题解析：
    1）查找出姓名相同的学生有谁，每个姓名相同学生的人数
    查询结果：姓名,人数
    条件：怎么算姓名相同？按姓名分组后人数大于等于2，因为同名的人数大于等于2
    分析思路
    select 查询结果 [姓名,人数：汇总函数count(*)]
    from 从哪张表中查找数据 [学生表student]
    where 查询条件 [没有]
    group by 分组 [姓名相同：按姓名分组]
    having 对分组结果指定条件 [姓名相同：count(*)>=2]
    order by 对查询结果排序[没有];
    */
    
    select name, count(*) as number
    from student
    group by name
    having count(*) >= 2;
    ```
    
- 查询不及格的课程并按课程号从大到小排列
    
    ```sql
    --查询不及格的课程并按课程号从大到小排列
    
    /* 
    分析思路
    select 查询结果 [课程号]
    from 从哪张表中查找数据 [成绩表score]
    where 查询条件 [不及格：成绩 <60]
    group by 分组 [没有]
    having 对分组结果指定条件 [没有]
    order by 对查询结果排序[课程号从大到小排列：降序desc];
    */
    
    select class_id
    from score
    where score < 60
    order by class_id desc;
    ```
    
- 查询每门课程的平均成绩，结果按平均成绩升序排序，平均成绩相同时，按课程号降序排列
    
    ```sql
    --查询每门课程的平均成绩，结果按平均成绩升序排序，平均成绩相同时，按课程号降序排列
    
    /* 
    分析思路
    select 查询结果 [课程号,平均成绩：汇总函数avg(成绩)]
    from 从哪张表中查找数据 [成绩表score]
    where 查询条件 [没有]
    group by 分组 [每门课程：按课程号分组]
    having 对分组结果指定条件 [没有]
    order by 对查询结果排序[按平均成绩升序排序:asc，平均成绩相同时，按课程号降序排列:desc];
    */
    
    select class_id, avg(score) as avg_score
    from score
    group by class_id
    order by avg_score asc, class_id desc;
    ```
    
- 检索课程编号为“0004”且分数小于60的学生学号，结果按按分数降序排列
    
    ```sql
    --检索课程编号为“0004”且分数小于60的学生学号，结果按按分数降序排列
    /* 
    分析思路
    select 查询结果 []
    from 从哪张表中查找数据 [成绩表score]
    where 查询条件 [课程编号为“04”且分数小于60]
    group by 分组 [没有]
    having 对分组结果指定条件 []
    order by 对查询结果排序[查询结果按按分数降序排列];
    */
    
    select student_id
    from score
    where class_id = '04' and score < 60
    order by score desc;
    ```
    
- 统计每门课程的学生选修人数(超过2人的课程才统计). 要求输出课程号和选修人数，查询结果按人数降序排序，若人数相同，按课程号升序排序
    
    ```sql
    --统计每门课程的学生选修人数(超过2人的课程才统计). 
    --要求输出课程号和选修人数，查询结果按人数降序排序，若人数相同，按课程号升序排序
    /* 
    分析思路
    select 查询结果 [要求输出课程号和选修人数]
    from 从哪张表中查找数据 []
    where 查询条件 []
    group by 分组 [每门课程：按课程号分组]
    having 对分组结果指定条件 [学生选修人数(超过2人的课程才统计)：每门课程学生人数>2]
    order by 对查询结果排序[查询结果按人数降序排序，若人数相同，按课程号升序排序];
    */
    
    select class_id, count(student_id) as 'student_num'
    from score
    group by class_id
    having count(student_id) > 2
    order by count(student_id) desc, class_id asc;
    ```
    
- 查询两门以上不及格课程的同学的学号及其平均成绩
    
    ```sql
    --查询两门以上不及格课程的同学的学号及其平均成绩
    
    /*
    分析思路
    先分解题目：
    1）[两门以上][不及格课程]限制条件
    2）[同学的学号及其平均成绩]，也就是每个学生的平均成绩，显示学号，平均成绩
    分析过程：
    第1步：得到每个学生的平均成绩，显示学号，平均成绩
    第2步：再加上限制条件：
    1）不及格课程
    2）两门以上[不及格课程]：课程数目>2
    
    /* 
    第1步：得到每个学生的平均成绩，显示学号，平均成绩
    select 查询结果 [学号,平均成绩：汇总函数avg(成绩)]
    from 从哪张表中查找数据 [涉及到成绩：成绩表score]
    where 查询条件 [没有]
    group by 分组 [每个学生的平均：按学号分组]
    having 对分组结果指定条件 [没有]
    order by 对查询结果排序[没有];
    */
    select student_id, avg(score) as avg_score
    from score
    group by student_id
    
    /* 
    第2步：再加上限制条件：
    1）不及格课程
    2）两门以上[不及格课程]
    select 查询结果 [学号,平均成绩：汇总函数avg(成绩)]
    from 从哪张表中查找数据 [涉及到成绩：成绩表score]
    where 查询条件 [限制条件：不及格课程，平均成绩<60]
    group by 分组 [每个学生的平均：按学号分组]
    having 对分组结果指定条件 [限制条件：课程数目>2,汇总函数count(课程号)>2]
    order by 对查询结果排序[没有];
    */
    
    select student_id, avg(score) as avg_score
    from score
    group by student_id
    where score < 60
    group by student_id
    having count(class_id) > 2;
    
    ```
    

## 5. Sum with Condition

- 查询学生的总成绩并进行排名. (分组查询)
    
    ```sql
    --查询学生的总成绩并进行排名
    
    --
    ---/*
    --分析思路
    --select 查询结果 [总成绩：sum(成绩), 学号]
    --from 从哪张表中查找数据 [成绩表score]
    --where 查询条件 [没有]
    --group by 分组 [学生的总成绩：按照每个学生学号进行分组]
    --order by 排序 [按照总成绩进行排序：sum(成绩)];
    --/*
    
    select student_id, sum(score) 
    from score
    group by student_id
    order by sum(score);
    
    ```
    
- 查询平均成绩大于60分的学生的学号和平均成绩
    
    ```sql
    --查询平均成绩大于60分的学生的学号和平均成绩
    
    select student_id, avg(score) 
    from score
    group by student_id
    having avg(score) > 60;
    ```
    

# 复杂查询

- 查询所有课程成绩小于60分学生的学号、姓名 (子查询 In ): Table: student, score
    
    ```sql
    
    /* 
    【知识点】子查询
    
    1.翻译成大白话
    1）查询结果：学生学号，姓名
    2）查询条件：所有课程成绩 < 60 的学生，需要从成绩表里查找，用到子查询
    
    第1步，写子查询（所有课程成绩 < 60 的学生）
    select 查询结果[学号]
    from 从哪张表中查找数据[成绩表：score]
    where 查询条件[成绩 < 60]
    group by 分组[没有]
    having 对分组结果指定条件[没有]
    order by 对查询结果排序[没有]
    limit 从查询结果中取出指定行[没有];
    
    select 学号 
    from student
    where score < 60;
    
    第2步，查询结果：学生学号，姓名，条件是前面1步查到的学号
    
    select 查询结果[学号,姓名]
    from 从哪张表中查找数据[学生表:student]
    where 查询条件[用到运算符in]
    group by 分组[没有]
    having 对分组结果指定条件[没有]
    order by 对查询结果排序[没有]
    limit 从查询结果中取出指定行[没有];
    
    分析思路
    select 查询结果 [学号，平均成绩：汇总函数avg(成绩)]
    from 从哪张表中查找数据 [成绩在成绩表中，所以查找的是成绩表score]
    where 查询条件 [没有]
    group by 分组 [平均成绩：先按学号分组，再计算平均成绩]
    having 对分组结果指定条件 [平均成绩大于60分]
    */
    
    --step 1: 写子查询（所有课程成绩 < 60 的学生）
    select student_id 
    from score
    where score < 60
    
    --step 2: 查询结果：学生学号，姓名，条件是前面1步查到的学号
    select student_id, name
    from student
    where student_id in (select student_id from score where score < 60);
    
    ```
    
- 查询没有学全所有课的学生的学号、姓名 (子查询 In ): Table: student, score
    
    ```sql
    /*
    查找出学号，条件：没有学全所有课，也就是该学生选修的课程数 < 总的课程数
    【考察知识点】in，子查询
    */
    
    select student_id, name
    from student
    where student_id in (
    	select student_id from score
    	group by student_id
    	having count(class_id) < (select count(class_id) from course)
    );
    ```
    
- 查询出只选修了两门课程的全部学生的学号和姓名
    
    ```sql
    select student_id, name from student
    where student_id in (
    	select student_id from score
    	group by student_id
      having count(class_id) = 2
    );
    ```
    
- 1990年出生的学生名单
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled.png)
    
    ```sql
    select student_id, name from student
    where year(date_of_birth) = 1990;
    ```
    
- 查询各科成绩前两名的记录(Top N 问题)：**这类问题其实就是常见的：分组取每组最大值、最小值，每组最大的N条（top N）记录。**
    
    [sql面试题：topN问题](https://mp.weixin.qq.com/s/MuxjlFV0gi1GydOrYfiSeQ)
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%201.png)
    
    - **分组取每组最大值**
        - **按课程号分组取成绩最大值所在行的数据**
            
            ```sql
            /*错误*/
            我们可以使用分组（group by）和汇总函数得到每个组里的一个值（最大值，最小值，平均值等）。
            但是无法得到成绩最大值所在行的数据。
            
            select class_id, max(score) as max_score
            from score
            group by class_id;
            ```
            
            ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%202.png)
            
            我们可以使用关联子查询来实现：先找出最大成绩的class_id—→ 再找出最大成绩对应的信息
            
            ```sql
            select * 
            from score as a
            where score = (
            	select max(score) from score as b
            	where b.class_id = a.class_id
            );
            ```
            
            ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%203.png)
            
            上面查询结果课程号“0001”有2行数据，是因为最大成绩80有2个
            
    - **分组取每组最小值**
        - **按课程号分组取成绩最小值所在行的数据**
            
            ```sql
            --same as below
            select * 
            from score as a 
            where score = (
            	select min(score) from score as b 
            	where b.class_id = a.class_id
            );
            ```
            
    - **每组最大的N条记录**
        - **查询各科成绩前两名的记录**
            
            ```sql
            --分组取每组最大值
            /*
            第1步，查出有哪些组
            我们可以按课程号分组，查询出有哪些组，对应这个问题里就是有哪些课程号
            */
            
            select class_id, max(score) as max_id from score
            group by class_id
            ```
            
            ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%204.png)
            
            ```sql
            /*
            第2步：先使用order by子句按成绩降序排序（desc），
            然后使用limt子句返回topN（对应这个问题返回的成绩前两名）
            */
            
            select * from score
            where class_id = '0001'
            order by score desc
            limit 2;
             
            ```
            
            同样的，可以写出其他组的（其他课程号）取出成绩前2名的sql
            
            ```sql
            /*
            第3步，使用union all 将每组选出的数据合并到一起
            */
            
            (select * from score where class_id = '0001' order by score desc limit 2)
            union all
            (select * from score where class_id ='0002' order by score desc limit 2)
            union all 
            (select * from score where class_id = '0003' order by score desc limit 2);
            ```
            
            ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%205.png)
            
    
- 查询各学生的年龄（精确到月份）
    
    ```sql
    /*
    【知识点】时间格式转化
    */
    
    select student_id, timestampdiff(month ,出生日期 ,now())/12 
    from student ;
    
    mysql> SELECT TIMESTAMPDIFF(MONTH,'2003-02-01','2003-05-01');   // 计算两个时间相隔多少月
            -> 3
    ```
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled.png)
    
- 查询本月过生日的学生
    
    本月过生日，要得到本月。然后和出生日期列中的月份比较
    
    出生日期这一列中的月份值可以用进行比较month(出生日期)得到日期的月份
    
    ```sql
    /*
    可以先用current_date得到当前日期，然后把current_date作为参数传入month里，
    也就是month(current_date)就可以得到本月
    */
    
    -- 找出本月过生日的学生
    select * 
    from student 
    where month(出生日期)= month(current_date);
    ```
    

## 1. 多表查询（Join）

- 查询所有学生的学号、姓名、选课数、总成绩(left join ‘On’): 2 table
    
    ```sql
    select a.student_id, a.student_name, count(b.class_id) as class_number, sum(b.score) as to total_score
    from student as a 
    left join score as b 
    on a.student_id = b.student_id
    group by a.student_id
    ```
    
- 查询平均成绩大于85的所有学生的学号、姓名和平均成绩(left Join ‘On’): 2 table
    
    ```sql
    select a.student_id, a.name, avg(b.score) as avg_score
    from student as a
    left join score as b
    on a.student_id = b.student_id
    group by a. student_id
    having avg(b.score) > 85;
    ```
    
- 查询学生的选课情况：学号，姓名，课程号，课程名称(Inner Join ‘On’): 3 tables
    
    ```sql
    --三个表； student, score, class
    
    select a.student_id, a.name, c.class_id, c.class_name
    from student as a
    inner join score as b on a.student_id = b.student_id
    inner join course as c on b.class_id = c.class_id;
    ```
    
- 查询出每门课程的及格人数和不及格人数(Case When)
    
    ```sql
    select class_id
    sum(case when score >= 60 then 1 else 0 end) as pass_num,
    sum(case when score < 60 then 1 else 0 end) as fail_num 
    from score
    group by class_id
    ```
    
- 使用分段[100-85],[85-70],[70-60],[<60]来统计各科成绩，分别统计：各分数段人数，课程号和课程名称(Case When) : 2 table
    
    ```sql
    select a.class_id, b.class_name
    sum(case when score between 85 and 100 then 1 else 0 end) as '[100-85]',
    sum(case when score >= 70 and score < 85 then 1 else 0 end) as '[85-70]',
    sum(case when score >= 60 and score < 70 then 1 else 0 end) as '[70-60]',
    sum(case when score < 60 then 1 else 0 end) as '[<60]'
    from score as a
    right join course as b 
    on a.class_id = b.class_id
    group by a.class_id, b.class_id
    ```
    
- 查询课程编号为0003且课程成绩在80分以上的学生的学号和姓名（Inner Join）
    
    ```sql
    --score, student 2 table
    
    select a.student_id, a.student_name
    from student as a
    inner join score as b
    where b.class_id = '0003' AND b.score > 80; 
    ```
    

SQL INNER JOIN 关键字
INNER JOIN 关键字在表中存在至少一个匹配时返回行。

[https://www.runoob.com/sql/sql-join-inner.html](https://www.runoob.com/sql/sql-join-inner.html)

# 行列互换

[sql面试题：行列如何互换？](https://mp.weixin.qq.com/s/6Kll4Q6Xp37i2PiLUh4cMA)

![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%206.png)

想要转换成为的样子

![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%207.png)

```sql
--第1步，使用常量列输出目标表的结构

select student_id, 'class_id_0001', 'class_id_0002', 'class_id_0003'
from score 
```

![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%208.png)

```sql
--第2步，使用case表达式，替换常量列为对应的成绩
select student_id, 
(case class_id when '0001' then score else 0 end) as 'class_id_0001'
(case class_id when '0002' then score else 0 end) as 'class_id_0002'
(case class_id when '0003' then score else 0 end) as 'class_id_0003' 
from score;
```

![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%209.png)

在这个查询结果中，每一行表示了某个学生某一门课程的成绩。比如第一行是'学号0001'选修'课程号00001'的成绩，而其他两列的'课程号0002'和'课程号0003'成绩为0。

每个学生选修某门课程的成绩在下图的每个方块内。我们可以通过分组，取出每门课程的成绩。

![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2010.png)

```sql
--第3关，分组
--分组，并使用最大值函数max取出上图每个方块里的最大值

select student_id, 
max(case class_id when '0001' then score else 0 end) as 'class_id_0001',
max(case class_id when '0002' then score else 0 end) as 'class_id_0002',
max(case class_id when '0003' then score else 0 end) as 'class_id_0003'
from score
group by student_id;
```

![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2011.png)

## 2. 多表连接

- 检索"0001"课程分数小于60，按分数降序排列的学生信息
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2012.png)
    
    ```sql
    select a.*, b.score
    from student as a
    inner join score as b
    on a.student_id = b.student_id
    where b.score < 60 AND b.class_id = '0001'
    order by b.score desc
    ```
    
- 查询不同老师所教不同课程平均分从高到低显示: 分组+条件+排序+多表连接
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2013.png)
    
    ```sql
    select 
    from teacher as a
    Inner join course as b
    on a.teacher_id = b.teacher_id
    inner join score as c 
    on b.class_id = c.class_id
    group by a.teacher_name
    order by avg(c.score) desc;
    ```
    
- 查询课程名称为"数学"，且分数低于60的学生姓名和分数3 tables
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2014.png)
    
    ```sql
    --student, score, class : 3 tables
    
    select 
    from student as a
    inner join score as b
    on a.clss_id = b.class_id
    inner join course as c 
    on b.student_id = c.student_id
    where b.score < 60 AND c.class_id = 'Math'
    ```
    
- 查询任何一门课程成绩在70分以上的姓名、课程名称和分数（与上题类似）
    
    ```sql
    select
    from student as a 
    inner join score as b
    on a.student_id = b.student_id
    inner join course as c b.class_id = c.class_id
    where b.score > 70;
    ```
    
- 查询两门及其以上不及格课程的同学的学号，姓名及其平均成绩【知识点】分组+条件+多表连接
    
    翻译成大白话:计算每个学号不及格分数个数，筛选出大于2个的学号并找出姓名，平均成绩，思路
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2015.png)
    
    ```sql
    --score, student,  
    
    select b.name, avg(a.score), a.student_id
    from score as a
    inner join student as b
    on a.student_id = b.student_id
    where a.score < 60
    group by a.student_id
    having count(a.student_id) >= 2;
    ```
    
- 查询不同课程成绩相同的学生的学生编号、课程编号、学生成绩
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2016.png)
    
    ```sql
    select distinct a.student_id, a.score, a.class_id
    from score as a
    inner join score as b
    on a.student_id = b.student_id
    where a.score = b.score AND a.class_id != b.class_id;
    ```
    
- 查询课程编号为“0001”的课程比“0002”的课程成绩高的所有学生的学号 多表连接+条件
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2017.png)
    
    ```sql
    select a.student_id
    from 
    (select student_id, score from score where class_id = 01) as a
    inner join
    (select student_id, score from score where class_id = 02) as b
    on a.student_id = b.student_id
    inner join student as c on c.student_id = a.student_id
    where a.score > b.score;
    ```
    
- 查询学过编号为“0001”的课程并且也学过编号为“0002”的课程的学生的学号、姓名
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2018.png)
    
    ```sql
    select a.studrnt_id
    from 
    (select student_id, score from score where class_id = 01) as a
    inner join
    (select student_id, score from score where class_id = 02) as b
    on a.student_id = b.student_id
    inner join student as c on c.student_id = a. student_id
    where a.score > b.score
    ```
    
- 查询学过“孟扎扎”老师所教的所有课的同学的学号、姓名
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2019.png)
    
    ```sql
    select s.student_id, s.student_name
    from student as s
    inner join score as a
    on s.student_id = a.student_id
    inner join course as b on a.class_id = b.class_id
    inner join teacher as c on b.teacher_id = c.teacher_id
    where c.teacher_name = 'Mengzhazha';
    ```
    
- 查询没学过"孟扎扎"老师讲授的任一门课程的学生姓名（与上题类似，"没学过"用not in来实现)
    
    ```sql
    select s.name, s.student_id
    from student
    where student_id not in (
    select a.student_id
    from student as a
    inner join score as b
    on a.student_id = b.student_id
    inner join course as c on b.class_id = c.class_id
    inner join teacher as d on c.teacher_id = d.teacher_id
    where d.teacher_name = 'Mengzhazha');
    ```
    
- 查询没学过“孟扎扎”老师课的学生的学号、姓名（与上题类似）???
    
    ```sql
    select student_id, student_name
    from student
    where student_id not in 
    (select )
    ```
    
- 查询选修“孟扎扎”老师所授课程的学生中成绩最高的学生姓名及其成绩（与上题类似,用成绩排名，用 limit 1得出最高一个）
    
    ```sql
    select a.student_name, b.student_score
    from student as a
    inner join score as b on a.student_id = b.student_id
    inner join course as c on b.class_id = c.class_id
    inner join teacher as d on c.teacher_id = d.teacher_id
    where d.teacher_name = 'Mengzhazha'
    order by b.score desc limit 1;
    ```
    
- 查询至少有一门课与学号为“0001”的学生所学课程相同的学生的学号和姓名
    
    ```sql
    select student_id, student_name
    from student
    where student_id in 
    (select distinct(student_id) from score where class_id in 
    (select class_id from score where student_id = 0001))
    AND student_id != 0001;
    ```
    
- 按平均成绩从高到低显示所有学生的所有课程的成绩以及平均成绩 多表连接 新建字段
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2020.png)
    
    ```sql
    select a.student_id, avg(a.score), 
    max(case when b.class_name = 'Math' then a.score else null end) as 'Math'
    max(case when b.class_name = 'Chinese' then a.score else null end) as 'Chinese'
    max(case when b.class_name = 'English' then a.score else null end) as 'English'
    from score as a
    inner join course as b 
    on a.class_id = b.class_id
    group by a.student_id
    ```
    

# **SQL高级功能：窗口函数**

[SQL之“高级功能”](https://zhuanlan.zhihu.com/p/110149308)

- 查询学生平均成绩及其名次 row_number() over
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2021.png)
    
    ```sql
    select student_id, avg(score)
    row_number () over (order by avg(score) desc)
    from score
    group by student_id;
    ```
    
- 按各科成绩进行排序，并显示排名
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2022.png)
    
    ```sql
    select class_id
    row_number () over (partition by class_id order by score)
    from score;
    ```
    
- 查询每门功成绩最好的前两名学生姓名
    
    ![Untitled](SQL%20Practice%200c752e1f80b744eea57cbb3f784bd334/Untitled%2023.png)
    
    ```sql
    select a.clss_id, b.name, a.score, a.ranking from (
    	select class_id, student_id, score
    	row_number () over(partition by class_id order by score desc) as ranking from score) as a
    inner join student as b
    on a.student_id = b.student_id
    where a.ranking < 3;
    ```
    
- 查询所有课程的成绩第2名到第3名的学生信息及该课程成绩
    
    ```sql
    select b.name, a.class_id, a.score
    from (
    select class_id, student_id, score 
    row_number () over (partition by class_id order by score desc) as ranking
    from score) as a
    inner join student as b 
    on a.student_id = b.student_id
    where a. ranking in (2, 3);
    
    ```
    
- 查询各科成绩前三名的记录（不考虑成绩并列情况）（与上一题相似）
    
    ```sql
    select b.name, a.class_id, a.score
    from (
    select class_id, student-id, score, 
    row_number () over (partition by class_id order by score desc) as 'ranking'
    from score ) as a
    inner join student as b
    on a.student_id = b.student_id
    where a.ranking < 4;
    ```
    

2022年下半年 去Florida所有的parcel ground 单子的

1. Order Number （nsorder）
2. Zip Code （shipto_zip）
3. Tracking Number (tracking_number)

我现在有四张表， 

Package: id(primary key), tracking_number

Order: id(primary key), nsorder_id, ship_state, shipto_zip, date 

Order_shipment:id(primary key), nsorder_id

shipment_package: id(primary key), package_id

我要查2023年上半年的order单子， 我应该怎么用SQL语言进行查询