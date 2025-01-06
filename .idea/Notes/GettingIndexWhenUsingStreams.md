We can use an IntStream to generate a sequence of numbers.

```Java
//prints  1 through 9
IntStream.range(1, 10).forEach(System.out::println); 
```

We can also use an IntStream to iterate through a list and access each element's index.

```Java
        List<String> names = Arrays.asList("Edward", "Katie", "Mark", "Jack");

//        Prints Edward is at 0, Katie is at 1, etc
        IntStream.range(0, names.size())
        .forEach(i -> {
        System.out.println(names.get(i) + " is at " + i);
        });
```

We can use IntStream with mapToObj to transform elements while also have access to index information.

```Java
        List<String> names = Arrays.asList("Edward", "Katie", "Mark", "Jack");

        // Transform every other name to upperCase
        List<String> transformedNames = IntStream.range(0, names.size())
                .mapToObj(index ->
                        index % 2 == 0 ? names.get(index).toUpperCase() : names.get(index)
                )
                .toList();


        // Output: [EDWARD, Katie, MARK, Jack]
        System.out.println(transformedNames);
```

