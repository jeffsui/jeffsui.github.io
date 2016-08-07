title: "maven3-jdk1.7-problem-fixed"
date: 2016-02-13 16:24:25
category: ci
tags: [jdk,maven,持续集成]

---

#maven3下jdk1.7编译错误解决

#环境
```

Apache Maven 3.3.3 (7994120775791599e205a5524ec3e0dfe41d4a06; 2015-04-22T19:57:3
7+08:00)
Maven home: d:\tools\apache-maven-3.3.3
Java version: 1.7.0_45, vendor: Oracle Corporation
Java home: c:\Program Files\Java\jdk1.7.0_45\jre
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 7", version: "6.1", arch: "amd64", family: "windows"

```
#使用`maven`命令行创建java项目
```
mvn archetype:generate -DgroupId=org.linfeng -DartifactId=mavendemo -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
```

* 创建成功

```
$ cd mavendemo && ls
pom.xml  src
```

* pom.xml

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>org.linfeng</groupId>
  <artifactId>mavendemo</artifactId>
  <packaging>jar</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>mavendemo</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
</project>
```
* 执行maven命令

`mvn test`

* 错误信息

```
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.1:compile (default-compile) on project mavendemo: Compilation failure
[ERROR] No compiler is provided in this environment. Perhaps you are running on a JRE rather than a JDK?

```

#解决方案

* 修改`settings.xml`,添加jdk1.7相关内容

```
<profile>
    <id>jdk-1.7</id>
    <activation>
        <activeByDefault>true</activeByDefault>
        <jdk>1.7</jdk>
    </activation>
    <properties>
        <maven.compiler.source>1.7</maven.compiler.source>
        <maven.compiler.target>1.7</maven.compiler.target>
        <maven.compiler.compilerVersion>1.7</maven.compiler.compilerVersion>
    </properties>
</profile>

```

*缺点*:修改所有项目的jre环境

* 修改当前项目的`pom.xml`

```
<build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.1</version>
        <configuration>
          <source>1.7</source>
          <target>1.7</target>
        </configuration>
      </plugin>
    </plugins>
</build>
```
重新执行`mvn build` 成功！

#问题分析
Maven官方文档有如下描述：
> 编译器插件用来编译项目的源文件.从3.0版本开始, 用来编译Java源文件的默认编译器是javax.tools.JavaCompiler (如果你是用的是java 1.6) . 如果你想强制性的让插件使用javac,你必须配置插件选项 forceJavacCompilerUse.
同时需要注意的是目前source选项和target 选项的默认设置都是1.5, 与运行Maven时的JDK版本无关.如果你想要改变这些默认设置, 可以参考 Setting the -source and -target of the Java Compiler中的描述来设置 source 和target 选项.

#参考资料

* http://stackoverflow.com/questions/15220392/maven-package-compilation-error
* http://www.cnblogs.com/leo100w/p/4017647.html
