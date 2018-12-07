#### 接口方法参数接收的几种方式
- page+map
- page+vo对象
- page+entitywrapper
- demo:
    - controller层:
        ``` java
        /**
         * <p>
         * 承运商信息 前端控制器
         * </p>
         *
         * @author zhang.mj
         * @since 2018-11-30
         */
        @FeignRestController
        @RequestMapping("/bmCarrier")
        @Api(tags = {"多参数类型DEMO"})
        public class BmCarrierController extends BaseSimplePageController<BmCarrier> {
        
            @Autowired
            private BmCarrierService service;
        
            @Override
            protected void init() {
        
                setBaseService(service);
                //设置VO实体
                //setMainVOClazz(UcUser.class);
            }
        
            /**
             * Map形式传参demo
             *
             *  关于接收参数使用@RequestParam、@RequestBody的说明
             *  RequestParam，接收的参数是来自于链接中拼接的参数
             *  RequestBody，接收的参数是来自于requestBody，即请求体中
             *
             *  请求链接：
             *  http://localhost:31003/bmCarrier/carrier?page.pn=1&page.size=10
             *      &carrierNo=1&carrierName=a&carrierType=1
             *      &createTime=2018-11-30 12:12:32
             *
             * @param page
             * @param params
             * @return
             */
            @ApiOperation(value = "Map形式传参demo", httpMethod = "GET", notes = "carrierNo=c01, carrierName like 测试, carrierType=1")
            @GetMapping(value = "/carrier")
            public Map<String,Object> testMapParameter(Page<BmCarrier> page, @RequestParam Map<String,Object> params){
        
                //检查参数
                if (params.get("carrierNo") == null || params.get("carrierName") == null || params.get("carrierType") == null) {
                    throw new MyselfMsgException("参数不完整！");
                }
        
                // 定义返回结果
                Map<String,Object> resultMap = WebUtils.getDefaultResultMap();
        
                // 两种形式
                // 1.将参数放入entityWrapper，利用${ew.sqlSegment} 让mybatis plus自动解析
                // 2.将map参数整个传入，在xml中手工解析
                //此处采用第一种方式
                EntityWrapper<BmCarrier> entityWrapper = new EntityWrapper<>();
                entityWrapper.eq("carrier_no", params.get("carrierNo").toString());
                entityWrapper.like("carrier_name", params.get("carrierName").toString());
                entityWrapper.eq("carrier_type", Integer.valueOf(params.get("carrierType").toString()));
        
                // 调用基类分页查询方法
                Page<BmCarrier> bmCarriers = service.selectPage(page, entityWrapper);
        
                // 填充并返回结果集
                resultMap.put(ResultConstants.ROWS, bmCarriers.getRecords());
                resultMap.put(ResultConstants.TOTAL, page.getTotal());
                return resultMap;
            }
        
            /**
             * VO形式传参demo
             *
             *  请求链接：
             *  http://localhost:31003/bmCarrier/carrier2?page.pn=1&page.size=10
             *      &carrierName=t&carrierType=1
             *
             *  tip1：GET请求，实体接收参数时，不能加@RequestParam
             *
             *  tip2：GET请求，实体接收参数时，日期类型转换会出错，即vo里面不能传日期相关的字段当做条件
             *  解决办法：
             *  在controller里增加以下方法
             *  @InitBinder
             *  public void initBinder(WebDataBinder binder) {
             *      SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd"); // 日期格式自己定义
             *      binder.registerCustomEditor(Date.class, new CustomDateEditor(dateFormat, false));
             *  }
             *
             * @param page
             * @param bmCarrierVO
             * @return
             */
            @ApiOperation(value = "VO形式传参demo", httpMethod = "GET")
            @GetMapping(value = "/carrier2")
            public Map<String,Object> testVoParameter(Page<BmCarrier> page, BmCarrierVO bmCarrierVO){
                // 定义返回结果
                Map<String,Object> resultMap = WebUtils.getDefaultResultMap();
        
                // 两种形式
                // 1.将参数从vo中get出来，放入entityWrapper中，利用${ew.sqlSegment} 让mybatis plus自动解析
                // 2.将vo整体传递，在xml中手工解析
                //此处采用第二种方式
                Page<BmCarrier> bmCarriers = service.selectPageByVO(page, bmCarrierVO);
        
                // 填充并返回结果集
                resultMap.put(ResultConstants.ROWS, bmCarriers.getRecords());
                resultMap.put(ResultConstants.TOTAL, page.getTotal());
                return resultMap;
            }
        
            /**
             * EntityWrapper形式传参demo
             *
             *  请求链接：
             *  http://localhost:31003/bmCarrier/carrier3?page.pn=1&page.size=10
             *      &search.carrierName_like=t&search.carrierType_eq=1
             *      &search.createTime_eq=2018-11-30 12:12:32
             *
             * @param page
             * @param entityWrapper
             * @return
             */
            @ApiOperation(value = "EntityWrapper形式传参demo", httpMethod = "GET")
            @GetMapping(value = "/carrier3")
            public Map<String,Object> testWrapperParameter(Page<BmCarrier> page, EntityWrapper<BmCarrier> entityWrapper){
                // 定义返回结果
                Map<String,Object> resultMap = WebUtils.getDefaultResultMap();
        
                // 调用基类查询方法
                Page<BmCarrier> bmCarriers = service.selectPage(page, entityWrapper);
        
                // 填充并返回结果集
                resultMap.put(ResultConstants.ROWS, bmCarriers.getRecords());
                resultMap.put(ResultConstants.TOTAL, page.getTotal());
                return resultMap;
            }
        }
        ```
        
    - service层
        ``` java
        /**
         * <p>
         * 承运商信息 服务类
         * </p>
         *
         * @author zhang.mj
         * @since 2018-11-30
         */
        
        public interface BmCarrierService extends BaseService<BmCarrier> {
        
            /**
             * VO形式传参demo
             * @param page
             * @param vo
             * @return
             */
            Page<BmCarrier> selectPageByVO(Page<BmCarrier> page, BmCarrierVO vo);
        }
        ```
    
    - mapper层
        ``` java
        /**
         * <p>
         * 承运商信息 Mapper 接口
         * </p>
         *
         * @author zhang.mj
         * @since 2018-11-30
         */
        public interface BmCarrierMapper extends BaseCrudMapper<BmCarrier> {
        
            /**
             * VO形式传参demo
             * @param page
             * @param vo
             * @return
             */
            Page<BmCarrier> selectPageByVO(Page<BmCarrier> page, BmCarrierVO vo);
        }
        ```
    
    - mapper.xml
        ``` xml
        <mapper namespace="com.belle.bms.chargingsys.core.mapper.BmCarrierMapper">
            <!-- 通用查询映射结果 -->
            <resultMap id="BaseResultMap" type="com.belle.bms.chargingsys.core.entity.BmCarrier">
                <id column="id" property="id" />
                <result column="carrier_no" property="carrierNo" />
                <result column="carrier_name" property="carrierName" />
                <result column="carrier_type" property="carrierType" />
                <result column="contacts_man" property="contactsMan" />
                <result column="contacts_phone" property="contactsPhone" />
                <result column="status" property="status" />
                <result column="del_tag" property="delTag" />
                <result column="creator" property="creator" />
                <result column="create_time" property="createTime" />
                <result column="modifier" property="modifier" />
                <result column="modify_time" property="modifyTime" />
                <result column="update_time" property="updateTime" />
            </resultMap>
        
            <!-- VO条件拼接 -->
            <sql id="BASE_COLUMN_CONDITION">
                <if test = "carrierNo != null and '' != carrierNo">
                    AND carrier_no = #{carrierNo,jdbcType=VARCHAR}
                </if>
        
                <if test = "carrierName != null and '' != carrierName">
                    AND carrier_name = #{carrierName,jdbcType=VARCHAR}
                </if>
        
                <if test = "carrierType != null and '' != carrierType">
                    AND carrier_type = #{carrierType,jdbcType=INTEGER}
                </if>
            </sql>
        
            <select id="selectPageByVO" parameterType="com.belle.bms.chargingsys.core.domain.vo.BmCarrierVO" resultMap="BaseResultMap" >
                SELECT carrier_no, carrier_name, carrier_type, contacts_man, contacts_phone, status, del_tag, creator,
                  create_time, modifier, modify_time
                FROM bm_carrier
                WHERE 0 &lt;&gt; 1
                <include refid="BASE_COLUMN_CONDITION"></include>
            </select>
        
        </mapper>
        ```