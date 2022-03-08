# 开发常用

## 使用jar包生成pom文件

命令：

```shell
mvn install:install-file -Dfile=[0]-DgroupId=[1] -DartifactId=[2] -Dversion=[3] -Dpackaging=jar
```

> [0] :  本机jar包的路径
>
> [1]：groupId
>
> [2]：artifactId
>
> [3]：version

示例：

- 依赖

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
```

- 对应的命令

```shell
mvn install:install-file -Dfile=E:\\xxx.jar -DgroupId=io.springfox -DartifactId=springfox-swagger2 -Dversion=2.9.2 -Dpackaging=jar
```

---

## Alibaba druid命令行加密

```shell
java -cp druid-1.1.10.jar com.alibaba.druid.filter.config.ConfigTools [0]
```

> [0] : password
>
> 注：jar包需位于当前路径下，版本可自行更换

---

## WEB参数设置-ContentType

```java
import javax.servlet.http.HttpServletResponse;
public void downloadBeforeSetting(HttpServletResponse resp) {
    // xlsx
    resp.setContentType("application/vnd.openxmlformats-officedocument.spreadsheetml.sheet;charset=UTF-8");
    // xls
    resp.setContentType("application/vnd.ms-excel;charset=UTF-8");
}
```

---

## Java新旧日期转换

```java
Date in = new Date();
LocalDateTime ldt = LocalDateTime.ofInstant(in.toInstant(), ZoneId.systemDefault());
Date out = Date.from(ldt.atZone(ZoneId.systemDefault()).toInstant());
```

## NIO拷贝文件

```java
public static void copyFileByChannel(File source, File dest) throws IOException {
    try (FileChannel sourceChannel = new FileInputStream(source).getChannel();
         FileChannel targetChannel = new FileOutputStream(dest).getChannel();) {
        for (long count = sourceChannel.size(); count > 0; ) {
            long transferred = sourceChannel.transferTo(sourceChannel.position(), count, targetChannel);
            sourceChannel.position(sourceChannel.position() + transferred);
            count -= transferred;
        }
    }
}
```

## 新版Swagger依赖

```xml
<dependency>
  <groupId>io.springfox</groupId>
  <artifactId>springfox-boot-starter</artifactId>
  <version>3.0.0</version>
</dependency>
```

## 
