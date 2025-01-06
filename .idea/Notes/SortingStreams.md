## **Sorted**

The Stream.sorted() is the method of the Stream interface that returns a sorted stream

If we use sorted on a list of letters or integers they will be sorted according to their natural order. 

For letters (characters or strings): They are sorted alphabetically. For example, the list ["b", "a", "c"] would be sorted to ["a", "b", "c"].
For integers: They are sorted numerically in ascending order. For example, the list [3, 1, 2] would be sorted to [1, 2, 3].

```Java
        List<String> listOfLetters = List.of("c", "a", "b");
        List<Integer> listOfNums = List.of(1, 3, 2);


        //sorted a,b,c
        var sortedNums = listOfNums.stream().sorted().toList();


        //sorted 1,2,3
        var sortedLetters = listOfLetters.stream().sorted().toList();
```

## **Using a comparator with sorted** 

If you want to compare a list of user defined objects we can define our own comparator and pass it as an argument to our sorted method.
A comparator is an interface that allows you to specify how objects should be compared and sorted.
Below we create a comparator to compare the capital field of the country object.

```Java
   List<Country> countries = List.of(
                new Country("Ireland", "Dublin", 5000000),
                new Country("Greece", "Athens", 10000000),
                new Country("Brazil", "Brasilia", 214000000),
                new Country("Egypt", "Cairo", 109000000)
        );


//        Here we define a comparator to sort by capital -  Athens, Brasilia, Cairo, Dublin
        Comparator<Country> compareCapitals = Comparator.comparing(Country::getCapital);

```
Below we create a comparator to compare the population field of the country object.

```Java
     List <Country> countries = List.of(
        new Country("Ireland", "Dublin", 5000000),
        new Country("Greece", "Athens", 10000000),
        new Country("Brazil", "Brasilia", 214000000),
        new Country("Egypt", "Cairo", 109000000)
);


Comparator <Country> comparePopulation = Comparator.comparingInt(Country::getPopulation);
```

Alternatively we could write the comparator as a lambda function

```Java
    List <Country> sortedCountries = countries
        .stream()
        .sorted((c1, c2) -> c1.getCapital().compareTo(c2.getCapital()))
        .toList();
```


A slightly different syntax using a method reference

```Java
    List <Country> sortedCountries = countries
        .stream()
        .sorted(Comparator.comparing(Country::getCapital))
        .toList();
```




Above we sorted the capital in alphabetical order from a-z, if we want reverse order we simply switch c2 with c1

```Java
    List <Country> sortedCountries = countries
        .stream()
        .sorted((c1, c2) -> c2.getCapital().compareTo(c1.getCapital()))
        .toList();
```


If we want to compare integers we can use compare int

```Java
    List <Country> sortedCountries = countries
        .stream()
        .sorted(Comparator.comparingInt(Country::getPopulation))
        .toList();
```


By default it will sort the integers from smallest to largest. Ireland would be first on the list as it has the smallest population and Brazil last. If we want to have the largest first we can do the following.

```Java
    List <Country> sortedCountries = countries
        .stream()
        .sorted(Comparator.comparingInt(Country::getPopulation).reversed())
        .toList();
```


Alternatively we could do this

```Java
    List <Country> sortedCountries = countries
    .stream()
    .sorted((c1, c2) -> c2.getPopulation() - c1.getPopulation())
    .toList();
```


## **Comparing multiple fields**

You can use a comparator to compare multiple fields of objects. In Java, you can achieve this by chaining comparators using the thenComparing method
To understand this better let's create a short list of football players two of which share the same name Ronaldo

```Java

        List<FootballPlayer> footballPlayers = List.of(
                new FootballPlayer("Messi", "Argentina"),
                new FootballPlayer("Ronaldo", "Portugal"),
                new FootballPlayer("Ronaldo", "Brazil")
        );

        Comparator<FootballPlayer> compareBySurname = Comparator
                .comparing(FootballPlayer::getSurname);

        footballPlayers.stream()
                .sorted(compareBySurname)
                .forEach(System.out::println);
        
```

After sorting our players by name, Messi will be first because M comes before R, the Portguese Ronaldo will be first because he is the first Ronaldo on our list, the Brazillian Ronaldo will be last.

Let’s say we want to sort our player first by their surname and then by their nationality if their surnames are the same, then we can use .thenComparing

```Java
     Comparator<FootballPlayer> compareBySurname = Comparator
                .comparing(FootballPlayer::getSurname)
                .thenComparing(FootballPlayer::getNationality);
```

Output

Given the list of players:

Messi, Argentina
Ronaldo, Portugal
Ronaldo, Brazil
The sorted order will be:

Messi, Argentina (sorted by surname “Messi”)
Ronaldo, Brazil (sorted by surname “Ronaldo” and then by nationality “Brazil”)
Ronaldo, Portugal (sorted by surname “Ronaldo” and then by nationality “Portugal”)

