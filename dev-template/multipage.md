#### 多表开发
1. 多表维护模块前端界面模型，实现多表的增删改查功能
  
    ![multypage](/assets/multypage.png)
  
2. 多表基类继承
    - 继承BaseMultiPageController
    - 通用的业务开发自定义的controller
    - ...

3. 前端请求参数封装格式
    - 新增
    
        - 保存主表
        
            `https://st-petrel.belle.net.cn/petrel/petrel-itg-api/itg/api/sysDict/multi_add.json`
          
            ``` json
            
            {
                "dictCode":"test",
                "dictName":"测试",
                "status":1
            }
            ```
        - 保存从表
        
            `https://st-petrel.belle.net.cn/petrel/petrel-itg-api/itg/api/sysDictDtl/simple_add.json`
            
            ``` json
            {
                "itemCode":"test1",
                "itemName":"测试1",
                "status":1,
                "dictId":"1067716634390736898"
            }
            ```
    - 修改(主从表分开修改)
    
        `https://st-petrel.belle.net.cn/petrel/petrel-itg-api/itg/api/sysDictDtl/simple_update.json`
        
        ``` json
        {
            "id":"1067716933369114626",
            "dictId":"1067716634390736898",
            "itemCode":"test1",
            "itemName":"测试2",
            "status":1,
            "creator":"admin",
            "createTime":"2018-11-28 17:48:49",
            "modifier":"admin",
            "modifyTime":"2018-11-28 17:51:51"
        }
        ```
    - 删除(多表删除，会将主从表一起删除)
    
        `https://st-petrel.belle.net.cn/petrel/petrel-itg-api/itg/api/sysDict/multi_delete.jsonn`
        
        *参数：tableId: 1067716634390736898*
    - 查询
    
        `https://st-petrel.belle.net.cn/petrel/petrel-itg-api/itg/api/sysDict/find.json?search.dictCode_like=res_group&page.pn=1&page.size=10`

4. 多表常用基类方法(继承基类后，即可使用，无需额外编码)

    - `/multi_add.json` 主表保存
    
    - `/multi_delete.json` 多表删除

    - `/simple_add.json` 简单保存
    
    - `/simple_delete.json` 简单删除
    
    - `/simple_update.json` 简单更新
    
    - `/find.json` 分页查询 返回分页数据
    
    - `/findVOAll.json` 不分页查询所有数据 返回所有数据集合
    
    - `/findOne.json` 按主键查询 返回对应实体