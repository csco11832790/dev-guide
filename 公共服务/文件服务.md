#### 文件服务
- 使用场景

    目前mongoDb主要用于导出文件存放以及图片上传下载，例如用户头像上传等
        
- 上传文件
    - 配置文件上传相关参数
    
      ``` SQL
      drop table if exists sys_bucket_config;
      create table sys_bucket_config
      (
          id         		bigint 		not null 					comment 'ID',
          code 		 			varchar(64) not null				comment '编号', //业务编号 如：BMS
          name 		 			varchar(64) not null				comment '名称', //业务名称 如：合同文件
          value         varchar(64) not null				comment '集合名称,存储到Mongodb的表名',
          suffix  			varchar(64) default '*.*' 	comment '支持的文件后缀格式: *.pdf,*.gif,*.pmp', //默认为所有类型的文件
          file_size  		bigint 							        comment '文件大小,已kb为单位', //限制最大上传文件大小
          status  			tinyint		default 1					comment '状态，1是启用，0是禁用',
          creator				varchar(20) not null				comment '建档人',
          create_time		datetime 	not null					comment '建档时间',
          modifier			varchar(20) 							  comment '最后修改人',
          modify_time		datetime								    comment '最后修改时间',
          remarks				varchar(255) 							  comment '备注',
          constraint pk_sys_bucket_config primary key(id),
          constraint unique_sys_bucket_config unique(code)
      ) ENGINE=InnoDB DEFAULT CHARSET=utf8 comment = '集合配置';
      ```
    - 前端调用上传接口
      - 接口地址
      
        ``` url
            http://10.0.43.18:20041/file/upload/CG001
        ```
      - html demo
      
        ``` html
        <form action="/file/upload/CG001" method="post" enctype="multipart/form-data">
            <input type="file" name="smallIcon">
            <input type="submit" value="提交">
        </form>
        ```
      - CG001参数为上一步中维护的code
      
    - 接口返回格式
    
      ``` json
      上传文件返回结果：
      {
          "flag": {
              "retCode": "0",
              "retMsg": "success"
          },
          "data": [
              {
                  "fileId": "5c050b32dbe31639e6ca1ee6",
                  "fileName": "测试文件上传.xlsx",
                  "fileSize": "23504",
                  "fileMD5": "932630b254cd55377286ba9f9078c6b7",
                  "fileUploadDate": "2018-12-03 18:53:38"
              }
          ]
      }
      ```
      
    - 前端拿到fileId后，填充实体中的图片url，并向后端请求实体保存接口
    - 多文件上传demo例子待前端集成后组件方可使用
    
    
- 文件下载
  - 前端发起下载文件请求
  - 请求链接
  
    ``` url
    http://10.0.43.18:20041/file/download/CG001/5bff5611473dd20689ab8455
    ```
    
    - CG001: 维护的code
    - 5bff5611473dd20689ab8455: 上传时返回并存入实体url字段的fieldId