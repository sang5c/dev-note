## 모음
```yml
spring:
  jpa:
    database: h2
    database-platform: org.hibernate.dialect.H2Dialect
    hibernate:
      ddl-auto: create-drop
    properties:
      hibernate:
        dialect: org.hibernate.dialect.H2Dialect
        show_sql: true
        format_sql: true
```

## binding parameter 출력
```yml
logging:
  level:
    org.hibernate.orm.jdbc.bind: trace

```
