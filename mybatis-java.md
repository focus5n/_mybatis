# MyBatis - Java API

## structure

    - /my_application
      - /bin
      - /devlib
      - /lib (mybatis.*.jar)
      - /src
        - /org/myapp
          - /action
          - /data (mapper.java, mapper.xml, mybatis-config.xml)
          - /model
          - /service
          - /view
        - /properties (*.properties)
      - /test
        - /~
      - /web
        - /WEB-INF
          - /web.xml

## SqlSessions

    - Spring, Guice 등 DI Framework는 자동으로 SqlSessions를 생성하고 삽입한다.

### SqlSessionFactoryBuilder

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC">
            ...
            <dataSource type="POOLED">
            ...
        </environment>
        <environment id="production">
            <transactionManager type="MANAGED">
            ...
            <dataSource type="JNDI">
            ...
        </environment>
    </environments>

### Build SqlSessionFactory in mybatis.config.xml

    String resource = "org/mybatis/builder/mybatis-config.xml";
    InputStream inputStream = Resources.getResourceAsStream(resource);
    SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
    SqlSessionFactory factory = builder.build(inputStream);

==================================================

    DataSource dataSource = BaseDataTest.createBlogDataSource();
    TransactionFactory transactionFactory = new JdbcTransactionFactory();
    
    Environment environment = new Environment("development", transactionFactory, dataSource);
    Configuration configuration = new Configuration(environment);

    configuration.setLazyLoadingEnabled(true);
    configuration.setEnhancementEnabled(true);
    configuration.getTypeAliasRegistry().registerAlias(Blog.class);
    configuration.getTypeAliasRegistry().registerAlias(Post.class);
    configuration.getTypeAliasRegistry().registerAlias(Author.class);
    configuration.addMapper(BoundBlogMapper.class);
    configuration.addMapper(BoundAuthorMapper.class);

    SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
    SqlSessionFactory factory = builder.build(configuration);