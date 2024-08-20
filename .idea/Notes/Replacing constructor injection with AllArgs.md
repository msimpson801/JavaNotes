In constructor based injection the dependencies required for the class are required as args for the constructor. Below we inject the film service into the film controller constructor

```Java
@RestController
public class FilmController {

    private final FilmService filmService;

    public FilmController(FilmService filmService) {
        this.filmService = filmService;
    }

    @GetMapping("film")
    public FilmModel getByTitle (@RequestParam String title) {
        return filmService.getFilmByTitle(title);
    }
}
```

We can replace this with the @RequiredArgs constructor annotation at the top of the class.

```Java
@RestController
@RequiredArgsConstructor
public class FilmController {

    private final FilmService filmService;

    @GetMapping("film")
    public FilmModel getByTitle (@RequestParam String title) {
        return filmService.getFilmByTitle(title);
    }
}

```

**_@RequiredArgsConstructor_ generates a constructor requiring an argument for the _final_ and _@NonNull_ field**




