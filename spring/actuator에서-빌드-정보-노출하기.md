## actuator import

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
}
```

## gradle 빌드시 빌드 정보 생성
```groovy
springBoot {
    buildInfo()
}
```


## /info path exposure
```yml
management:
  endpoints:
    web:
      exposure:
        include: health, info
```
`GET /actuator/info` 로 접근 가능하다. (default base path는 /actuator이다)

## 기타 참고사항
해당 정보는 gradle build시에 `build.gradle`에 명시된 빌드 정보를 읽어 
`build/resources/main/META-INF/build-info.properties` 경로에 파일을 생성한 다음, actuator를 통해 노출하는 방식으로 진행된다.  
때문에 Intellij IDEA 설정에서 `Build and run using` 항목을 `Gradle`로 사용해야 한다.

![image](https://user-images.githubusercontent.com/28697758/218649728-7d1bba73-d3f0-408f-9eda-26d96f4cc927.png)

코드 내에서 정보를 사용하고 싶다면 BuildProperties를 주입받아 사용할 수 있다.
```java
// ProjectInfoAutoConfiguration.class
@ConditionalOnResource(resources = "${spring.info.build.location:classpath:META-INF/build-info.properties}")
@ConditionalOnMissingBean
@Bean
public BuildProperties buildProperties() throws Exception {
  return new BuildProperties(
		loadFrom(this.properties.getBuild().getLocation(), "build", this.properties.getBuild().getEncoding()));
}
```


## 링크
- [build info 생성하기](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#howto.build.generate-info)
- [Actuator Endpoint 노출하기](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#actuator.endpoints.exposing)
