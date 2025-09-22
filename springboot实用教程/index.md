# SpringBoot简明教程


## SpringBoot 的优势

SpringBoot 是由Pivotal团队提供的全新框架，其设计目的是用来简化Spring应用的初始搭建以及开发过程。

简化操作体现在四个方面

- **parent** 统一控制版本，解决版本冲突问题
- **starter** 整合依赖的固定搭配格式，减少依赖配置
- **引导类** `@SpringBootApplication`，用于启动程序、初始化容器
- **内嵌 Tomcat**

## 配置

首先，需要用 maven 来将 SpringBoot 的环境搭建好。也可以从官网下载源文件进行导入。

在 `application.properties` 中可以更改已导入 starter 的配置。更进一步，可以在 `.yml` 以及 `.yaml` 中进行配置，简化书写。

配置中的数据也是可以被使用的，用Spring中的注解@Value读取单个数据，如 `@Value("{$server.port}")` ，同时，所有的数据封装到了一个 Environment 对象当中，可以调用。

### 技术整合

#### JUnit

```Java
@SpringBootTest
class Springboot04JunitApplicationTests {
    //注入你要测试的对象
    @Autowired
    private BookDao bookDao;
    @Test
    void contextLoads() {
        //执行要测试的对象对应的方法
        bookDao.save();
        System.out.println("two...");
    }
}

```

简化了测试类的编写，自动装配要测试的对象。

#### Mybatis

```java
@Mapper
public interface BookDao {
    @Select("select * from tbl_book where id = #{id}")
    public Book getById(Integer id);
}

```

导入、配置以后，用一个 Dao 映射端口即可实现 Mybatis 的功能。

#### Lombok

```java
import lombok.Data;
@Data
public class Book {
    private Integer id;
    private String type;
    private String name;
    private String description;
}

```

直接通过注解实现 getter、setter 等操作的自动添加。

## 基本开发流程

### 架构

JavaWeb 的开发一般为前后端分离开发，此处说明后端开发的架构。

#### Controller层

接收前端传输过来的数据，并将后端处理后的数据返回给前端，作为前后端联通的桥梁。

```java
@RestController
@RequestMapping("/books")
public class BookController2 {

    @Autowired
    private IBookService bookService;

    @GetMapping("/...")
    public List<Book> getAll(){
        return bookService.list();
    }
```

指定接收路径、接收参数的格式。

路径变量用 `@PathVariable` 接收，Json 变量用 `@RequestBody` 接收，并自动封装到对应的对象中。（如果参数差别过大，可能需要用DTO对象来进行中介，然后再用 BeanUtils 方法来克隆到实体对象）

返回的变量一般封装为 Result 对象传输到前端。

#### Service层

作为后端逻辑处理的核心，在 Controller 层调用以后，进行一定的加工，调用 Mapper 并把结果再返回给 Controller。

需要先定义一个接口，然后根据接口实现 Impl    类，后续直接 `@Autowired`即可自动调用。

#### Mapper层

处理数据库的枢纽，执行指定 SQL 。

复杂的 、动态SQL 语句可映射到 Mapper.xml 文件当中进行编写。

## 未完待续

- [ ] Aspect 切面类，公共字段填充
- [ ] 消息扩展器统一日期格式
- [ ] Redis + 自动缓存
- [ ] ……
