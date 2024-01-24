```xml
<!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-validation -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-validation</artifactId>
    <version>3.2.0</version>
</dependency>

```


`javax.validation.constraints` 包中包含了一些常用的校验注解，用于在 Java Bean 实体类中进行参数校验。这些注解可以用于限制属性的取值范围、长度、格式等，以确保数据的有效性。以下是一些常见的校验注解：

1. **@NotNull：**
    - 用于检查被注解的元素（字段、方法参数等）不能为 null。
   ```java
   public class ExampleEntity {
       @NotNull
       private String name;
   }
   ```

2. **@NotEmpty：**
    - 用于检查被注解的字符串元素不能为空（不为 null 且长度大于 0）。
   ```java
   public class ExampleEntity {
       @NotEmpty
       private String username;
   }
   ```

3. **@NotBlank：**
    - 用于检查被注解的字符串元素不能为空白（不为 null 且去掉前后空格后长度大于 0）。
   ```java
   public class ExampleEntity {
       @NotBlank
       private String email;
   }
   ```

4. **@Min 和 @Max：**
    - 用于检查数字元素的最小值和最大值。
   ```java
   public class ExampleEntity {
       @Min(18)
       private int age;

       @Max(100)
       private int score;
   }
   ```

5. **@Size：**
    - 用于检查集合或数组的大小。
   ```java
   public class ExampleEntity {
       @Size(min = 1, max = 10)
       private List<String> hobbies;
   }
   ```

6. **@Pattern：**
    - 用于检查字符串元素是否符合指定的正则表达式。
   ```java
   public class ExampleEntity {
       @Pattern(regexp = "[a-zA-Z0-9]+")
       private String code;
   }
   ```

7. **@Email：**
    - 用于检查字符串元素是否符合电子邮件地址格式。
   ```java
   public class ExampleEntity {
       @Email
       private String email;
   }
   ```

这些注解可以直接在实体类的字段上使用，也可以在方法参数上使用。在 Spring MVC 中，可以在控制器方法的参数上使用这些注解来进行请求参数的校验。例如：

```java
@RestController
@RequestMapping("/example")
public class ExampleController {

    @PostMapping("/create")
    public ResponseEntity<String> createExample(@Valid @RequestBody ExampleEntity exampleEntity) {
        // 处理请求逻辑
        return ResponseEntity.ok("Entity created successfully");
    }
}
```

在上述例子中，`@Valid` 注解用于告诉 Spring 对 `ExampleEntity` 进行校验。如果请求的参数不满足校验条件，将会触发 `MethodArgumentNotValidException` 异常。您可以通过处理异常来返回相应的错误信息。在实际应用中，通常会结合使用 `BindingResult` 对象来获取校验结果。