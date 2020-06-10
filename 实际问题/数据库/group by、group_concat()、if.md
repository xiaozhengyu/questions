# group by��group_concat()��if()

---

1. ?ͭ��??��??ɽ��ɽ?�èǡ����

| ����̾          | ?��    | ?�� | ?   | ��?                                |
| --------------- | ------- | ---- | ---- | ----------------------------------- |
| id              | varchar | 32   | ��    |                                     |
| trainName       | varchar | 255  |      | ��??��̾��                        |
| insideTeacher   | varchar | 255  |      | ����??��̾                        |
| outsideTeacher  | varchar | 255  |      | ����??��̾                        |
| insideOrOutside | varcher | 2    |      | ???����0������?? 1������??�� |

2. ɽ��ͭǡ��������

![image-20200610105048897](markdown/group by��group_concat()��if.assets/image-20200610105048897.png)

3. ?�߼���??ǡ���ʼ�Ū������

   > |trainName|trainTeacher|

4. ��ϩ��

>1. ���׺���trainName?����?��ʬ?��
>2. ����group_concat()��ˡ?ʬ?����?�ԏ���ܡ�
>3. ����if()Ƚ�ǻ���insideTeacher?��outsideTeacher?�ԏ���ܡ�

5. ?�ܡ�

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

6. ?�̡�

![image-20200610105233536](markdown/group by��group_concat()��if.assets/image-20200610105233536.png)

