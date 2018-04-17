# GitHub构建Maven

### 1. 在github创建respository, 名称 maven_test

### 2. 克隆到本地

### 3. 选择要添加的project，在gradle.properties中添加    arr.deployPath=/home/..../maven_test 克隆后的本地路径

### 4. lib的gradle中添

```
uploadArchives {
    repositories.mavenDeployer {
        //地址
        def deployPath = file(getProperty('arr.deployPath'))
        repository(url: "file://${deployPath.absolutePath}")
        pom.project {
            pom.groupId = "com.ding"
            pom.artifactId = "maventest"//不同组件对应的pom.artifactId不同
            pom.version = "1.0.0"//release 版本不需要加SNAPSHOT1.0.0,只需要1.0.0即可
        }
    }
}
```

### 5. uploadArchives 操作，在本地maven_test生成依赖文件

### 6. push maven_test origin master


### 7. 本地使用

project的gradle中
```
maven { url 'https://raw.githubusercontent.com/xxx/maven_test/master' }  //xxx为git地址

```

app的gradle中
```
compile 'com.ding:maventest:1.0.0
```