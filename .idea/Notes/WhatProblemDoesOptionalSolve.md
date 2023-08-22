### What problem does Optional solve

Let's create a REST API which returns data about famous musicians.
Let's create an endpoint which returns a musician based on their name.


```Java
  @GetMapping("/musician/{artistname}")
    public Musician getMusician(@PathVariable String artistname) {
       return musicianService.findByName(artistname);
    }
```



In our Service class we have a list of musicians and a method which finds musicians in our list based on their name.

```Java
@Service
public class MusicianService {

    List <Musician> musicians = Arrays.asList(
            Musician.builder()
                    .name("Prince")
                    .biggestHit("Purple Rain")
                    .build(),
            Musician.builder()
                    .name("Michael Jackson")
                    .biggestHit("Thriller")
                    .build()
    );
    

    public Musician findByName (String name){
        for (Musician musician: musicians) {
            if (name.equalsIgnoreCase(musician.getName())) {
                return musician;
            }
        }

        return null;

    }
}
```

If we run our API and go to the endpoint `http://localhost:8080/musician/prince` we will get the following JSON payload.

```JSON
{"name":"Prince","biggestHit":"Purple Rain"}
```

However, what would happen if were to search for a musician which is not on our list? If we go to `http://localhost:8080/musician/marksimpson` our API will return null, which is not ideal.

What if we were to modify our code to return one of the fields of the Musician object rather than the entire object.

```Java
  @GetMapping("/musician/{artistname}")
    public String getHitSong(@PathVariable String artistname) {
       Musician musician = musicianService.findByName(artistname);

       return musician.getBiggestHit();
    }
```

Now if we go to `http://localhost:8080/musician/prince` we will get the following

```
Purple Rain
```

However if we try to find the hit song of a musician that does not exist on our list then we will get an error. If we go to `http://localhost:8080/musician/marksimpson` we will get a 500 error.

We could change our code to handle a null scenario.

```Java
 @GetMapping("/musician/{artistname}")
    public String getHitSong(@PathVariable String artistname) {
       Musician musician = musicianService.findByName(artistname);

       if (musician == null) {
           return "No hit song";
       }

       return musician.getBiggestHit();
    }
```

The problem with our `findByName` method returning `null` is that the caller is not _forced_ into handling the `null` case,  whoever is writing the `getHitSong` method to must remember to handle null.

A better solution might be to change the `findByName` method in our service class to return an Optional object.

```Java
 public Optional <Musician> findByName (String name){
        for (Musician musician: musicians) {
            if (name.equalsIgnoreCase(musician.getName())) {
                return Optional.of(musician);
            }
        }

        return Optional.empty();

    }
```

An optional  can either contain a non-null value or it can contain no value at all. Because our method has been refactored to return an Optional it makes whoever calls it aware that it could potentially contain null and forces them to handle that appropriately

Now that our `findByName` method returns an Optional, we will refactor our `getHitSong` method.

```Java
 @GetMapping("/musician/{artistname}")
    public String getHitSong(@PathVariable String artistname) {
       Optional <Musician> optionalMusician = musicianService.findByName(artistname);

       return optionalMusician.map(Musician::getBiggestHit).orElse("No hit song");
    }
```
