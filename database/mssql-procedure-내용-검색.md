출처: https://sqlhints.com/2011/10/01/how-to-find-all-the-stored-procedures-having-a-given-text-in-it/

```sql
SELECT name, OBJECT_DEFINITION(object_id)
FROM sys.procedures
WHERE OBJECT_DEFINITION(object_id) LIKE '%키워드%'
```
- `sys.procedures`에 프로시저 정보가 저장되어있다.
- [OBJECT_DEFINITION()](https://learn.microsoft.com/ko-kr/sql/t-sql/functions/object-definition-transact-sql?view=sql-server-ver16) 함수로 프로시저 정의 정보를 읽는다.
