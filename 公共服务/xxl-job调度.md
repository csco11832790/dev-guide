#### 调度平台
- 引入依赖
  ``` xml
  <dependency>
    <groupId>com.belle</groupId>
    <artifactId>petrel-schedule-client</artifactId>
    <version>1.0.0-SNAPSHOT</version>
  </dependency>
  ```
  
- 添加配置

  ``` property
  xxl:
  job:
    executor:
      #本地日志存放路径
      logpath: /Users/liqi/logs/xxl/
      #执行器
      appname: xxl-job-executor-sample2
      port: 9999
      logretentiondays: -1
      ip:
    admin:
      #调度中心地址
      addresses: http://10.0.43.18:10010
    accessToken:
  ```
  
- 调度任务开发
  - 开发步骤
  
    >1、继承"IJobHandler"：“com.xxl.job.core.handler.IJobHandler”；
    
    >2、注册到Spring容器：添加“@Component”注解，被Spring容器扫描为Bean实例；
    
    >3、注册到执行器工厂：添加“@JobHandler(value="自定义jobhandler名称")”注解，注解value值对应的是调度中心新建任务的JobHandler属性的值。
    
    >4、执行日志：需要通过 "XxlJobLogger.log" 打印执行日志；
  - 代码demo
  
    ``` java
    /**
     * Created by liqi on 2018/12/4.
     */
    @JobHandler(value = "lqJobHandler")
    @Component
    public class LqJobHandler extends IJobHandler {

        @Autowired
        private BmCarrierService bmCarrierService;

        /**
         * 具体的任务功能说明
         * @param param 用于接收页面维护的参数
         * @return
         * @throws Exception
         */
        @Override
        public ReturnT<String> execute(String param) throws Exception {
            XxlJobLogger.log("Begin schedule");
            EntityWrapper<BmCarrier> entityWrapper = new EntityWrapper<>();
            entityWrapper.eq("del_tag", 0).last("limit 10");
            // 按del_flag=0 每次查询10条更新状态
            List<BmCarrier> list = bmCarrierService.selectList(entityWrapper);
            for (BmCarrier bmCarrier : list) {
                bmCarrier.setDelTag(1);
            }
            bmCarrierService.updateBatchById(list);
            XxlJobLogger.log("Begin end");
            return SUCCESS;
        }
    }
    ```
  - 新增执行器(每个工程配置一个执行器)
  
    ![新增执行器](/assets/新增执行器.png)
    
  - 本地服务启动后，会显示在线机器
  
    ![机器注册](/assets/机器注册.png)
    
  - 新增定时任务
  
    ![新增定时任务](/assets/新增定时任务.png)
    
    - 执行器：选择上一步中维护的对应的执行器
    - JobHandler：编写的具体调度服务(注解[@JobHandler(value = "lqJobHandler")] 中的value值)
    - corn：标准corn表达式


### &ensp;&ensp;[请参考:调度开发](http://wiki.wonhigh.cn:8090/pages/viewpage.action?pageId=24877520)