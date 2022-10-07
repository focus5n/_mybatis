# MyBatis - Logging

## implements

    1. slf4j (logback)
    2. apache commons logging (Jakarta CommonS Logging, JCL)
    3. log4j2
    4. JDK logging

    - Tomcat 등 웹앱서버는 JCL을 사용하므로 그 이하 단계는 무시된다.
    - slf4j를 사용하자

## slf4j + logback Install

    - into /WEB-INF/lib
    - -classpath 시작 파라미터에 입력

    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.x.x</version>
    </dependency>

### logback setting

    package org.mybatis.example;
    public interface BlogMapper {
        @Select("SELECT * FROM blog WHERE id = #{id}")
        Blog selectBlog(int id);
    }


    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE configuration>
    <configuration>

        <!-- 결과 출력 -->
        <appender name="stdout" class="ch.qos.logback.core.ConsoleAppender">
            <encoder>
                <pattern>%5level [%thread] - %msg%n</pattern>
            </encoder>
        </appender>

        <logger name="org.mybatis.example">
            <level value="trace"/>
        </logger>
        <root level="error">
            <appender-ref ref="stdout"/>
        </root>

        <!-- 결과가 아닌 구문만 보고 싶다면 -->
        <logger name="org.mybatis.example">
            <level value="debug"/>
        </logger>

    </configuration>

## log4j2 install

    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.x.x</version>
    </dependency>

### log4j2 setting

    <?xml version="1.0" encoding="UTF-8"?>
    <Configuration xmlns="http://logging.apache.org/log4j/2.0/config">

        <Appenders>
        <Console name="stdout" target="SYSTEM_OUT">
            <PatternLayout pattern="%5level [%t] - %msg%n"/>
        </Console>
        </Appenders>

        <Root level="error" >
            <AppenderRef ref="stdout"/>
        </Root>
        </Loggers>

    </Configuration>