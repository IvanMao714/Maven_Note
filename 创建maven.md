## 创建maven

**① 创建约定的目录结构**（maven工程必须按照约定的目录结构创建）

> 根目录：工程名 |---src：源码 |---|---main:存放主程序 |---|---|---java：java源码文件 |---|---|---resource：存放框架的配置文件 |---|---test：存放测试程序 |---pop.xml：maven的核心配置文件

我们按照上面的文件夹目录结构手动创建一下，不用任何IDE环境（手动的其实最有助于我们理解maven）

![](img\2.png)

**文件内容如下**

在src/main/java/com/hzg/maven目录下新建文件Hello.java，内容如下

```java
package com.hzg.maven;
public class Hello {
　　public String sayHello(String name){
　　　　return "Hello "+name+"!";
　　}
}
```

POM文件内容：

```xml
<?xml version="1.0" ?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.hzg.maven</groupId>
    <artifactId>Hello</artifactId>
    <version>0.0.1-SNAPSHOT</version>

    <name>Hello</name>

    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

**② 常用maven命令**

- mvn clean：清理
- mvn compile：编译主程序
- mvn test-compile：编译测试程序
- mvn test：执行测试
- mvn package：打包
- mvn install：安装

**执行maven命令必须进入到pom.xml的目录中进行执行**

![](img\3.png)

进入到项目的pom.xml目录之后，就可以执行啦。

**1、运行 mvn compile**

![](img\4.png)

OK，运行完毕，你在pom.xml配置的依赖的包已经导入到仓库了，问题来了，**仓库默认的位置在哪？**

**仓库的默认位置：**c:\Usrs[登录当前系统的用户名].m2\repository

刚才执行完compile之后，之前的文件夹发生了变化

![](img\5.png)

我们发现Hello项目里里多了一个target文件夹。文件夹的内容为：

![](img\6.png)

发现target里主要存放的就是编译后的字节码文件

**2、运行mvn test-compile**，target文件夹下面除了classes之外多了test-classes文件夹

**3、运行mvn package**，target文件夹下面又多了一个打好的jar包

![](img\7.png)

**4、运行mvn clean**，发现整个target文件夹都没了。又回到了编译之前我们手动创建的文件夹

![](img\8.png)