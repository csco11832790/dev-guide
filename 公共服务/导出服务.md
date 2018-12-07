#### 导出
* 导出数据流说明
    - 前端发请求到导出服务
    - 导出服务解析findUrl及参数searchData、pageNum、pageSize，去对应的应用请求数据
    - 将请求的数据写入excel存入mongodb，并写通知
    - 前端页面打开通知面板，下载文件
* DEMO    
    * 请求类型： POST
    * 测试URL
    ``` url
    https://dev-petrel.belle.net.cn/petrel/petrel-export-center-api/exportCenter/export
    ```
    * 参数
    ``` json
    {
        "exportColumns": [{
            "field": "userName",
            "title": "用户名",
            "width": 120
        }, {
            "field": "projectId",
            "title": "编号",
            "width": 120
        }, {
            "field": "projectName",
            "title": "项目名称",
            "width": 120
        }, {
            "field": "companyName",
            "title": "集团名称",
            "width": 120
        }],
        "exportCombos": {
            "projectId": {
                "textFiled": "text",
                "valeFiled": "id",
                "comboData": [{
                    "id": "1",
                    "text": "启用"
                }, {
                    "id": "0",
                    "text": "禁用"
                }]
            },
            "enableFlag": {
                "textFiled": "text",
                "valeFiled": "id",
                "comboData": [{
                    "id": "1",
                    "text": "启用"
                }, {
                    "id": "0",
                    "text": "禁用"
                }]
            }
        },
        "fileName": "export",
        "findUrl": "https://dev-petrel.belle.net.cn/petrel/petrel-priv-api/priv/api/sysUserProject/find.json",
        "searchData": "search.e.loginName_like=sds",
        "pageNum": 1,
        "pageSize": 25,
        "exportFileFormat": "xlsX",
        "requestMethodType":"GET"
    }
    ```
    - 参数说明
        - fileName 导出文件名称
        - findUrl 导出数据查询url
        - searchData 导出数据查询参数
        - pageNum/pageSize 数据分页情况,不传代表查询所有
        - exportFileFormat 导出格式,默认:xlsx
        - compress是否压缩，默认不压缩
        - templateId暂时可不填,预留使用
    

