#### Redis
1. 使用场景：
    - 基础数据缓存
    - 数据权限缓存
    - 常用热点数据缓存

2. core层引入redis依赖

    ``` xml
    <!-- petrel-redis 基础操作工具包 -->
    <dependency>
        <groupId>com.belle</groupId>
        <artifactId>petrel-redis-core</artifactId>
    </dependency>
    ```
3. redis工具类

    >PetrelJdkRedisService
    
    *序列化java对象，且被序列化的对象必须实现Serializable接口*
    
    >PetrelStringRedisService
    
    *简单的字符串序列化。一般如果key-value都是string的话，使用该工具类*
    
4. 使用方法举例
    - 引入redis服务
    
        ``` java
        @Autowired
	    private PetrelJdkRedisService petrelJdkRedisService;
	    ```
	- 取对象demo
	    
        ``` java
        String cacheKey = "petrel:itg:sys.dict.dtl:find.list";
		String hKey = columnMap == null ? "all" : columnMap.toString(); // columnMap为Map结构的查询条件
		List<SysDictDtl> list = null;
		if (petrelJdkRedisService != null) {
			list = (List<SysDictDtl>)petrelJdkRedisService.hashOpsGet(cacheKey, hKey);
			if (CommonUtil.isNotNull(list)) {
				return list;
			}
		}
		```
	- 存对象demo
	    
        ``` java
        String cacheKey = "petrel:itg:sys.dict.dtl:find.list";
        String hKey = columnMap == null ? "all" : columnMap.toString();
        List<SysDictDtl> resultList = this.baseService.selectByMap(columnMap); // columnMap为Map结构的查询条件
        if (petrelJdkRedisService != null && CommonUtil.isNotNull(resultList)) {
			petrelJdkRedisService.hashOpsPut(cacheKey, hKey, resultList,30L,DAYS); // 缓存30天
		}
        ```
5. 使用规范
    - key名设计
        - 遵循可读性和可管理性
            key=项目工程名称+微服务名称＋业务名＋业务数据＋XXX，用冒号分隔，多个连接单词用英文"."连接，如：petrel:itg:sys.dict.dtl:find.list
        - key名要简洁，且不要包含特殊字符
            控制key的长度，当key较多时，内存占用也不容忽视
        - 控制key的生命周期，key的过期时间根据具体业务确定，做到精确设置，所有key必须设置expire，必须设置，不然后果很严重哦~
    - value设计
        - 控制值大小，为避免大key的产生，value值存储尽量精简，减小内存开支
        - 选择合适的数据类型
    - 常规缓存方案指引
    
        ![](/assets/常规缓存方案指引.png)