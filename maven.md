## tomcat

版本

```xml
<!--<tomcat>9.0.30</tomcat>-->
<!--<tomcat>8.5.50</tomcat>-->
<tomcat>7.0.99</tomcat>
```

依赖

```xml
  <dependency>
    <groupId>org.apache.tomcat</groupId>
    <artifactId>tomcat-catalina</artifactId>
    <version>${tomcat}</version>
    <scope>provided</scope>
  </dependency>

  <dependency>
    <groupId>org.apache.tomcat</groupId>
    <artifactId>tomcat-coyote</artifactId>
    <version>${tomcat}</version>
    <scope>provided</scope>
  </dependency>
```

## Junit

```xml
<dependency>
  <groupId>junit</groupId>
  <artifactId>junit</artifactId>
  <version>4.11</version>
  <scope>test</scope>
</dependency>
```

