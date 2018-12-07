#### entity&表结构
- 注解
    ``` java
    @TableName("sys_dict_dtl") // 映射数据库表名
    @Getter // lombok插件注解 生成get方法
    @Setter // lombok插件注解 生成set方法
    @ToString // lombok插件注解 生成toString方法
    ```
    
- 继承关系
    
    单表、多表 继承AbstractEntity即可
    单据继承表继承BaseEditEntity(增加了单据相关字段)
    
    
- 表名命名规则参考阳磊整理的数据库规范文档

- 常见问题
    
    - question1: 数据库id字段写数据时未生成全局唯一ID
        - 实体类里 idType=ID_WORKER
        
            ```
            @TableId(value = "id", type = IdType.ID_WORKER)
            private Long id;
            ```
        - 实体里的getId不能返回null
        
            ``` java
            @Override
            public Long getId() {
                // TODO Auto-generated method stub
                return this.id;
            }
            ```
        - 检查mybatis配置文件

            ``` xml
            #主键类型  0:"数据库ID自增", 1:"用户输入ID",2:"全局唯一ID (数字类型唯一ID)", 3:"全局唯一ID UUID";
            id-type: 0
            ```
        - 检查表结构的id字段是否误设置为自增
        
            `id bigint not null comment 'ID'`