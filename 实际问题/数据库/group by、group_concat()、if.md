# group by、group_concat()、if()

---

1. ?有一??程??表，表?构如下：

| 字段名          | ?型    | ?度 | ?   | 注?                                |
| --------------- | ------- | ---- | ---- | ----------------------------------- |
| id              | varchar | 32   | √    |                                     |
| trainName       | varchar | 255  |      | 培??程名称                        |
| insideTeacher   | varchar | 255  |      | 内部??姓名                        |
| outsideTeacher  | varchar | 255  |      | 外部??姓名                        |
| insideOrOutside | varcher | 2    |      | ???型（0：内部?? 1：外部??） |

2. 表中有如下数据：

![image-20200610105048897](markdown/group by、group_concat()、if.assets/image-20200610105048897.png)

3. ?在需要??如下格式的数据：

   > |trainName|trainTeacher|

4. 思路：

>1. 需要根据trainName?数据?行分?；
>2. 使用group_concat()方法?分?数据?行拼接；
>3. 使用if()判断使用insideTeacher?是outsideTeacher?行拼接。

5. ?施：

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

6. ?果：

![image-20200610105233536](markdown/group by、group_concat()、if.assets/image-20200610105233536.png)

