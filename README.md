# FlyGo_WebDemo1
文档地址：https://www.flygo520.com/docs/maven/maven-1am1lulmhmfet

# 创建Maven Web项目

1、右键`New` -> 选择`Project...`

![创建Web项目 步骤1](https://www.flygo520.com/uploads/maven/images/m_29b83fe3f04034389ddc6b78293e9d58_r.png#size=360x0)

2、选择 `Maven Project` -> 选择 `下一步`

![创建Web项目 步骤2](https://www.flygo520.com/uploads/maven/images/m_f05cb2cf736c817a83b0a05f4f888bd2_r.png#size=360x0)

3、勾选`Create a simple project  (skip archetype selection)`-> 选择`下一步`

![创建Web项目 步骤3](https://www.flygo520.com/uploads/maven/images/m_88023f621995d513fd20c9440faf3ab5_r.png#size=360x0)

4、填写相关信息，特别注意，Web项目有且只能选择 `Packaging` 为 `war` 类型。

![创建Web项目 步骤4](https://www.flygo520.com/uploads/maven/images/m_ee36ac89b265812362913841d9b171a4_r.png#size=360x0)

# Maven Web项目的目录结构

>[danger] 刚开始创建的web项目，`webapp`目录 缺少 `WEB-INF` 和 `META-INF`文件目录及 `web.xml`文件。项目工程报错。

报错的工程目录结构

![Web项目的目录结构](https://www.flygo520.com/uploads/maven/images/m_08869399e19114453664264b2b61ded3_r.png#size=360x0)

修复好的工程目录结构

![修复好的工程目录结构](https://www.flygo520.com/uploads/maven/images/m_3d991e766c660e11b34d2a0e1d171a70_r.png#size=360x0)

```bash
其中 `web.xml`的文件内容为：

<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.0" xmlns="http://java.sun.com/xml/ns/javaee"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://java.sun.com/xml/ns/javaee                       
	http://java.sun.com/xml/ns/javaee/web-app_3_0.xsd">

</web-app>
```
# 添加web工程所依赖的Jar包和插件

1、添加web工程所有依赖的Jar包
>[info]其中 `javax.servlet-api` 和 `jsp-api` 申明为 `scope` 为 `provided`。原因为引入的插件包含`javax.servlet-api` 和 `jsp-api` 相关Jar包。

```bash
<dependencies>
	<dependency>
		<groupId>javax.servlet</groupId>
		<artifactId>javax.servlet-api</artifactId>
		<version>3.0.1</version>
		<scope>provided</scope>
	</dependency>
	<dependency>
		<groupId>javax.servlet.jsp</groupId>
		<artifactId>jsp-api</artifactId>
		<version>2.2</version>
		<scope>provided</scope>
	</dependency>
	<dependency>
		<groupId>jstl</groupId>
		<artifactId>jstl</artifactId>
		<version>1.2</version>
	</dependency>
</dependencies>
```

2、添加web工程所有依赖的插件

```bash
<build>
	<plugins>
		<plugin>
			<groupId>org.apache.tomcat.maven</groupId>
			<artifactId>tomcat7-maven-plugin</artifactId>
			<version>2.2</version>
			<configuration>
				<!-- 控制tomcat端口号 -->
				<port>8090</port>
				<!-- 项目发布到tomcat后的名称 -->
				<!-- / 相当于把项目发布名称为ROOT -->
				<!-- /webDemo -->
				<path>/web_demo</path>
			</configuration>
		</plugin>
	</plugins>
</build>
```
# 编写简单的测试代码

创建类`WebDemoTest` 继承 `javax.servlet.http.HttpServlet`。重写 `service` 方法。

![测试代码](https://www.flygo520.com/uploads/maven/images/m_6831881df224c1d547f753225f82754d_r.png#size=360x0)

# 配置启动项目

1、右键选择项目 -> 选择`Run As` -> 选择 `Maven Build...`

![启动项目步骤1](https://www.flygo520.com/uploads/maven/images/m_83f4c725ca0ea751ca152dcc3106259a_r.png#size=360x0)

2、选择 `Main` 选项卡 -> 配置 `Goals` 为 `clean tomcat7:run` -> 点击`Apply` -> 点击`run`即可运行项目。

![启动项目步骤2](https://www.flygo520.com/uploads/maven/images/m_df6fb2a8b1a33649cdb12e74e3c022f4_r.png#size=360x0)

3、输出控制台没有报错，启动成功

![启动图片](https://www.flygo520.com/uploads/maven/images/m_66b71d15f69d413088e38ff7e80a35a2_r.png#size=360x0)

# 项目启动报错相关汇总

1、项目已经启动，再启动一个，控制台报端口被占用

![端口被占用错误](https://www.flygo520.com/uploads/maven/images/m_80d3e141a037dd1fc2b60bc686d3808f_r.png#size=360x0)

2、Tomcat中已经有 `javax.servlet-api` 包，`pom.xml` 没有被声明为 `provided`, 引入的包会和 `Tomcat`的包冲突。

![包冲突错](https://www.flygo520.com/uploads/maven/images/m_a8b1c80624589535132dee20c516106a_r.png#size=360x0)

3、同理，Tomcat中已经有 `jsp-api` 包，`pom.xml` 没有被声明为 `provided`, 引入的包会和 `Tomcat`的包冲突。

![包冲突](https://www.flygo520.com/uploads/maven/images/m_b15695fc9e5614d4cd0e3676c6617601_r.png#size=360x0)

>[info] 特别注意，在 `pom.xml` 中被声明为 `provided` 的依赖包。打包时会排除相关依赖包。

![排除的依赖包](https://www.flygo520.com/uploads/maven/images/m_86a02c1340a5d955eeb354a31e482a54_r.png#size=360x0)

# 演示Demo源码地址

