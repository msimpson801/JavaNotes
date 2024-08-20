
##  map and flatMap

#### .map 
`Map` transforms the value contained in an **Optional**, if present, and returns a new **Optional** of the transformed value. If it is not present, then it returns an empty **Optional**.

```Java
Optional <String> boxWithGift = Optional.of("Roller skates");  
Optional <String> boxWithNothing = Optional.ofNullable(null);  
  
System.out.println("When map finds a value it transforms it: " + boxWithGift.map(String::toUpperCase));

System.out.println("When an optional is empty, map does nothing: " + boxWithNothing.map(String::toUpperCase));
```

Output

```
When map finds a value it transforms it: Optional[ROLLER SKATES]

When an optional is empty, map does nothing: Optional.empty
```

#### .flatMap

In the case when we have nested **Optionals**, we can use the **flatMap()** method to flatten the result.
To help us understand let's first create an optional within an optional.

```Java
Optional <String> boxWithSurprise = Optional.of("Suprise");  
Optional<Optional<String>> boxWithAnotherBoxInside = Optional.of(boxWithSurprise);  
```

If we were to use a map on this nested optional it would return an optional within an optional.

```Java
Optional <String> boxWithSurprise = Optional.of("Suprise");  
Optional<Optional<String>> boxWithAnotherBoxInside = Optional.of(boxWithSurprise);  
  
System.out.println("Nested optional using just map : " + boxWithAnotherBoxInside.map(box -> box.map(String::toUpperCase)));
```

Output 

```
Nested optional using just map : Optional[Optional[SUPRISE]]
```

However if we use `flatMap` it will just return a single optional

```Java
Optional <String> boxWithSurprise = Optional.of("Suprise");  
Optional<Optional<String>> boxWitAnotherBoxInside = Optional.of(boxWithSurprise);  
  
System.out.println("Nested optional using just map : " + boxWitAnotherBoxInside.flatMap(box -> box.map(String::toUpperCase)));
```

Output

```
Optional using flatMap: Optional[SUPRISE]
```


**_NOTE:_**  Like `map` the `flatMap` method will return an empty optional if there is nothing there

```Java
Optional <String> emptyInnerBox = Optional.ofNullable(null);  
Optional<Optional<String>> emptyOuterBox = Optional.of(emptyInnerBox);  
  
System.out.println("Empty inner optional returns: " + emptyOuterBox.flatMap(box -> box.map(String::toUpperCase)));
```

Output

```
Empty inner optional returns: Optional.empty
```


##  orElse  orElseGet

#### .orElse 

`orElse` is used to get the value inside of an optional container. In the case that there is a value it will retrieve it. In the case that there is not a value it will return the default value contained within the `orElse`

Example 1 - Optional with something in it

```Java
//This time there is something inside the optional
String surprise = "Rollerblades";  
  
Optional <String> boxWithSurprise = Optional.ofNullable(surprise);  
  
String surpriseInsideBox = boxWithSurprise.orElse("Unlucky, there is no surprise");  
  
System.out.println("My surprise is: " + surpriseInsideBox);
```

Output

```
My surprise is: Rollerblades
```

Example 2: Optional with nothing in it

```Java
//This time there will be nothing inside the optional
String surprise = null;  
  
Optional <String> boxWithSurprise = Optional.ofNullable(surprise);  
  
String surpriseInsideBox = boxWithSurprise.orElse("Unlucky, there is no surprise");  
  
System.out.println("My surprise is: " + surpriseInsideBox);
```

Output

```
My surprise is: Unlucky, there is no surprise
```

#### .orElseGet

The _orElseGet()_ method is similar to _orElse()_. However, instead of taking a value to return if the _Optional_ value is not present, it takes a supplier function

```Java
String surprise = null;  
  
Optional <String> boxWithSurprise = Optional.ofNullable(surprise);  
  
String surpriseInsideBox = boxWithSurprise.orElseGet(() -> "Unlucky, there is no surprise");  
  
System.out.println("My surprise is: " + surpriseInsideBox);
```