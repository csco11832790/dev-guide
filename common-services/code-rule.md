#### 使用场景
* 后端生成单据时，取单据编号

#### 2. 取单号接口
``` java
@RequestMapping(value = "/itg/api/sysCodeRule/getSheetIdCode.json", method = RequestMethod.GET)
public String getSystemCode(@RequestParam(value = "codeRuleNo") String codeRuleNo);

```

#### 3. 取单据号接口使用

* 引入取单号服务

    ``` java
    @Autowired
    private SysCodeRuleApi sysCodeRuleApi;
    ```

* 调用接口获取对应业务单据的单据号

#### 4. 批量取单号服务

...

