title: mybatis generator
date: 2017-11-29 12:21:04
category: 笔记
tags: mybatis-generator
---
## maven依赖
    <dependency>
      <groupId>org.mybatis.generator</groupId>
      <artifactId>mybatis-generator-core</artifactId>
      <version>1.3.2</version>
     </dependency>

## generatorConfig.xml配置文件
    <?xml version="1.0" encoding="UTF-8"?>
        <!DOCTYPE generatorConfiguration
            PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
            "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
    <generatorConfiguration>
    <context id="Mysql">

        <!-- 生成的Java文件的编码 -->
        <property name="javaFileEncoding" value="UTF-8"/>
        <!-- 格式化java代码 -->
        <property name="javaFormatter" value="org.mybatis.generator.api.dom.DefaultJavaFormatter"/>
        <!-- 格式化XML代码 -->
        <property name="xmlFormatter" value="org.mybatis.generator.api.dom.DefaultXmlFormatter"/>
        <commentGenerator type="MyCommentGenerator">
            <!-- <property name="suppressDate" value="true"/>
             &lt;!&ndash; 是否去除自动生成的注释 true：是 ： false:否 &ndash;&gt;
             <property name="suppressAllComments" value="false"/>-->
        </commentGenerator>

        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://192.168.4.246:3306/qa_guser?useUnicode=true"
                        userId="qa_user"
                        password="1QAZ@wsx">
        </jdbcConnection>

        <javaModelGenerator targetPackage="com.tlkg.guser.biz.dal.dataobject.sso" targetProject="/Users/jianyun.zheng/dev/tlkg-guser/biz/dal/src/main/java"/>

        <sqlMapGenerator targetPackage="sys"  targetProject="/Users/jianyun.zheng/dev/tlkg-guser/biz/dal/src/main/resources/sqlmap"/>

        <javaClientGenerator targetPackage="com.tlkg.guser.biz.dal.dataobject.sso" targetProject="/Users/jianyun.zheng/dev/tlkg-guser/biz/dal/src/main/java" type="XMLMAPPER" />

        <table tableName="sys_app_area_product" domainObjectName="sysAppAreaProductDO" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false">
            <generatedKey column="id" sqlStatement="Mysql" identity="true"/>
        </table>
    </context>
    </generatorConfiguration>

## 注释类
    import static org.mybatis.generator.internal.util.StringUtility.isTrue;

    import java.text.SimpleDateFormat;
    import java.util.Date;
    import java.util.Properties;

    import org.mybatis.generator.api.CommentGenerator;
    import org.mybatis.generator.api.IntrospectedColumn;
    import org.mybatis.generator.api.IntrospectedTable;
    import org.mybatis.generator.api.dom.java.CompilationUnit;
    import org.mybatis.generator.api.dom.java.Field;
    import org.mybatis.generator.api.dom.java.InnerClass;
    import org.mybatis.generator.api.dom.java.InnerEnum;
    import org.mybatis.generator.api.dom.java.JavaElement;
    import org.mybatis.generator.api.dom.java.Method;
    import org.mybatis.generator.api.dom.java.Parameter;
    import org.mybatis.generator.api.dom.xml.XmlElement;
    import org.mybatis.generator.config.MergeConstants;
    import org.mybatis.generator.config.PropertyRegistry;

    public class MyCommentGenerator implements CommentGenerator{
        private Properties properties;
        private Properties systemPro;
        private boolean suppressDate;
        private boolean suppressAllComments;
        private String currentDateStr;

        public MyCommentGenerator() {
            super();
            properties = new Properties();
            systemPro = System.getProperties();
            suppressDate = false;
            suppressAllComments = false;
            currentDateStr = (new SimpleDateFormat("yyyy-MM-dd")).format(new Date());
        }

        public void addJavaFileComment(CompilationUnit compilationUnit) {
            // add no file level comments by default
            return;
        }

        /**
         * Adds a suitable comment to warn users that the element was generated, and
         * when it was generated.
         */
        public void addComment(XmlElement xmlElement) {
            return;
        }

        public void addRootComment(XmlElement rootElement) {
            // add no document level comments by default
            return;
        }

        public void addConfigurationProperties(Properties properties) {
            this.properties.putAll(properties);

            suppressDate = isTrue(properties.getProperty(PropertyRegistry.COMMENT_GENERATOR_SUPPRESS_DATE));

            suppressAllComments = isTrue(properties.getProperty(PropertyRegistry.COMMENT_GENERATOR_SUPPRESS_ALL_COMMENTS));
        }

        /**
         * This method adds the custom javadoc tag for. You may do nothing if you do
         * not wish to include the Javadoc tag - however, if you do not include the
         * Javadoc tag then the Java merge capability of the eclipse plugin will
         * break.
         *
         * @param javaElement
         *            the java element
         */
        protected void addJavadocTag(JavaElement javaElement, boolean markAsDoNotDelete) {
            javaElement.addJavaDocLine(" *");
            StringBuilder sb = new StringBuilder();
            sb.append(" * ");
            sb.append(MergeConstants.NEW_ELEMENT_TAG);
            if (markAsDoNotDelete) {
                sb.append(" do_not_delete_during_merge");
            }
            String s = getDateString();
            if (s != null) {
                sb.append(' ');
                sb.append(s);
            }
            javaElement.addJavaDocLine(sb.toString());
        }

        /**
         * This method returns a formated date string to include in the Javadoc tag
         * and XML comments. You may return null if you do not want the date in
         * these documentation elements.
         *
         * @return a string representing the current timestamp, or null
         */
        protected String getDateString() {
            String result = null;
            if (!suppressDate) {
                result = currentDateStr;
            }
            return result;
        }

        public void addClassComment(InnerClass innerClass, IntrospectedTable introspectedTable) {
            if (suppressAllComments) {
                return;
            }
            StringBuilder sb = new StringBuilder();
            innerClass.addJavaDocLine("/**");
            sb.append(" * ");
            sb.append(introspectedTable.getFullyQualifiedTable());
            sb.append(" ");
            sb.append(getDateString());
            innerClass.addJavaDocLine(sb.toString().replace("\n", " "));
            innerClass.addJavaDocLine(" */");
        }

        public void addEnumComment(InnerEnum innerEnum, IntrospectedTable introspectedTable) {
            if (suppressAllComments) {
                return;
            }
            StringBuilder sb = new StringBuilder();
            innerEnum.addJavaDocLine("/**");
            sb.append(" * ");
            sb.append(introspectedTable.getFullyQualifiedTable());
            innerEnum.addJavaDocLine(sb.toString().replace("\n", " "));
            innerEnum.addJavaDocLine(" */");
        }

        public void addFieldComment(Field field, IntrospectedTable introspectedTable,
            IntrospectedColumn introspectedColumn) {
            if (suppressAllComments) {
                return;
            }
            StringBuilder sb = new StringBuilder();
            field.addJavaDocLine("/**");
            sb.append(" * ");
            sb.append(introspectedColumn.getRemarks());
            field.addJavaDocLine(sb.toString().replace("\n", " "));
            field.addJavaDocLine(" */");
        }

        public void addFieldComment(Field field, IntrospectedTable introspectedTable) {
            if (suppressAllComments) {
                return;
            }
            StringBuilder sb = new StringBuilder();
            field.addJavaDocLine("/**");
            sb.append(" * ");
            sb.append(introspectedTable.getFullyQualifiedTable());
            field.addJavaDocLine(sb.toString().replace("\n", " "));
            field.addJavaDocLine(" */");
        }

        public void addGeneralMethodComment(Method method, IntrospectedTable introspectedTable) {
            if (suppressAllComments) {
                return;
            }
            method.addJavaDocLine("/**");
            addJavadocTag(method, false);
            method.addJavaDocLine(" */");
        }

        public void addGetterComment(Method method, IntrospectedTable introspectedTable,
            IntrospectedColumn introspectedColumn) {
            if (suppressAllComments) {
                return;
            }
            method.addJavaDocLine("/**");
            StringBuilder sb = new StringBuilder();
            sb.append(" * ");
            sb.append(introspectedColumn.getRemarks());
            method.addJavaDocLine(sb.toString().replace("\n", " "));
            sb.setLength(0);
            sb.append(" * @return ");
            sb.append(introspectedColumn.getActualColumnName());
            sb.append(" ");
            sb.append(introspectedColumn.getRemarks());
            method.addJavaDocLine(sb.toString().replace("\n", " "));
            method.addJavaDocLine(" */");
        }

        public void addSetterComment(Method method, IntrospectedTable introspectedTable,
            IntrospectedColumn introspectedColumn) {
            if (suppressAllComments) {
                return;
            }
            method.addJavaDocLine("/**");
            StringBuilder sb = new StringBuilder();
            sb.append(" * ");
            sb.append(introspectedColumn.getRemarks());
            method.addJavaDocLine(sb.toString().replace("\n", " "));
            Parameter parm = method.getParameters().get(0);
            sb.setLength(0);
            sb.append(" * @param ");
            sb.append(parm.getName());
            sb.append(" ");
            sb.append(introspectedColumn.getRemarks());
            method.addJavaDocLine(sb.toString().replace("\n", " "));
            method.addJavaDocLine(" */");
        }

        public void addClassComment(InnerClass innerClass, IntrospectedTable introspectedTable, boolean markAsDoNotDelete) {
            if (suppressAllComments) {
                return;
            }
            StringBuilder sb = new StringBuilder();
            innerClass.addJavaDocLine("/**");
            sb.append(" * ");
            sb.append(introspectedTable.getFullyQualifiedTable());
            innerClass.addJavaDocLine(sb.toString().replace("\n", " "));
            sb.setLength(0);
            sb.append(" * @author ");
            sb.append(systemPro.getProperty("user.name"));
            sb.append(" ");
            sb.append(currentDateStr);
            innerClass.addJavaDocLine(" */");
        }
    }

 ## main方法
    /*
     * Project: tlkg-guser
     *
     * File Created at 2017-11-23
     *
     * Copyright 2012-2015 tlkg.com.cn Corporation Limited.
     * All rights reserved.
     *
     * This software is the confidential and proprietary information of
     * tlkg Company. ("Confidential Information").  You shall not
     * disclose such Confidential Information and shall use it only in
     * accordance with the terms of the license agreement you entered into
     * with tlkg.com.
     */

    /**
     * TODO
     *
     * @author jianyun.zheng
     * @version V1.0
     * @since 2017-11-23 11:32
     */
    import java.io.IOException;
    import java.io.InputStream;
    import java.net.URISyntaxException;
    import java.sql.SQLException;
    import java.util.ArrayList;
    import java.util.List;

    import org.mybatis.generator.api.MyBatisGenerator;
    import org.mybatis.generator.config.Configuration;
    import org.mybatis.generator.config.xml.ConfigurationParser;
    import org.mybatis.generator.exception.InvalidConfigurationException;
    import org.mybatis.generator.exception.XMLParserException;
    import org.mybatis.generator.internal.DefaultShellCallback;


    public class StartUp {
        public static void main(String[] args) throws URISyntaxException {
            try {
                List<String> warnings = new ArrayList<String>();
                boolean overwrite = true;
                ClassLoader classloader = Thread.currentThread().getContextClassLoader();
                InputStream is = classloader.getResourceAsStream("generatorConfig.xml");
                ConfigurationParser cp = new ConfigurationParser(warnings);
                Configuration config = cp.parseConfiguration(is);
                DefaultShellCallback callback = new DefaultShellCallback(overwrite);
                MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config, callback, warnings);
                myBatisGenerator.generate(null);
            } catch (SQLException e) {
                e.printStackTrace();
            } catch (IOException e) {
                e.printStackTrace();
            } catch (InterruptedException e) {
                e.printStackTrace();
            } catch (InvalidConfigurationException e) {
                e.printStackTrace();
            } catch (XMLParserException e) {
                e.printStackTrace();
            }
        }
    }



