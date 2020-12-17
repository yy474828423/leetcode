# 题目

sql

有一个courses 表 ，有: student (学生) 和 class (课程)。
请列出所有超过或等于5名学生的课。
例如,表:

+---------+------------+
| student | class      |
+---------+------------+
| A       | Math       |
| B       | English    |
| C       | Math       |
| D       | Biology    |
| E       | Math       |
| F       | Computer   |
| G       | Math       |
| H       | Math       |
| I       | Math       |
+---------+------------+


应该输出:

+---------+
| class   |
+---------+
| Math    |
+---------+


Note:
学生在每个课中不应被重复计算。


# 解题思路
```sql
#共三种写法
#最朴实的写法，共三层查询，先利用 DISTINCT 去掉重复记录得到表 A，再利用 GROUP BY 为 CLASS 分组，然
#后用 COUNT() 统计每组个数得到表 B，最后在最外层限定数量 >=5 查到结果
SELECT B.CLASS								#最外层
	FROM (SELECT A.CLASS,COUNT(A.CLASS) C          #第二层查询，得到具有 CLASS、COUNT(CLASS) 的表 B
		FROM (SELECT DISTINCT *				#第三层查询，去重得到表 A
			FROM COURSES) A
	GROUP BY A.CLASS) B						#分组
	WHERE B.C >= 5;							#条件

#稍微优化，两层查询，主要是因为用了 HAVING 省了一层查询
SELECT A.CLASS					#最外层
	FROM (SELECT DISTINCT *		#第二层查询，去重得到表 a
		FROM COURSES) A
	GROUP BY A.CLASS			        #分组
	HAVING COUNT(A.CLASS) >= 5;	#利用 COUNT() 计算每组个数并筛选

#极致优化，一层查询，利用 GROUP BY 为 CLASS 分组后，直接用 COUNT() 统计每组学生个数，在统计前先用
#DISTINCT 去掉重复学生
SELECT CLASS
	FROM COURSES
	GROUP BY CLASS							#分组
	HAVING COUNT(DISTINCT STUDENT) >= 5;          #利用 COUNT() 统计每门课 STUDENT 的个数，同时利
                                                                                        #用 DISTINCT 去掉重复学生
```