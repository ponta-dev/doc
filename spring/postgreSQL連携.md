# 構築・接続
```groovy
dependencies {
	// sql
	implementation "org.springframework.boot:spring-boot-starter-jdbc:${version_spring_boot}"
	implementation "org.springframework.boot:spring-boot-starter-data-jpa:${version_spring_boot}"
	runtime "org.postgresql:postgresql:${version_postgresql_jdbc}"
}
```
Spring BootでDependenciesを組み込むとDataSourceの定義がないとエラーになる  
DataSourceはデフォルトとカスタマイズがある  

## デフォルトDataSource

```conf
#schema.sql,data.sqlによる初期化のためnoneにする
spring.jpa.hibernate.ddl-auto=none
#デフォルトがembedded
#PostgreSQLの場合はALWAYS
#カスタムでDataSourceを設定する場合はnever
spring.datasource.initialization-mode=ALWAYS
#利用するドライバー
spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.sql-script-encoding=utf-8
#ここに設定した値をもとにschema-postgresql.sqlが実行される
spring.datasource.platform=postgresql
#DataSourceに対する接続設定
spring.datasource.url=jdbc:postgresql://localhost:5432/buildframe?currentSchema=apiserver
#DataSourceの接続ユーザ
spring.datasource.username=buildframe
spring.datasource.password=buildframe
#初期化用
#schemaやdataは別に指定がなくてもいい
#指定するとフォルダをきれいにできるがplatformがあまり意味がなくなる
#DML,DDL実行のユーザは別に設定できる
spring.datasource.schema=classpath:initialize/schema-postgresql.sql
spring.datasource.schema-username=buildframe
spring.datasource.schema-password=buildframe
spring.datasource.data=classpath:initialize/data-postgresql.sql
spring.datasource.data-username=buildframe
spring.datasource.data-password=buildframe
```
## カスタム
詳しくは[公式](https://docs.spring.io/spring-boot/docs/2.2.2.RELEASE/reference/html/howto.html#howto-configure-a-datasource)参照  
また複数DB接続時の各DBの初期化方法は下記が詳しい
<http://mythosil.hatenablog.com/entry/2019/07/28/181613>
PostgreSQLとMongoDBなどの組み合わせでカスタム設定がマストかどうか不明  
カスタムの場合、任意のプロパティを利用できる
```conf
postgresql.datasource.driver-class-name=org.postgresql.Driver
postgresql.datasource.sql-script-encoding=utf-8
postgresql.datasource.platform=postgresql
postgresql.datasource.jdbcUrl=jdbc:postgresql://localhost:5432/buildframe?currentSchema=apiserver
postgresql.datasource.username=buildframe
postgresql.datasource.password=buildframe
```

# 初期化
## 概要
Spring bootではDDLとDMLを初期化時に実行できる  
機能的にはSpring Data JPAの機能  
* spring.jpa.generate-ddl(boolean)

で機能のON/OFFができる  

## 初期化の仕方
標準のルートパス直下の
* schema.sql
* data.sql

が対象  
さらに下記を処理する
* schema-${platform}.sql
* data-${platform}.sql

${platform}は
* spring.datasource.platform

の値  
これによりDBの切り替えができる  

## その他挙動
* spring.datasource.initialization-mode

Spring BootではDatasourceを自動的に作成する  
Datasourceを毎回作るかどうかの設定に使う  
デフォルトはembedded  
embeddedは組み込みDBの時のみ初期化を実行する  
PostgreSQLを使う場合はalwaysにする必要がある

* spring.jpa.hibernate.ddl-auto
Hibernateの機能としてデータを初期化する方法  
schema.sqlと両方使うことはできないので、schema.sqlを使う場合は無効化する