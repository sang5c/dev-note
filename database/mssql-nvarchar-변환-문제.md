MSSQL JDBC 드라이버에서 자바의 String 타입을 데이터베이스로 전달할 때 자동으로 NVARCHAR로 매핑한다.  

SQL SERVER의 NVARCHAR 타입 우선순위가 VARCHAR보다 높다.  
인덱스가 잡혀있는 컬럼이 VARCHAR 타입이고 NVARCHAR로 쿼리를 실행했다면 우선순위에 의해 VARCHAR -> NVARCHAR 변환이 이루어진다. 그럼 인덱스를 못탄다.
해결하려면 jdbc url 전달시 sendStringParametersAsUnicode=false 를 추가한다.

```
jdbc-url: jdbc:sqlserver://localhost:1433;encrypt=false;databaseName=master;sendStringParametersAsUnicode=false
```

전달한 타입 확인은 profiler를 사용하여 볼 수 있다.
![image](https://user-images.githubusercontent.com/28697758/226791685-c6f0bed7-b7a6-47a1-ae9f-c66cec85d043.png)


```
// 설정 추가 전
exec sp_executesql N'select xxx from xxx where xxx=@P0',N'@P0 nvarchar(4000)',N'문자열'
// 설정 추가 후
exec sp_executesql N'select xxx from xxx where xxx=@P0',N'@P0 varchar(8000)','문자열'
```

참고: https://techblog.woowahan.com/2605/
