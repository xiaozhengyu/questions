# Oracle数据库函数：TO_CHAR(datetime)

---

## Syntax

*to_char_date*::=

![Description of to_char_date.gif follows](markdown/TO_CHAR.assets/to_char_date.gif)



## Purpose

`TO_CHAR` (datetime) converts **<u>a datetime or interval value</u>** of `DATE`, `TIMESTAMP`, `TIMESTAMP` `WITH` `TIME` `ZONE`, or `TIMESTAMP` `WITH` `LOCAL` `TIME``ZONE` datatype to a value of `VARCHAR2` datatype in the format specified by the date format `fmt`. If you omit `fmt`, then `date` is converted to a `VARCHAR2` value as follows:

- `DATE` values are converted to values in the default date format.
- `TIMESTAMP` and `TIMESTAMP` `WITH` `LOCAL` `TIME` `ZONE` values are converted to values in the default timestamp format.
- `TIMESTAMP` `WITH` `TIME` `ZONE` values are converted to values in the default timestamp with time zone format.

Please refer to ["Format Models"](https://docs.oracle.com/cd/B19306_01/server.102/b14200/sql_elements004.htm#i34510) for information on datetime formats.

The `'nlsparam'` argument specifies the language in which month and day names and abbreviations are returned. This argument can have this form:

```
'NLS_DATE_LANGUAGE = language' 
```

If you omit `'nlsparam'`, then this function uses the default date language for your session.

## Examples

The following example uses this table:

```
CREATE TABLE date_tab (
   ts_col      TIMESTAMP,
   tsltz_col   TIMESTAMP WITH LOCAL TIME ZONE,
   tstz_col    TIMESTAMP WITH TIME ZONE);
```

The example shows the results of applying `TO_CHAR` to different `TIMESTAMP` datatypes. The result for a `TIMESTAMP` `WITH` `LOCAL` `TIME` `ZONE`column is sensitive to session time zone, whereas the results for the `TIMESTAMP` and `TIMESTAMP` `WITH` `TIME` `ZONE` columns are not sensitive to session time zone:

```
ALTER SESSION SET TIME_ZONE = '-8:00';
INSERT INTO date_tab VALUES (  
   TIMESTAMP'1999-12-01 10:00:00',
   TIMESTAMP'1999-12-01 10:00:00',
   TIMESTAMP'1999-12-01 10:00:00');
INSERT INTO date_tab VALUES (
   TIMESTAMP'1999-12-02 10:00:00 -8:00', 
   TIMESTAMP'1999-12-02 10:00:00 -8:00',
   TIMESTAMP'1999-12-02 10:00:00 -8:00');

SELECT TO_CHAR(ts_col, 'DD-MON-YYYY HH24:MI:SSxFF'),
   TO_CHAR(tstz_col, 'DD-MON-YYYY HH24:MI:SSxFF TZH:TZM')
   FROM date_tab;

TO_CHAR(TS_COL,'DD-MON-YYYYHH2 TO_CHAR(TSTZ_COL,'DD-MON-YYYYHH24:MI:
------------------------------ -------------------------------------
01-DEC-1999 10:00:00           01-DEC-1999 10:00:00.000000 -08:00
02-DEC-1999 10:00:00           02-DEC-1999 10:00:00.000000 -08:00

SELECT SESSIONTIMEZONE, 
   TO_CHAR(tsltz_col, 'DD-MON-YYYY HH24:MI:SSxFF')
   FROM date_tab;

SESSIONTIMEZONE  TO_CHAR(TSLTZ_COL,'DD-MON-YYYY
---------------  ------------------------------
-08:00           01-DEC-1999 10:00:00.000000
-08:00           02-DEC-1999 10:00:00.000000

ALTER SESSION SET TIME_ZONE = '-5:00';
SELECT TO_CHAR(ts_col, 'DD-MON-YYYY HH24:MI:SSxFF'),
   TO_CHAR(tstz_col, 'DD-MON-YYYY HH24:MI:SSxFF TZH:TZM')
   FROM date_tab;

TO_CHAR(TS_COL,'DD-MON-YYYYHH2 TO_CHAR(TSTZ_COL,'DD-MON-YYYYHH24:MI:
------------------------------ -------------------------------------
01-DEC-1999 10:00:00.000000    01-DEC-1999 10:00:00.000000 -08:00
02-DEC-1999 10:00:00.000000    02-DEC-1999 10:00:00.000000 -08:00

SELECT SESSIONTIMEZONE,
   TO_CHAR(tsltz_col, 'DD-MON-YYYY HH24:MI:SSxFF') 
   FROM date_tab;

SESSIONTIMEZONE           TO_CHAR(TSLTZ_COL,'DD-MON-YYYY
------------------------- ------------------------------
-05:00                    01-DEC-1999 13:00:00.000000
-05:00                    02-DEC-1999 13:00:00.000000
```