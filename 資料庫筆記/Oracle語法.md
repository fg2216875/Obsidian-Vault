Oracle語法

=======================================================================

## 取得select查詢中的第一筆資料
```sql
select fname from MyTbl where rownum = 1

--或是

select max(fname) over (rank() order by some_factor) from T_TABLE
```
=======================================================================

## 將varchar轉成date
```sql
update T_Pwd_change_log

set C_Date = to_date('2021-04-10','yyyy/MM/dd')  --to_date('2021-04-10','yyyy/MM/dd')
```
=======================================================================

## 將group by後的欄位值null改顯示為0
```sql
NVL(b.num, 0) -- 如果num欄位為null的話，則顯示0

SELECT a.id, NVL(b.num, 0) AS num FROM a

LEFT JOIN (SELECT id, COUNT(*) AS num FROM b GROUP BY id) b on b.id=a.id
```
=======================================================================

## coalesce 函式可為多個參數，回傳第一個不是NULL值的參數
```sql
select * from T_RECRUIT a

WHERE coalesce(REAL_RECRUIT_DATE,RECRUIT_DATE) IS NOT NULL AND coalesce(REAL_QUIT_DATE,QUIT_DATE) IS NULL
```
如果REAL_RECRUIT_DATE不為null的話，則會回傳此參數。如果REAL_RECRUIT_DATE為null的話，則會查看下個參數，

以此類推

=======================================================================

## EXTRACT 函式能夠抽取日期時間(datetime)格式資料中的指定資訊，例如年，月，日等。
```sql
SELECT

    SYSDATE "System datetime",

    EXTRACT(YEAR FROM SYSDATE) "Year",   --取得現在時間的"年份"

    EXTRACT(MONTH FROM SYSDATE) "Month", --取得現在時間的"月份"

    EXTRACT(DAY FROM SYSDATE) "Day",     --取得現在時間的"日"

FROM DUAL;
```
=======================================================================

## 另外的寫法 T_CAREERAPPLY.CA_ID，假設Table命名為'a'的話，則可寫成 a.CA_ID
```sql
select * from T_CAREERAPPLY

where TEACHERS = (

select LISTAGG(t.CAT_TT_NO,',') WITHIN GROUP (ORDER BY t.CAT_NO)

from T_CAREERAPPLY_TEACHER t

where t.CAT_CA_ID = T_CAREERAPPLY.CA_ID and t.STATUS = 1

  )

||

select * from T_CAREERAPPLY a

where TEACHERS = (

select LISTAGG(t.CAT_TT_NO,',') WITHIN GROUP (ORDER BY t.CAT_NO)

from T_CAREERAPPLY_TEACHER t

where t.CAT_CA_ID = a.CA_ID and t.STATUS = 1

  )
```
=====================================================================

## 將日期字串轉換成Date型態

由於Oracle必須使用函式將字串轉換成Date，才可以存入資料型態為"Date"的欄位
```sql
C_Date = to_date('2021-04-10','yyyy/MM/dd')
```
====================================================

## Oracle EXISTS條件的使用方法

## EXISTS(subquery)

目前有資料表DEPARTMENT及EMPLOYEE，為一對多的關聯，資料如下：

DEPARTMENT

  DEPARTMENT_ID   DEPARTMENT_NAME   
 ------------------------------  
  1                                 總經理室 
  2                                 財務部               
  3                                 業務部               
  4                                 產品部               
  5                                 行銷部             

EMPLOYEE

  EMPLOYEE_ID   EMPLOYEE_NAME   DEPARTMENT_ID   
 -------------------------------------------  
  0001                     Frank Wang                    1               
  0002                     Amy Liu                           1               
  0003                     John Chen                       2               
  0004                     Peter Hung                     2               
  0005                     Matt Lin                          3               
  0006                     Maggie Huang                3               
  0007                     Bill Kin                             4               
  0008                     May Wang                      4               
  0009                     Ryan Hen                        4               
  0010                     Jane Li                             4             

使用EXISTS來查詢出在EMPLOYEE中有存在的DEPARTMENT資料（EMPLOYEE資料表中有DEPARTMENT_ID）。
```sql
SELECTd.* FROM DEPARTMENT d  
WHERE EXISTS(

SELECTNULLFROMEMPLOYEE e  
WHERE e.DEPARTMENT_ID =d.DEPARTMENT_ID );
```
在EMPLOYEE中並沒有DEPARTMENT_ID = 5(行銷部)的資料，因此查詢結果如下：

  DEPARTMENT_ID   DEPARTMENT_NAME   
 --------------------------------  
  1                                    總經理室              
  2                                    財務部               
  3                                    業務部               
  4                                    產品部             

相反地，如果要查出在EMPLOYEE不存在的DEPARTMENT資料（EMPLOYEE資料表中沒有DEPARTMENT_ID），則使用NOT EXISTS
```sql
SELECT d.* FROM DEPARTMENT d  
WHERE NOT EXISTS(

SELECTNULLFROMEMPLOYEE e  
WHERE e.DEPARTMENT_ID = d.DEPARTMENT_ID );
```
查詢結果如下：

  DEPARTMENT_ID   DEPARTMENT_NAME   
 -----------------------------  
  5                                   行銷部             

來自 <[https://matthung0807.blogspot.com/2018/12/oracle-exists-not-exists.html](https://matthung0807.blogspot.com/2018/12/oracle-exists-not-exists.html)>