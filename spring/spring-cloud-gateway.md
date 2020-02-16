# Spring Cloud Gateway
[spring cloud gateway document 2.1系](https://cloud.spring.io/spring-cloud-gateway/2.1.x/single/spring-cloud-gateway.html)  

## Cloud Gatewayの役割
本番のユースケースだとApplication Gatewayとかを使うと思うが、  
開発だとドメインを簡単に一緒にするためにプロキシ用途で使う

## SpringCloudGatewayの組み込み
```groovy
plugins {
    id 'java'
	id 'org.springframework.boot' version "${version_spring_boot}"
}

compileJava {
	options.encoding = 'UTF-8'
}

sourceCompatibility = 11
targetCompatibility = 11

repositories {
    mavenCentral()
}

dependencies {
	implementation "org.springframework.boot:spring-boot-starter-webflux:${version_spring_boot}"
	implementation "org.springframework.boot:spring-boot-actuator:${version_spring_boot}"
	implementation "org.springframework.cloud:spring-cloud-starter-gateway:${version_spring_cloud}"
}
```

## application.ymlの書き方
```yaml
server:
  port: 8080
spring:
  cloud:
    gateway:
      routes:
      - id: ui_route
        uri: http://localhost:8088
        predicates:
        - Path=/buildframe/**
      - id: api_route
        uri: http://localhost:8089
        predicates:
        - Path=/apis/{segment}
        filters:
        - SetPath=/api-server/{segment}
```

## メモ
### UIサーバへのプロキシ
/jsや/cssファイル含めプロキシするのにあたって、  
```
filters:
- SetPath=
```
の書き換えだと上手く出来なかった（やり方まちがってる？）  
いまいまはUIサーバのcontext-pathとpredicatesで指定するパスを一緒にして、  
パス部分は変えずにip:portのリダイレクトだけしたら動いた

### APi呼び出し
SetPathの{segment}は名前は何でもいい  
単一の？API呼び出しであれば上手く動く