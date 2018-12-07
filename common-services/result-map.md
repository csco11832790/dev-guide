#### 结果封装
- 接口返回结果统一使用工具类ResultMapUtils
- petrel-common 1.0.1-SNAPSHOT及以上版本支持
- 弃用原工具类WebUtils
- 方法说明
  ``` java
  /**
     * 获取默认的resultMap
     *
     * @return
     */
    public static Map<String, Object> getSuccessResultMap() {
        Map<String, Object> resultMap = Maps.newHashMap();
        ResultVO flag = new ResultVO();
        flag.setRetMsg("success!");

        resultMap.put(ResultConstants.FLAG, flag);
        return resultMap;
    }

    /**
     * 返回带成功信息的resultMap
     * @param successMsg
     * @return
     */
    public static Map<String, Object> getSuccessResultMap(String successMsg) {
        Map<String, Object> resultMap = Maps.newHashMap();
        ResultVO flag = new ResultVO();
        flag.setRetMsg(successMsg);

        resultMap.put(ResultConstants.FLAG, flag);
        return resultMap;
    }

    /**
     * 返回带错误信息的resultMap
     * @param errorMsg
     * @return
     */
    public static Map<String, Object> getErrorResultMap(String errorMsg) {
        Map<String, Object> resultMap = new HashMap<>();
        ResultVO flag = new ResultVO();
        flag.setRetCode(ResultVO.ERROR_CODE);
        if (StringUtils.isNotBlank(errorMsg))
            flag.setRetMsg(errorMsg);

        resultMap.put(ResultConstants.FLAG, flag);
        return resultMap;
    }

    /**
     * 返回带错误编码、错误信息的resultMap
     * @param errCode
     * @param errorMsg
     * @return
     */
    public static Map<String, Object> getErrorResultMap(String errCode, String errorMsg) {
        Map<String, Object> resultMap = new HashMap<>();
        ResultVO flag = new ResultVO();
        flag.setRetCode(errCode);
        if (StringUtils.isNotBlank(errorMsg))
            flag.setRetMsg(errorMsg);

        resultMap.put(ResultConstants.FLAG, flag);
        return resultMap;
    }

    /**
     * 返回带错误编码、错误信息、错误信息明细的resultMap
     * @param errCode
     * @param errorMsg
     * @param detailMessage
     * @return
     */
    public static Map<String, Object> getErrorResultMap(String errCode, String errorMsg, String detailMessage) {
        Map<String, Object> resultMap = new HashMap<>();
        ResultVO flag = new ResultVO();
        flag.setRetCode(errCode);
        if (StringUtils.isNotBlank(errorMsg))
            flag.setRetMsg(errorMsg);
        if (StringUtils.isNotBlank(detailMessage))
            flag.setRetDetail(detailMessage);

        resultMap.put(ResultConstants.FLAG, flag);
        return resultMap;
    }
  ```