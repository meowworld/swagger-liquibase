# swagger-liquibase
swagger 和 liquibase的gradle中集成

随便写了一点，后面再修改吧

依赖：
	
	springfox-swagger2
	
	spring-swagger-ui

配置类
@EnableSwagger2	
@Configuration{
	
	1.最基本的配置
	
	  @Bean
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo());
    }

    public ApiInfo apiInfo(){
        Contact contact = new Contact("meow", "www.baidu.com", "418611516@qq.com");

        return new ApiInfo("我的测测文档", "测试文档描述","1.0.0 SHOPSNAP" ,
                "www.baidu.com", contact,"" , "",new ArrayList<>());
    }
	
	2.@Bean 默认的配置
    public Docket docket(){
        return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.any()).build();
    }
	
	3.配置包扫描
	return new Docket(DocumentationType.SWAGGER_2)
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.example.demo"))
				
				//.path("/hello/**")
                .build();
	
	4.忽略参数
	 return new Docket(DocumentationType.SWAGGER_2)
                .ignoredParameterTypes(HttpServletRequest.class);
	
	
	5.那种环境下展示
	如何动态配置当项目处于test、dev环境时显示swagger，处于prod时不显示？
	@Bean
    public Docket docket(Environment environment) {
        // 设置要显示swagger的环境
        Profiles of = Profiles.of("dev", "test");
        // 判断当前是处于该环境，通过 enable() 接收此参数判断是否要显示
        boolean b = environment.acceptsProfiles(of);

        return new Docket(DocumentationType.SWAGGER_2).apiInfo(apiInfo())
                .ignoredParameterTypes(HttpServletRequest.class)
                .enable(b) // 配置是否启用Swagger，如果是false，在浏览器将无法访问
                .select()
                .apis(RequestHandlerSelectors.basePackage("com.example.swaggerexample.controller")).build();
    }


	6.配置多个分组只需要配置多个docket即可：

	@Bean
	public Docket docket1() {
		return new Docket(DocumentationType.SWAGGER_2).groupName("group1");
	}
	@Bean
	public Docket docket2() {
		return new Docket(DocumentationType.SWAGGER_2).groupName("group2");
	}
	@Bean
	public Docket docket3() {
		return new Docket(DocumentationType.SWAGGER_2).groupName("group3");
	}

	7.实体配置
	@ApiModel("用户实体")
	public class User {
		@ApiModelProperty("用户名")
		private String username;
		@ApiModelProperty("密码")
		private String password;
		// 省略getter/setter
	}

	8.配置全局参数
	Parameter parameter = new ParameterBuilder().name("token")
                .description("登录令牌")
                .parameterType("header") //query
				.required(true)
                .modelRef(new ModelRef("String"))
                .build();
        List<Parameter> list = new ArrayList<>();
        list.add(parameter);

        return new Docket(DocumentationType.SWAGGER_2)
                .globalOperationParameters(list);




