## **What are collectors?**

As the name suggests, the Collectors class is used to collect elements of a Stream into Collection. It has some useful methods  for aggregating elements of streams into various data structures like lists, sets, maps, etc. It's often used along with the collect() method of Stream class which accepts Collectors
What are collectors?

As the name suggests, the Collectors class is used to collect elements of a Stream into Collection. It has some useful methods  for aggregating elements of streams into various data structures like lists, sets, maps, etc. It's often used along with the collect() method of Stream class which accepts Collectors


### toSet

The toSet() method is used to collect a stream into a set.
If you collect a stream with duplicate elements into a set it will remove duplicates:


```Java
  public static void main(String[] args) {
        Set<String> uniqueNames = Stream.of(
                "Mark",
                "Edward",
                "Paul",
                "Edward"
        ).collect(Collectors.toSet());
        
        System.out.println(uniqueNames);

    }
```

Output

```
[Mark, Edward, Paul]
```


### toMap

We can use Collectors.toMap() function to collect the stream elements to a Map. This method accepts two arguments one for mapping a key and another for mapping the corresponding value in the Map.

Let’s transform our stream of foods into a Map. For the first example let's say we'd like to map our name of the food to their average quantity, that is create a <K, V> pair that has a <name, quantity> form.

```Java
   List<Food> foods = List.of(
                new Food("Carrot", "Vegetable", "Orange", 1),
                new Food("Lemon", "Fruit", "Yellow", 2),
                new Food("Blueberry", "Fruit", "Blue", 4),
                new Food("Pumpkin", "Vegetable", "Orange", 5)
        );


        Map<String, Integer> foodAndQuantity = foods
                .stream()
                .collect(
                        Collectors.toMap(Food::getName, Food::getQuantity)
                );


        System.out.println("Here is our map");
        System.out.println(foodAndQuantity);

        var noOfCarrots = foodAndQuantity.get("Carrot");
        System.out.println("\nUsing map to get quantity of carrots");
        System.out.println(noOfCarrots);

```

Output

```
Here is our map
{Carrot=1, Blueberry=4, Pumpkin=5, Lemon=2}

Using map to get quantity of carrots
1

```

### maxBy

The maxBy() method that is used to find the maximum element of the input elements using the passed comparator.

```Java
List<Food> foods = List.of(
                new Food("Carrot", "Vegetable", "Orange", 1),
                new Food("Lemon", "Fruit", "Yellow", 2),
                new Food("Blueberry", "Fruit", "Blue", 4),
                new Food("Pumpkin", "Vegetable", "Orange", 5)
        );

        Optional<Food> foodWithLargestQuantity = foods
                .stream()
                .collect(
                        Collectors.maxBy(Comparator.comparing(Food::getQuantity)
                        ));
        
        foodWithLargestQuantity.ifPresent(System.out::println);
```


N.B. We can achieve the same result use max as our terminal operation on the stream

```Java
        List<Food> foods = List.of(
        new Food("Carrot", "Vegetable", "Orange", 1),
        new Food("Lemon", "Fruit", "Yellow", 2),
        new Food("Blueberry", "Fruit", "Blue", 4),
        new Food("Pumpkin", "Vegetable", "Orange", 5)
);


Optional <Food> foodWithLargestQuantity = foods
        .stream()
        .max(Comparator.comparing(Food::getQuantity));


        System.out.println(foodWithLargestQuantity);

```

### partitioningBy

The partitioningBy collector is used to partition elements of a stream into two groups based on a given predicate.
Let’s imagine we have a list of fruits and vegetables and we want to partition our list into two categories: foods which are vegetables and foods which are not.


```Java
  List<Food> foods = List.of(
        new Food("Carrot", "Vegetable", "Orange"),
        new Food("Lemon", "Fruit", "Yellow"),
        new Food("Pumpkin", "Vegetable", "Orange")
);


Predicate<Food> isVegetable = food -> "Vegetable".equals(food.getType());


Map<Boolean,List <Food>> fruitAndVeg = foods
        .stream()
        .collect(Collectors.partitioningBy(isVegetable));

        System.out.println(fruitAndVeg);
```
Output
```
{false=[Food(name=Lemon, type=Fruit, colour=Yellow)], 
true=[Food(name=Carrot, type=Vegetable, colour=Orange), Food(name=Pumpkin, type=Vegetable, colour=Orange)]}
```
In this example:

The isVegetable predicate checks if the food category is “Vegetable”.
The partitioningBy method partitions the list into two groups: vegetables and non-vegetables.

### groupingBy

The groupingBy() method is used for grouping objects by some property and storing results in a Map instance. Let’s start with the simplest groupingBy method, which only takes one argument which is a function to classify/categorize our list of objects.

Let’s add blueberries to our list of foods and group each food by its colour.


```Java
 List<Food> foods = List.of(
        new Food("Carrot", "Vegetable", "Orange"),
        new Food("Lemon", "Fruit", "Yellow"),
        new Food("Pumpkin", "Vegetable", "Orange"),
        new Food("Blueberry", "Fruit", "Blue")
);

Function<Food, String> groupByColour = food -> food.getColour();

//Passing a grouping function
Map<String, List<Food>> foodsByColour = foods.stream()
        .collect(Collectors.groupingBy(groupByColour));


//Writing the same thing but using a method reference
Map<String, List<Food>> foodsByColourMap = foods
        .stream()
        .collect(Collectors.groupingBy(Food::getColour));

        System.out.println(foodsByColourMap);
```

This will give us the following output

```
{Blue=[Food(name=Blueberry, type=Fruit, colour=Blue)], Yellow=[Food(name=Lemon, type=Fruit, colour=Yellow)], Orange=[Food(name=Carrot, type=Vegetable, colour=Orange), Food(name=Pumpkin, type=Vegetable, colour=Orange)]}
```


We can get each individual category by calling the get method of the map

```Java
List <Food> blueFood = foodsByColour.get("Blue");
List <Food> orangeFood = foodsByColour.get("Orange");
List <Food> yellowFood = foodsByColour.get("Yellow");
```











