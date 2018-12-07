#### 单表开发
1. 单表维护模块前端界面模型，实现单表的增删改查功能

    ![simplepage](/assets/simplepage.png)
  
2. 单表基类继承
    - 继承BaseSimplePageController
    - 通用的业务开发自定义的controller
    - ...

3. 前端请求参数封装格式
    - 新增
  
        `https://st-petrel.belle.net.cn/petrel/petrel-itg-api/itg/api/sysCompany/simple_add.json`
      
        ``` json
        {
          "address":"测试地址",
          "code":"Ctest",
          "status":1,
          "linkman":"测试联系人",
          "name":"测试公司",
          "workPhone":"13923232323",
          "remarks":""
        }
        ```
    - 修改
    
        `https://st-petrel.belle.net.cn/petrel/petrel-itg-api/itg/api/sysProject/simple_update.json`
        
        ``` json
        {
          "address":"测试地址",
          "code":"Ctest",
          "status":1,
          "linkman":"测试联系人",
          "name":"测试公司",
          "workPhone":"13923232323",
          "remarks":"test"
        }
        ```
    - 删除
    
        `https://st-petrel.belle.net.cn/petrel/petrel-itg-api/itg/api/sysCompany/simple_delete.json`
        
        *参数：tableId: 1067702981834747906*
    - 查询
    
        `https://st-petrel.belle.net.cn/petrel/petrel-itg-api/itg/api/sysProject/find.json?search.code_like=test&search.name_like=test&page.pn=1&page.size=10`
  
4. 单表常用基类方法(继承基类后，即可使用，无需额外编码)

    - `/simple_add.json` 简单保存
    
    - `/simple_delete.json` 简单删除
    
    - `/simple_update.json` 简单更新
    
    - `/find.json` 分页查询 返回分页数据
    
    - `/findVOAll.json` 不分页查询所有数据 返回所有数据集合
    
    - `/findOne.json` 按主键查询 返回对应实体