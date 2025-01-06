### @Configuration and @Bean:
The @Configuration and @Bean annotations in Spring Boot are used together to define custom configurations and create Spring beans explicitly.
@Configuration annotation is used in Spring Boot to indicate that a class contains one or more bean definitions.

### @Configuration Annotation:
The @Configuration annotation is used to indicate that a class contains one or more bean definitions and should be processed by the Spring container to generate Spring beans

**NB**: _A Spring bean is just a simple Java object managed by the Spring framework._


To understand let's create a coolBean class, we've created a CoolBean class using Lombok annotations to simplify boilerplate code

```Java
@AllArgsConstructor
@NoArgsConstructor
@Data
@Builder
public class CoolBean {
    private String typeOfBean;
    private String colour;
}
```

Now we have created a class. We will then use  @Configuration and @Beans to different types of beans

```Java
@Configuration
public class BeanConfigurations {

    @Bean
    public CoolBean bakedBean() {
        return CoolBean.builder()
                .typeOfBean("Baked")
                .colour("Orange")
                .build();
    }

    @Bean
    public CoolBean kidneyBean() {
        return CoolBean.builder()
                .typeOfBean("Kidney")
                .colour("purple")
                .build();
    }

    @Bean
    public CoolBean coffeeBean() {
        return CoolBean.builder()
                .typeOfBean("Coffee")
                .colour("brown")
                .build();
    }
}
```

Now that we have created our beans, we can use them in our controller using dependency injection. 
To inject our beans into our controller, we will use Lombok's @RequiredArgsConstructor, which automatically generates a constructor with required arguments (i.e., final fields). 
This approach utilizes constructor-based dependency injection, ensuring that our beans are properly injected and available for use within the controller.

Behind the scenes, Spring Boot scans for @Configuration classes and @Bean annotations, wires the beans together, and manages their lifecycle, ensuring that they are available and properly injected wherever needed in your application.

```Java
@RestController
@RequiredArgsConstructor
public class SupermarketController {
    private final CoolBean bakedBean;
    private final CoolBean kidneyBean;
    private final CoolBean coffeeBean;

    @GetMapping("/beans")
    public List <CoolBean> allBeans () {
        return List.of(bakedBean, kidneyBean, coffeeBean);
    }

}

```

T   


