
There are two ways to create a WebClient, the first using the create method which has two versions: either an empty argument to set up a default WebClient or one that takes in the base URL that this WebClient will call.

There’s also a more comprehensive builder way, which will allow you to set more defaults on the WebClient if you want

```Java

//Method one
WebClient webClient1 = WebClient.create(); //with empty URI 
WebClient webClient2 = WebClient.create("http://localhost:8081/")

//Method two using builder pattern
 WebClient webclient = WebClient
            .builder()
            .baseUrl("http://localhost:8081/")
            .build();
```

### Making calls with WebClient

Let’s look at how to make a basic call with the `webClient` we just created. I have created a film API with two endpoints.

The first endpoint gives us an array o all the films, at the moment there are just two.

http://localhost:8081/films

```JSON
[{"title":"The Godfather","director":"Francis Ford Copolla","rating":5},{"title":"Goodfellas","director":"Martin Scorsese","rating":5}]
```

The second endpoint will give you a single film based on the path parameter

Endpoint

http://localhost:8081/films/the_godfather

JSON returned

```JSON
{"title":"The Godfather","director":"Francis Ford Copolla","rating":5}
```

Endpoint

http://localhost:8081/films/goodfellas

JSON returned

```JSON
{"title":"Goodfellas","director":"Martin Scorsese","rating":5}
```


Now that we have a n API set up to return film data, let's create another to consume our film API.
To consume our film API we will use Spring Web Client

```Java
  WebClient webclient = WebClient
            .builder()
            .baseUrl("http://localhost:8081/")
            .build();
```

```Java
   public MovieModel getFilmByTitle(String title) {

        return webclient
                .get()
                .uri("film/" + title)
                .retrieve()
                .bodyToMono(MovieModel.class)
                .block();
    }

```

Let's look at each line to see what it does.

- `get()` method denote, you are making an **HTTP.GET** request. You can change it accordingly like `post()`, `put()`, `delete()` etc.
- - `.uri()` indicates what path we want appended to the base URL we set this WebClient up with earlier
-  The _retrieve()_ will actually make the call out  and retrieves the response body
- `bodyToMono(_YourPOJOClass_.class)` method maps the response of the API to the POJO class. In this case it’s expecting the call to return a JSON payload that is mappable to `MovieModel` class
- `block()`makes a synchronous call. This option blocks the thread until the end of execution


Mono represents 0 or 1 objects.
Flux represents any number of objects