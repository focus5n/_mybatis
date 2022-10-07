# MyBatis - Intro

## Introduce

    - Database's SQL, Procedure, Advanced Mapping features to Java POJO
    - Use XML or Annotation for mapping

## Installation

    - mybatis.jar in project's classpath

## SqlSessionFactory

    - sqlSessionFactoryBuilder makes sqlSessionFactory
    - sqlSessionFactory

### Use Resources Class for classpath

    String resource = "org/mybatis/example/mybatis-config.xml";
    InputStream inputStream = Resources.getResourceAsStream(resource);
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);

## TransactionManager

    - Environment: 개발환경 설정, 트랜잭션 관리, 커넥션 풀링 관리
    - TransactionManager: 트랜잭션 설정
    - DataSource: 커넥션 풀링 설정

    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-config.dtd">

    <configuration>
    <environments default="development">
        <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <property name="driver" value="${driver}"/>
            <property name="url" value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>
        </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>
    </configuration>

## Use Java Configuration Class without XML Config

    DataSource dataSource = BlogDataSourceFactory.getBlogDataSource();
    TransactionFactory transactionFactory = new JdbcTransactionFactory();
    Environment environment = new Environment("development", transactionFactory, dataSource);
    Configuration configuration = new Configuration(environment);
    configuration.addMapper(BlogMapper.class);
    SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(configuration);

## sqlSessionFactory

    try (SqlSession session = sqlSessionFactory.openSession()) {
        BlogMapper mapper = session.getMapper(BlogMapper.class);
        Blog blog = mapper.selectBlog(101);
    }

## XML Mapper

    - mapper namespace: 인터페이스 바인딩
    - name resolution: 설정 엘리멘트를 위한 이름 분석 규칙 (com.exaple.mapper.SomeMapper)

    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">

    <mapper namespace="org.mybatis.example.BlogMapper">
        <select id="selectBlog" resultType="Blog">
            select
              * 
            from
                Blog
            where id = #{id}
        </select>
    </mapper>

## SQL in Mapper Interface without XML

    public interface BlogMapper {
        @Select("SELECT * FROM blog WHERE id = #{id}")
        Blog selectBlog(int id);
    }

## DI Framework for MyBatis

    - MyBatis-Spring
    - MyBatis-Guice

## SqlSession

    - 스프링이나 쥬스와 같은 의존성 삽입 프레임워크와 함께 사용할때 SqlSessions은 DI프레임워크에 의해 생성되고 삽입된다.
    - 그래서 SqlSessionFactoryBuilder나 SqlSessionFactory가 필요하지 않을 것이기 때문에 SqlSession섹션으로 바로 넘어가도 무방하다.
    - 추가적인 정보는 MyBatis-Spring이나 MyBatis-Guice를 참고하길 바란다.


    - SqlSessionFactoryBuilder
      - 유지할 필요가 없음
      - 메서드 지역변수
    - SqlSessionFactory
      - 애플리케이션을 작동하는 동안 존재해야 하므로 삭제하거나 재생성하지 않는 것이 가장 좋음
      - 애플리케이션 스코프
      - 싱글턴 패턴, static 싱글턴 패턴, DI Framework(MyBatis-Spring, MyBatis-Guice)
    - SqlSession
      - 모든 쓰레드는 SqlSession 인스턴스가 필요함
      - SqlSession 인스턴스는 공유되지 않고, 쓰레드에 안전하지도 않음
      - 메서드 스코프
      - try (SqlSession session = sqlSessionFactory.openSession()) {
            // do work
        }
    - Mapper Instance
      - 맵핑된 구문을 바인딩할 때 사용함
      - SqlSession에서 mapper Interface Instance 생성
      - SqlSession과 동일한 라이프사이클 보유