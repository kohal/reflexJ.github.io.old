---
　　layout: default
　　title: something interesting 
---

### something interesting 

#### 


1. fastJson 序列化问题 
    - 问题描述及原因：

    ``` java

    public class Result<T> implements Serializable {
        public boolean isSuccess() {
        return String.valueOf(ResultCode.SUCCESS.getCode()).equals(code);
     }
    }

    Result<Object> result = Result.success(new Object());
        System.out.println(JSON.toJSONString(result));
        
    ```

    这个result的结果：

    ``` json
    {"code":"0","data":{},"message":"成功","success":true}
    ```
    这边会多一个success key，但是没有这个成员变量的，debug fastJson源码,发现序列化解析的时候会根据 isXxx 这种方法形式

    ``` java
    @link com.alibaba.fastjson.util.TypeUtils; 

     if (methodName.startsWith("is")) {
        Field field = ParserConfig.getField(clazz, propertyName);
     }

    ```

    - 延伸：看json序列化，看到了asm的应用

        [liangfei 的一篇动态代理方案的性能对比](http://javatar.iteye.com/blog/814426)


2. DataGrip 配置文件问题 
    -  之前鼓捣过datagrip的主题，安装后发现太丑了，就删除了，然后悲剧发现窗口边框黑了，看起来超变扭。
    - 
        * 配置文件的目录:/Users/xuyongjian/Library/Preferences/DataGrip2018.1

        * 之前删除过一次 colors/，这个只是主题颜色的配置，删除完 /options 后重启就恢复了，��

3. 新开的项目编译不通过
    - 问题描述：编译报这个鬼错，初步定位是lombok跟jdk版本不兼容的问题

    ``` java
    atal error compiling: java.lang.ExceptionInInitializerError: com.sun.tools.javac.code.TypeTags -> [Help 1] 
    ```

    ![lombok异常信息](/images/lombok_e.jpg)


    - 问题解决：当我在iterm中查看jdk版本是jdk1.8，然后又设置成jdk1.7，发现还是compile失败，崩溃了。。。纠结了好几个小时，然后我无意中在idea自带的terminal中 java -version 才发现居然是jdk10.这时候才发现是环境变量未加载的原因。重新在idea终端中执行`source ~/.bash_profile` 即可。

    - 事后：当一次jdk更新时，发现oracle的更新程序会自动修改JAVA_HOME这个环境变量为最新的版本，需重新source 用户的配置文件






 