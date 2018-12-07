#### feign开发规范(client层)
- 使用场景：用于工程间的调用
    - 提供方定义好接口，需要将jar包打包上传
    - 使用方需要引用该client jar包，再使用里面的接口
    - jar包打包上传命令：mvn clean source:jar deploy -Dmaven.test.skip=true -U -X

- feign接口定义
    ``` java
    @FeignClient(
      name = Providers.PETREL_ITG_API, // 提供者名称
      fallbackFactory = ItgCodeTypeApiFallbackFactory.class // 回调类名
    )
    ```
    
- 接口命名
  `ItgCodeTypeApi` 以api结尾
  
- 接口方法定义及注意事项
  ``` java
  /**查询字典主表,
   * @param  columnMap:根据字典明细的数据库字段,进行查询,map为空则查全部
   * */
   @RequestMapping(value = "/itg/api/basDictCodeTypeApi/cache/findList.json", method = RequestMethod.POST)
   public List findList(@RequestBody Map<String, Object> columnMap);
   ```
   tips:
    - RequestMapping的路径对应api层controller的路径
    - method定义的http协议与controller里的定义保持一直
    - 使用@RequestBody接收对象参数，@RequestParam接收多个平铺的参数