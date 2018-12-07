#### 引入代码生成器
* src/test/java/codegenerator/BaseTypeEnum.java
* src/test/java/codegenerator/CodeGenerator.java
* src/test/resources/templates

#### 代码说明
* BaseTypeEnum: 类型选择枚举类，目前可继承基类的三种类型单表、多表以及单据
* CodeGenerator: 代码生成器入口
* 生成器模板

#### 使用规则
* 修改具体工程对应的packageName
```
 String packageName = "com.belle.bms.chargingsys.core";
```

* 填写对应需要生成的表名，多个表用逗号隔开
```
generateByTables(packageName, 0, "bm_carrier","xx","xx");
```

* 修改数据库链接及用户名、密码
```
String dbUrl = "jdbc:mysql://10.0.30.39:3306/db_bms_cs?useUnicode=true&characterEncoding=utf-8&useSSL=false";
DataSourceConfig dataSourceConfig = new DataSourceConfig();
dataSourceConfig.setDbType(DbType.MYSQL)
                .setUrl(dbUrl)
                .setUsername("user_bms_cs" )
                .setPassword("scm_bms_cs" )
                .setDriverName("com.mysql.jdbc.Driver" );
```

* 修改生成代码的用户名
```
config.setActiveRecord(false)
                .setIdType(tableIdType)
                .setAuthor("zhang.mj" )
```

* 修改实体继承对象
```
.setSuperEntityClass("com.belle.petrel.common.base.entity.AbstractEntity" );//修改替换成你需要的类名AbstractEntity/BaseEditEntity
```

* 设置代码生成的文件存放路径
```
config.setActiveRecord(false)
                .setIdType(tableIdType)
                .setAuthor("zhang.mj" )
                .setBaseResultMap(true)
                .setBaseColumnList(false)
                .setOutputDir("/Users/liqi/code/belle/codegen" )
```

* 去对应目录将生成的文件拷贝到项目对应的目录中



