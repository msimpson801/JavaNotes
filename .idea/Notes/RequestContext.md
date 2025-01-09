# Using Custom Objects in Spring Boot Controllers: A Clean Approach to Header Handling

When a web request has headers in Spring Boot, you typically extract them using the `@RequestHeader` annotation within your controller methods. However, there are times when you need to access these headers across multiple endpoints, and repeatedly extracting them can lead to duplicated code

```Java
public class HelloWorldController {  
  
    @GetMapping("/helloworld")  
    public String helloWorld (@RequestHeader("Planet") String planet) {  
        if (planet.equals("Earth")) {  
            return "Hello World";  
        }  
  
        return "Hello from another world";  
    }  
}
```

But what if you have multiple headers to deal with or the same headers in various controllers? Manually adding annotations  would  be tedious.

```Java
@RestController  
public class MarioGameController {  
  
    @GetMapping("startMarioGame")  
    public String startMarioGame(@RequestHeader("characterName") String characterName,  
                                 @RequestHeader("noOfLives") String noOfLives) {  
  
        return "Let's go " + characterName + " you have " + noOfLives + " lives";  
    }  
}


@RestController  
public class PokemonGameController {  
  
    @GetMapping("/startPokemonGame")  
    public String startPokemonGame(@RequestHeader("characterName") String characterName,  
                                   @RequestHeader("noOfLives") String noOfLives) {  
  
        return "Trainer " + characterName + ", you're ready to catch 'em all! You have " + noOfLives + " Pokémon ready for battle.";  
    }  
}
```



The solution is to use a custom `RequestContextArgumentsResolver` to manage headers more efficiently. By implementing the `HandlerMethodArgumentResolver`, you can create a clean and reusable way to handle multiple headers across different controllers.


### Creating a Custom Object and Resolver

First, define a custom object to encapsulate the header information.

```Java
@NoArgsConstructor  
@AllArgsConstructor  
@Data  
public class GameContext {  
    private String characterName;  
    private String noOfLives;  
}
```

### Next, Implement the `HandlerMethodArgumentResolver`

Now, let’s implement the `HandlerMethodArgumentResolver` to populate our custom object.
; }`


``` Java
@Component
public class GameContextArgumentResolver implements HandlerMethodArgumentResolver {

    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        return parameter.getParameterType().equals(GameContext.class);
    }

    @Override
    public Object resolveArgument(MethodParameter parameter, 
                                  ModelAndViewContainer mavContainer,
                                  NativeWebRequest webRequest, 
                                  WebDataBinderFactory binderFactory) {
        String characterName = webRequest.getHeader("characterName");
        String noOfLives = webRequest.getHeader("noOfLives");
        return new GameContext(characterName, noOfLives);
    }
}
```

The `supportsParameter` method checks if the parameter type is `GameContext` and returns `true` if it is, indicating that the resolver can handle it.

The `resolveArgument` method retrieves the values of the specified headers (`characterName` and `noOfLives`) from the web request and creates a `GameContext` object with those values, which is then passed to the controller method.
### Register the Resolver

After defining the custom resolver, register it in your `WebMvcConfigurer` implementation; otherwise, Spring won't know about your custom resolver and it won't be used to handle the parameters in your controllers.

``` Java
@Configuration
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addArgumentResolvers(List<HandlerMethodArgumentResolver> resolvers) {
        resolvers.add(new GameContextArgumentResolver());
    }
}
```

### Using the Custom Object in Controllers

Finally, update your controllers to use the custom `GameContext` object.

#### Mario Game Controller

``` Java
@RestController  
public class MarioGameController {  
  
    @GetMapping("/startMarioGame")  
    public String startMarioGame(GameContext gameContext) {  
        return "Let's go " + gameContext.getCharacterName() + "! You have " + gameContext.getNoOfLives() + " lives.";  
    }  
}
```

#### Pokémon Game Controller


```Java
@RestController  
public class PokemonGameController {  
  
    @GetMapping("/startPokemonGame")  
    public String startPokemonGame(GameContext gameContext) {  
        return "Trainer " + gameContext.getCharacterName() + ", you're ready to catch 'em all! You have " + gameContext.getNoOfLives() + " Pokémon ready for battle.";  
    }  
}
```

### Conclusion

Using a custom `RequestContextArgumentsResolver` and `GameContext` object helps us handle headers in a smart way across different Spring Boot endpoints. Instead of repeatedly writing header extraction code, we create one central place to manage all our headers. This makes our code cleaner, easier to maintain, and less prone to error