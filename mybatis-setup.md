# MyBatis - Setup

## Mapper Setting

### structure

    - configuration
      - properties
      - settings
      - typeAliases
      - typeHandlers
      - objectFactory
      - plugins
      - environments
        - environment
          - transactionManager
          - dataSource
      - databaseIdProvider
      - mappers

### properties

    - in Java properties
    - in properties element

    <properties resource="org/mybatis/example/config.properties">
        <property name="username" value="dev_user"/>
        <property name="password" value="F2Fa3!33TYyg"/>
    </properties>

    <dataSource type="POOLED">
        <property name="driver" value="${driver}"/>
        <property name="url" value="${url}"/>
        <property name="username" value="${username}"/>
        <property name="password" value="${password}"/>
    </dataSource>

### settings

    <settings>
        <setting name="cacheEnabled" value="true"/>
        <setting name="lazyLoadingEnabled" value="true"/>
        <setting name="aggressiveLazyLoading" value="true"/>
        <setting name="multipleResultSetsEnabled" value="true"/>
        <setting name="useColumnLabel" value="true"/>
        <setting name="useGeneratedKeys" value="false"/>
        <setting name="autoMappingBehavior" value="PARTIAL"/>
        <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
        <setting name="defaultExecutorType" value="SIMPLE"/>
        <setting name="defaultStatementTimeout" value="25"/>
        <setting name="defaultFetchSize" value="100"/>
        <setting name="safeRowBoundsEnabled" value="false"/>
        <setting name="safeResultHandlerEnabled" value="true"/>
        <setting name="mapUnderscoreToCamelCase" value="false"/>
        <setting name="localCacheScope" value="SESSION"/>
        <setting name="jdbcTypeForNull" value="OTHER"/>
        <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
        <setting name="defaultScriptingLanguage" value="org.apache.ibatis.scripting.xmltags.XMLLanguageDriver"/>
        <setting name="defaultEnumTypeHandler" value="org.apache.ibatis.type.EnumTypeHandler"/>
        <setting name="callSettersOnNulls" value="false"/>
        <setting name="returnInstanceForEmptyRow" value="false"/>
        <setting name="logPrefix" value="exampleLogPreFix_"/>
        <setting name="logImpl" value="SLF4J | LOG4J | LOG4J2 | JDK_LOGGING | COMMONS_LOGGING | STDOUT_LOGGING | NO_LOGGING"/>
        <setting name="proxyFactory" value="CGLIB | JAVASSIST"/>
        <setting name="vfsImpl" value="org.mybatis.example.YourselfVfsImpl"/>
        <setting name="useActualParamName" value="true"/>
        <setting name="configurationFactory" value="org.mybatis.example.ConfigurationFactory"/>
    </settings>


    - Alias
    - 대소문자 구분하지 않음

    <typeAliases>
        <typeAlias alias="Author" type="domain.blog.Author"/>
        <typeAlias alias="Blog" type="domain.blog.Blog"/>
        <typeAlias alias="Comment" type="domain.blog.Comment"/>
        <typeAlias alias="Post" type="domain.blog.Post"/>
        <typeAlias alias="Section" type="domain.blog.Section"/>
        <typeAlias alias="Tag" type="domain.blog.Tag"/>
    </typeAliases>

    <typeAliases>
        <package name="domain.blog"/>
    </typeAliases>

    
    - Mappers
    - SQL 구문이 있는 곳을 지정

    <!-- 매퍼로 패키지내 모든 인터페이스를 등록 -->
    <mappers>
        <package name="org.mybatis.builder"/>
    </mappers>
    
    <!-- 매퍼 인터페이스를 사용 -->
    <mappers>
        <mapper class="org.mybatis.builder.AuthorMapper"/>
        <mapper class="org.mybatis.builder.BlogMapper"/>
        <mapper class="org.mybatis.builder.PostMapper"/>
    </mappers>

    <!-- 클래스패스의 상대경로의 리소스 사용 -->
    <mappers>
        <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
        <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
        <mapper resource="org/mybatis/builder/PostMapper.xml"/>
    </mappers>
