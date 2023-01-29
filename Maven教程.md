> 摘要: 原创出处 cnblogs.com/hzg110/p/6936101.html 「啤酒大泡泡」欢迎转载，保留摘要，谢谢！
>
> - [一、为什么使用Maven这样的构建工具【why】](http://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/)
> - [二、maven是什么【what】](http://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/)
> - [三、安装maven](http://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/)
> - [四、第一个maven](http://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/)
> - [五、仓库和坐标](http://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/)
> - [六、依赖](http://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/)
> - [七、生命周期　　](http://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/)
> - [八、Eclipse中使用maven](http://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/)
> - [九、maven工程的依赖高级特性](http://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/)
> - [十、build配置](http://www.iocoder.cn/Fight/Maven-most-complete-tutorial-read-must-understand/)

## 使用Maven的原因

**① 一个项目就是一个工程**

如果项目非常庞大，就不适合使用package来划分模块，最好是每一个模块对应一个工程，利于分工协作。借助于maven就可以将一个项目拆分成多个工程

**② 项目中使用jar包，需要“复制”、“粘贴”项目的lib中**

同样的jar包重复的出现在不同的项目工程中，你需要做不停的复制粘贴的重复工作。借助于maven，可以将jar包保存在“仓库”中，不管在哪个项目只要使用引用即可就行。

**③ jar包需要的时候每次都要自己准备好或到官网下载**

借助于maven我们可以使用统一的规范方式下载jar包，规范

**④ jar包版本不一致的风险**

不同的项目在使用jar包的时候，有可能会导致各个项目的jar包版本不一致，导致未执行错误。借助于maven，所有的jar包都放在“仓库”中，所有的项目都使用仓库的一份jar包。

**⑤ 一个jar包依赖其他的jar包需要自己手动的加入到项目中**

FileUpload组件->IO组件，commons-fileupload-1.3.jar依赖于commons-io-2.0.1.jar

极大的浪费了我们导入包的时间成本，也极大的增加了学习成本。借助于maven，它会自动的将依赖的jar包导入进来。

## Maven是什么

**① maven是一款服务于java平台的自动化构建工具**

make->Ant->Maven->Gradle

名字叫法：我们可以叫妹文也可以叫麦文，但是没有叫妈文的。

**② 构建**

构建定义：把动态的Web工程经过编译得到的编译结果部署到服务器上的整个过程。

编译：java源文件[.java]->编译->Classz字节码文件[.class]

部署：最终在sevlet容器中部署的不是动态web工程，而是编译后的文件

<img src="img\1.png" style="zoom:75%;" />

**③ 构建的各个环节**

- 清理clean：将以前编译得到的旧文件class字节码文件删除
- 编译compile：将java源程序编译成class字节码文件
- 测试test：自动测试，自动调用junit程序
- 报告report：测试程序执行的结果
- 打包package：动态Web工程打War包，java工程打jar包
- 安装install：Maven特定的概念-----将打包得到的文件复制到“仓库”中的指定位置
- 部署deploy：将动态Web工程生成的war包复制到Servlet容器下，使其可以运行

## 安装maven



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