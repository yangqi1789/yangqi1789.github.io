---
layout:     post
title:      MybatisGenerator的使用
subtitle:   根据数据库表名及其列名生成JAVA对应entity以及DAO
date:       2019-06-12
author:     YQ
header-img: 
catalog: false
sidebar: false
tags:
    - MybatisGenerator
---

## MybatisGenerator(MB)的安装

MB的安装非常简单，只需要在项目中导入maven依赖及plugin即可，我这里用的是1.3.6版本。

只需要在POM文件中添加:

```xml
    <dependencies>
        <dependency>
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-core</artifactId>
            <version>1.3.6</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.mybatis.generator</groupId>
                <artifactId>mybatis-generator-maven-plugin</artifactId>
                <version>1.3.6</version>
            </plugin>
        </plugins>
    </build>
```

在resource中添加generatorConfig.xml配置文件：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
 PUBLIC " -//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
 "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>

    <classPathEntry
            location="D:\Java\apache-maven-3.3.1\repository\mysql\mysql-connector-java\5.1.34\mysql-connector-java-5.1.34.jar"/>
    <context id="mysql" defaultModelType="hierarchical" targetRuntime="MyBatis3Simple">
        <commentGenerator>
            <property name="suppressDate" value="true"/>
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>

　　　　 <!-- mysql数据库连接 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://XXX:3306/crm?allowMultiQueries=true"
                        userId="XXX"
                        password="XXX">
            <!-- 这里面可以设置property属性，每一个property属性都设置到配置的Driver上 -->
        </jdbcConnection>

　　　　　<!-- 生成model实体类文件位置 -->
        <javaModelGenerator targetPackage="com.mybatis.generator.entity"
                            targetProject="src/main/java">
            <property name="enableSubPackages" value="true"/>
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

　　　　　<!-- 生成mapper.xml配置文件位置 -->
        <sqlMapGenerator targetPackage="com.mybatis.generator.mapper"
                         targetProject="src/main/java">
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>

        <!-- 生成mapper接口文件位置 -->
        <javaClientGenerator targetPackage="com.mybatis.generator.mapper"
                             targetProject="src/main/java" type="XMLMAPPER">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>

　　　　 <!-- 需要生成的实体类对应的表名，多个实体类复制多份该配置即可 -->
        <table tableName="cr_table_setting" domainObjectName="TableSetting"
               enableCountByExample="false" enableUpdateByExample="false"
               enableDeleteByExample="false" enableSelectByExample="false"
               selectByExampleQueryId="false">
        </table>

    </context>
</generatorConfiguration>
```

## MB常用配置

### 插件

在`<context>`标签下增加`<plugin>`即可导入插件，MB自带plugin:

```xml
<!-- 实现序列化接口，并生成id-->
<plugin type="org.mybatis.generator.plugins.SerializablePlugin" />
<!-- 增加equals和has-->
<plugin type="org.mybatis.generator.plugins.EqualsHashCodePlugin" />
<!-- 增加toString方法-->
<plugin type="org.mybatis.generator.plugins.ToStringPlugin" />
<!-- 在生成的DAO上增加@Mapper注解-->
<plugin type="org.mybatis.generator.plugins.MapperAnnotationPlugin"/>
<!-- 虚拟主键，可以在数据库没有主键的时候，指定一个虚拟主键，以便生成相应的增删改查方法-->
<plugin type="org.mybatis.generator.plugins.VirtualPrimaryKeyPlugin"/>
<table tableName="foo">
    <property name="virtualKeyColumns" value="ID1, ID2" />
</table>
```

### `<javaModelGenerator>`标签内常用可选属性，javaModelGenerator属性可以`<table>`标签内重写

1. 为true时自动为每一个生成的类创建一个构造方法，构造方法包含了所有的field

    ```xml
    <property name="constructorBased" value="true"/>
    ```

2. 为true时在getter方法中，对String类型字段调用trim()方法

    ```xml
    <property name="trimStrings" value="true"/>
    ```

3. 为true时不生成dao类，如果配置了sqlMapGenerator，那么在mapper XML文件中，只生成resultMap元素和selectAll方法

    ```xml
    <property name="modelOnly" value="true"/>
    ```

4. 为生成的entity指定基类

    ```xml
    <property name="rootClass" value="com.mybatis.generator.entity.BaseEntity"/>
    ```

5. 为生成的dao指定基类

    ```xml
    <property name="rootInterface" value="com.mybatis.generator.dao.BaseDao"/>
    ```

### `<table>`内常用可选属性

1. 为生成的selectAll指定order by语句，也可以增加其他语句，会追加在sql的结尾

    ```xml
    <property name="selectAllOrderByClause" value="age desc,username asc"/>
    ```

2. 为true时根据数据库本身生成属性，而不是驼峰命名

    ```xml
    <property name="useActualColumnNames" value="false"/>
    ```

3. 指定列对应的JAVA属性类型

    ```xml
    <columnOverride column="id" javaType="java.lang.Long" jdbcType="decimal"/>
    ```
