# spring-node連携  
## buildの出力先の連携
Gradleにnode.jsの連携プラグインを入れる
> build.gralde
```groovy
//pluginを組み込み
plugins {
	id 'java'
	id 'org.springframework.boot' version "${version_spring_boot}"
	id "com.moowork.node" version "${version_node_plugin}"
}
//node.jsのプロジェクトディレクトリ、nodeモジュールのディレクトリの指定
node {
	workDir = file("${project.projectDir}/${dir_web_ui}")
	nodeModulesDir = file("${project.projectDir}/${dir_web_ui}")
}
//node.jsモジュールをビルドするタスクの定義
//argsはnpm に続くコマンドの指定
task webUiBuild(type: NpmTask) {
	args = ['run', 'build']
}
//classesの依存関係に追加
classes.dependsOn webUiBuild
```

vue  
> vue.config.js  
```javascript
module.exports = {
  outputDir: "../ui-server/src/main/resources/static",
}
```

gradleタスクの実行  
`gradle build`

## ベースURLの連携
spring  
> application.properties  
`server.servlet.context-path=/ui-server/`

vue  
> vue.config.js  
```javascript
module.exports = {
  publicPath: "/ui-server/"
}
```