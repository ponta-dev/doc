# Spring Boot のデータアクセス
Spring Dataのコアモジュールは
* Spring Data Common
* Spring Data JPA
* Spring Data JDBC

等があるが、Spring Bootとは構築がかなり異なるので、Spring Boot用の設定を書く

## DataSource設定
### 基本
Spring BootでJPAなどを使おうとすると、DataSourceの設定が必要  



### カスタムDataSource
#### 独自のデータソース作成
```java
@Bean
@ConfigurationProperties(prefix="app.datasource")
public DataSource dataSource() {
    return new FancyDataSource();
}
```
```java
app.datasource.url=jdbc:h2:mem:mydb
app.datasource.username=sa
app.datasource.pool-size=30
```
#### 標準のデータソース作成
```java
@Bean
@ConfigurationProperties("app.datasource")
public DataSource dataSource() {
    return DataSourceBuilder.create().build();
}
```
```java
app.datasource.url=jdbc:mysql://localhost/test
app.datasource.username=dbuser
app.datasource.password=dbpass
app.datasource.pool-size=30
```