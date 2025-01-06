
We''ll create an API which returns Movie data.

Let's set up an end point to retrieve Movies based on their title.

Here is the controller.

````java
@RestController
public class FilmController {

    private FilmService filmService;

    public FilmController(FilmService filmService) {
        this.filmService = filmService;
    }

    @GetMapping("film")
    public FilmModel getByTitle (@RequestParam String title) {
        return filmService.getFilmByTitle(title);
    }
}
````

Here is the service

```Java  
@Service
public class FilmService {
    private List <FilmModel> filmList = Stream.of(
            new FilmModel("The Godfather", "Francis Ford Copolla", 5),
            new FilmModel("Goodfellas", "Martin Scorsese", 5)
    ).collect(Collectors.toList());

    public FilmModel getFilmByTitle(String title) {

        return filmList.stream()
                .filter(film -> title.replace("_","").equalsIgnoreCase(film.getTitle()))
                .findFirst()
                .orElse(null);
    }
}
```

If start our application and send a GET request to:

http://localhost:8080/film?title=the_godfather

We will get a 200 response with the folllowing payload

```JSON
{
  "title": "The Godfather",
  "director": "Francis Ford Copolla",
  "rating": 5
}
```

However if we send a GET request to

http://localhost:8080/film?title=dave_the_rave

We get the **Status: 200 OK** and **empty body** which is a successful response even though the resource does not exist. But it is not the proper response when a resource does not exist.

Let's create a FilmNotFound exception to throw.

```Java
public class FilmNotFoundException extends RuntimeException{

    public FilmNotFoundException (String message) {
        super(message);
    }
}
```

Now let's throw our exception.

```Java
public FilmModel getFilmByTitle(String title) {  
  
return filmList.stream()  
.filter(film -> title.replace("_", " ").equalsIgnoreCase(film.getTitle()))  
.findFirst()  
.orElseThrow(() -> new FilmNotFoundException("No film of this name found"));  
}
```

Now when we hit the endpoint with a movie title which does not exist we will get a 500 error.
But the Status: 500 Internal Server Error is not the appropriate response for the resource not found. So, we will add an annotation **@ResponseStatus** to generate the Status: 404 Not Found.

![[Pasted image 20230515090415.png]]

```Java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class FilmNotFoundException extends RuntimeException{

    public FilmNotFoundException (String message) {
        super(message);
    }
}

```


#SpringBoot #ExceptionHandling