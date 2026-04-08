### Option A — Using SQL*Plus (recommended)

Open Command Prompt and type:

```bash
sqlplus / as sysdba
```


### Disable it

```sql
EXEC DBMS_XDB.SETHTTPPORT(0);
```


# Step 4: Re-enable later

Whenever you want Oracle to use port 8080 again:

```sql
EXEC DBMS_XDB.SETHTTPPORT(8080);
```




```sql
SELECT * FROM EMPLOYEES;
ALTER TABLE EMPLOYEES
ADD DEPARTMENT_ID NUMBER;
UPDATE EMPLOYEES SET DEPARTMENT_ID = 10 WHERE EMPLOYEE_ID = 1;
UPDATE EMPLOYEES SET DEPARTMENT_ID = 20 WHERE EMPLOYEE_ID = 2;
UPDATE EMPLOYEES SET DEPARTMENT_ID = 10 WHERE EMPLOYEE_ID = 3;
UPDATE EMPLOYEES SET DEPARTMENT_ID = 30 WHERE EMPLOYEE_ID = 4;
UPDATE EMPLOYEES SET DEPARTMENT_ID = 20 WHERE EMPLOYEE_ID = 5;

SELECT EMPLOYEE_ID,
     first_name,department_id,
     salary,
     RANK() over (Partition by department_id ORDER BY salary DESC) rnk,
     DENSE_RANK() OVER(Partition By department_id ORDER BY salary DESC) drnk,
     ROW_NUMBER() OVER (Partition By department_id ORDER BY salary DESC) rn
FROM EMPLOYEES;

------ WRITE A QUERY TO FIND THE HIGHEST SALARY
DESC EMPLOYEES;

--- WRITE A QUERY TO DISPLAY SECOND HIGHEST SALARY
SELECT DISTINCT SALARY FROM (SELECT EMPLOYEE_ID,
     first_name,department_id,
     salary,
     RANK() over (Partition by department_id ORDER BY salary DESC) rnk,
     DENSE_RANK() OVER(Partition By department_id ORDER BY salary DESC) drnk,
     ROW_NUMBER() OVER (Partition By department_id ORDER BY salary DESC) rn
FROM EMPLOYEES) WHERE

```