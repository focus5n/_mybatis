# MyBatis - Dynamic

## features

    1. if
    2. choose (when, otherwise)
    3. trim (where, set)
    4. foreach

## if

    <select id="findActiveBlogWithTitleLike" resultType="Blog">
        SELECT
           * 
        FROM
            BLOG
        WHERE
            state = ‘ACTIVE’
        <if test="title != null">
            AND title like #{title}
        </if>
        <if test="author != null and author.name != null">
            AND author_name like #{author.name}
        </if>
    </select>

## choose, when, foreach

    <select id="findActiveBlogLike" resultType="Blog">
        SELECT
           * 
        FROM
            BLOG
        WHERE
            state = ‘ACTIVE’
        <choose>
            <when test="title != null">
                AND title like #{title}
            </when>
            <when test="author != null and author.name != null">
                AND author_name like #{author.name}
            </when>
            <otherwise>
                AND featured = 1
            </otherwise>
        </choose>
    </select>

## trim, where, set

    - trim은 쓸 일이 없을 것 같다

    <select id="findActiveBlogLike" resultType="Blog">
        SELECT
           * 
        FROM
            BLOG
        <where>
        <if test="state != null">
            state = #{state}
        </if>
        <if test="title != null">
            AND title like #{title}
        </if>
        <if test="author != null and author.name != null">
            AND author_name like #{author.name}
        </if>
        </where>
    </select>

    <update id="updateAuthorIfNecessary">
        update
            Author
        <set>
            <if test="username != null">username=#{username},</if>
            <if test="password != null">password=#{password},</if>
            <if test="email != null">email=#{email},</if>
            <if test="bio != null">bio=#{bio}</if>
        </set>
        where
            id=#{id}
    </update>

## foreach

    <select id="selectPostIn" resultType="domain.blog.Post">
        SELECT
            *
        FROM
            POST P
        <where>
            <foreach item="item" index="index" collection="list" open="ID in (" separator="," close=")" nullable="true">
                #{item}
            </foreach>
        </where>
    </select>

## script

    - in Mapper Class

    @Update({"<script>",
      "update Author",
      "  <set>",
      "    <if test='username != null'>username=#{username},</if>",
      "    <if test='password != null'>password=#{password},</if>",
      "    <if test='email != null'>email=#{email},</if>",
      "    <if test='bio != null'>bio=#{bio}</if>",
      "  </set>",
      "where id=#{id}",
      "</script>"})
    void updateAuthorValues(Author author);

