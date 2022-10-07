# MyBatis - XML

## elements

    1. cache
        - 해당 네임스페이스 설정
    2. cache-ref
        - 다른 네임스페이스 설정 참조
    3. resultMap
        - DB Data를 Object에 로드하는 방법 정의
    4. sql
        - SQL 재사용
    5. insert
    6. update
    7. delete
    8. select

## select

    <select id="selectPerson" parameterType="int" resultType="hashmap">
        SELECT * FROM PERSON WHERE ID = #{id}
    </select>

    <select
        id="selectPerson"
        parameterType="int"
        parameterMap="deprecated"
        resultType="hashmap"
        resultMap="personResultMap">

### parameter

    #{PreparedStatement}

    - JDBC 사용하면 PreparedStatement에는 "?" 형태로 파라미터 전달

## insert, update, delete

    - id: PK
    - useGeneratedKeys, keyProperty: Id 자동 생성

    <insert id="insertAuthor" useGeneratedKeys="true" keyProperty="id">
        insert into
            Author (id,username,password,email,bio)
        values
            (#{id},#{username},#{password},#{email},#{bio})
    </insert>

    <update id="updateAuthor">
        update Author set
            username = #{username},
            password = #{password},
            email = #{email},
            bio = #{bio}
        where id = #{id}
    </update>

    <delete id="deleteAuthor">
        delete from Author where id = #{id}
    </delete>



    <insert
        id="insertAuthor"
        parameterType="domain.blog.Author"
        resultMap="someMap"
        resultType="int">

    <update
        id="updateAuthor"
        parameterType="domain.blog.Author"
        resultMap="someMap"
        resultType="int">

    <delete
        id="deleteAuthor"
        parameterType="domain.blog.Author"
        resultMap="someMap"
        resultType="int">

### multiple record

    - 사용하는 데이터베이스가 다중레코드 입력을 지원한다면, Author의 목록이나 배열을 전달할수 있고 자동생성키를 가져올 수 있다.

    <insert id="insertAuthor" useGeneratedKeys="true"
        keyProperty="id">
        insert into
            Author (username, password, email, bio)
        values
            <foreach item="item" collection="list" separator=",">
                (#{item.username}, #{item.password}, #{item.email}, #{item.bio})
            </foreach>
    </insert>

## sql

    <sql id="userColumns">
        ${alias}.id,
        ${alias}.username,
        ${alias}.password
    </sql>


    <select id="selectUsers" resultType="map">
        select
            <include refid="userColumns"><property name="alias" value="t1"/></include>,
            <include refid="userColumns"><property name="alias" value="t2"/></include>
        from some_table t1
            cross join some_table t2
    </select>


    <sql id="sometable">
        ${prefix}Table
    </sql>

    <sql id="someinclude">
        from
        <include refid="${include_target}"/>
    </sql>

    <select id="select" resultType="map">
        select
            field1, field2, field3
        <include refid="someinclude">
            <property name="prefix" value="Some"/>
            <property name="include_target" value="sometable"/>
        </include>
    </select>

## parameters

    - null 값을 걸러내려면, #{parameters, JdbcType=string} 이런 식으로 설정 가능

    <insert id="insertUser" parameterType="User">
        insert into
            users (id, username, password)
        values
            (#{id}, #{username}, #{password})
    </insert>


    - sql 구문에 별칭 지정

    <select id="selectUsers" resultType="User">
        select
            user_id             as "id",
            user_name           as "userName",
            hashed_password     as "hashedPassword"
        from some_table
        where id = #{id}
    </select>


    - resultMap에 별칭 지정

    <select id="selectUsers" resultMap="userResultMap">
        select user_id, user_name, hashed_password
        from some_table
        where id = #{id}
    </select>

    <resultMap id="userResultMap" type="User">
        <id property="id" column="user_id" />
        <result property="username" column="user_name"/>
        <result property="password" column="hashed_password"/>
        <association property="author" javaType="Author">
            <id property="id" column="author_id"/>
            <result property="username" column="author_username"/>
            <result property="password" column="author_password"/>
            <result property="email" column="author_email"/>
            <result property="bio" column="author_bio"/>
            <result property="favouriteSection" column="author_favourite_section"/>
            </association>
            <collection property="posts" ofType="Post">
                <id property="id" column="post_id"/>
                <result property="subject" column="post_subject"/>
                <association property="author" javaType="Author"/>
                <collection property="comments" ofType="Comment">
                <id property="id" column="comment_id"/>
            </collection>
    </resultMap>

### association, collection

    - association: has-one
    - collection: has-many

### discriminator

    - switch 효과: 여러 결과 중 도출된 결과에 매칭

    <resultMap id="vehicleResult" type="Vehicle">
        <id property="id" column="id" />
        <result property="vin" column="vin"/>
        <result property="year" column="year"/>
        <result property="make" column="make"/>
        <result property="model" column="model"/>
        <result property="color" column="color"/>
        <discriminator javaType="int" column="vehicle_type">
            <case value="1" resultMap="carResult"/>
            <case value="2" resultMap="truckResult"/>
            <case value="3" resultMap="vanResult"/>
            <case value="4" resultMap="suvResult"/>
        </discriminator>
    </resultMap>
