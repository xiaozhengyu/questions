# group by、group_concat()、if()

---

1. 现有一培训课程表，表结构如下：

| 字段名          | ?型     | ?度  | 主?  | ?注                              |
| --------------- | ------- | ---- | ---- | -------------------------------- |
| id              | varchar | 32   | √    |                                  |
| trainName       | varchar | 255  |      | 培训课程名称                     |
| insideTeacher   | varchar | 255  |      | 内部讲师姓名                     |
| outsideTeacher  | varchar | 255  |      | 外部讲师姓名                     |
| insideOrOutside | varcher | 2    |      | ??型（0：内部讲师  1：外部讲师） |

2. 表中有如下数据：

![image-20200610105048897](markdown/group by、group_concat()、if2.assets/image-20200610105048897.png)

3. 现在需要获取如下格式的数据：

    > |trainName|trainTeacher|

4. 思路：

>1. 需要根据trainName对数据进行分组；
>2. 使用group_concat()方法将分组数据进行拼接；
>3. 使用if()判断使用insideTeacher还是outsideTeacher进行拼接。

5. 实施：

```sql
SELECT
	t.trainName,
	GROUP_CONCAT(
	IF
	( t.insideOrOutside = 0, t.insideTeacher, t.outsideTeacher )) as trainTeacher
FROM
	`trainteacher` t 
GROUP BY
	t.trainName
```

6. 结果：

![image-20200610105233536](markdown/group by、group_concat()、if2.assets/image-20200610105233536.png)

