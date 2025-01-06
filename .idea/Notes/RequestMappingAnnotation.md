### Using `@RequestMapping` to Create a Base URL Path for All Endpoints

Let me show you how to use `@RequestMapping` to create a base URL path for all endpoints within a controller. Here's how you can set up a base path for all endpoints in a controller:

```java
@RestController
@RequestMapping("/countries")  // Base path for all endpoints in this controller
public class CountryController {
    
    @GetMapping("/all")        // Will map to /countries/all
    public List<Country> getAllCountries() {
        // ... implementation
    }
    
    @GetMapping("/{name}")     // Will map to /countries/{name}
    public Country getCountryByName(@PathVariable String name) {
        // ... implementation
    }
    
    @PostMapping              // Will map to /countries
    public Country addCountry(@RequestBody Country country) {
        // ... implementation
    }
}
````

### Key Points to Understand:

- The `@RequestMapping("/countries")` at the class level sets the base path.
- All method-level mappings are appended to this base path.
- You can also use more specific annotations like `@GetMapping`, `@PostMapping`, etc., which will still respect the base path.

