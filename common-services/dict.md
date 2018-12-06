#### 数据字典
- 前后端交互
    - http接口说明
    
    `/itg/api/sysDictDtl/findSysDict.json` 根据字典编码取对应名称
    `/itg/api/sysDict//find_SysDict.json` 获取字典明细

- 工程间调用
    - feign接口说明
        ``` java
        /**查询字典主表,
         * @param  columnMap:根据字典明细的数据库字段,进行查询,map为空则查全部
         * */
         @RequestMapping(value = "/itg/api/basDictCodeTypeApi/cache/findList.json", method = RequestMethod.POST)
         public List findList(@RequestBody Map<String, Object> columnMap);
        
        /**查询字典明细表,
         * @param  columnMap:根据字典主表字段dict_code,以及明细表item_code,item_name进行查询
         * */
         @RequestMapping(value = "/itg/api/basDictCodeDtlTypeApi/findSysDict.json", method = RequestMethod.POST)
         public List<SysDictDtlDTO> findSysDict(@RequestBody Map<String, Object> columnMap);
        ```
    - 使用方法
        - 注入feign接口
 
            ``` java
              @Autowired
              private ItgCodeTypeApi itgCodeTypeApi;
            ```
        - 调用接口方法，查询字典数据