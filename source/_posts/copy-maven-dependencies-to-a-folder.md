title: copy maven dependencies to a folder
date: 2016-09-30 13:02:27
category: java
tags: [java,maven]
---
# copy maven dependencies to a folder

## background

一个简单的需求,当你的同事需要调试代码的时候,他并不想建立maven环境,这时候依赖的jar包 该如何导出呢?

## no code say nothing

这时候你需要的是[maven-dependency-plugin](http://maven.apache.org/plugins/maven-dependency-plugin/)。

### 添加依赖配置

```
<plugins>
            <plugin>
                <groupId>org.eclipse.m2e</groupId>
                <artifactId>lifecycle-mapping</artifactId>
                <version>1.0.0</version>
                <configuration>
                    <lifecycleMappingMetadata>
                        <pluginExecutions>
                            <!-- copy-dependency plugin -->
                            <pluginExecution>
                                <pluginExecutionFilter>
                                    <groupId>org.apache.maven.plugins</groupId>
                                    <artifactId>maven-dependency-plugin</artifactId>
                                    <versionRange>[1.0.0,)</versionRange>
                                    <goals>
                                        <goal>copy-dependencies</goal>
                                    </goals>
                                    <configuration>
                                        <outputDirectory>${project.build.directory}/alternateLocation</outputDirectory>
                                        <overWriteReleases>false</overWriteReleases>
                                        <overWriteSnapshots>false</overWriteSnapshots>
                                        <overWriteIfNewer>true</overWriteIfNewer>
                                    </configuration>
                                </pluginExecutionFilter>
                                <action>
                                    <ignore />
                                </action>
                            </pluginExecution>
                        </pluginExecutions>
                    </lifecycleMappingMetadata>
                </configuration>
            </plugin>
    </plugins>
```

此处的`<outputDirectory>` 指定了你导出jar包的路径.

### 执行命令 `mvn dependency:copy-dependencies`

### 查看项目多了一个`/alternateLocation`目录,并且依赖的jar包都下载到这个目录下了。
