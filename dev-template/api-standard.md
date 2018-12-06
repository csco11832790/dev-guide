#### api开发规范(api层)
1. 使用场景：用于前后端交互，前端请求入口

2. 接口定义规范
    >@RestController<br/>
    >@RequestMapping("/itg/api/sysNotice")<br/>
    >@Api(tags = {"公告服务"})<br/>
  
3. 服务引入及命名建议
    >``` java
    >@Autowired
    >private SysNoticeService service; // 模块本身服务引入统一叫service
    >
    >@Autowired
    >private SysPrivApi sysPrivApi; // 相关服务引入名称采用驼峰命名，且与类名保持一致
    >```
  
4. 接口方法注解使用建议
    >``` java
    >@ApiOperation(value = "查询用户权限", httpMethod = "POST") // 接口功能说明
    >@PostMapping(value = "/findSysUserPermission.json") // 用特定的注解标明该接口的http请求方式
    >```
  
5. 接口返回值结构及封装
    >``` java
    >Map<String, Object> resultMap = WebUtils.getDefaultResultMap(); // 工具类取返回结构
    >Page<DataPrivilegeDtlVO> dtls = service.findList(page, entityWrapper); // 查询分页数据
    >resultMap.put(ResultConstants.ROWS, dtls.getRecords()); // 填充返回结构中的数据
    >resultMap.put(ResultConstants.TOTAL, page.getTotal()); // 填充返回结构中的总行数
    >return resultMap;
    >```