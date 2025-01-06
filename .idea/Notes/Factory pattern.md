## Factory pattern

A factory pattern is a design pattern in programming which helps us create objects in a more organized and flexible way,where the end-user get's to decide what kind of object they want without actually worrying about creating them.

Usually object creation in Java takes place like so

```Java
        SomeClass someClass = new SomeClass();
```

When we use the word new you areinstantiating a concrete class.

When you have a lot of related concrete classes you often have to write code like this.

```Java
        if (havingPicnic) {
        return new ActualRealDuck();
        } else if (hunting) {
        return new  DecoyDuck();
        } else if (inBath) {
        return new RubberDuck();
        }
```

Here we’ve got several concrete classes being instantiated, and the decision of which to instantiate is made at runtime depending on some set of conditions.
There is no issue with the new keyword, but the issue is how to tackle the change down the road.

To understand the problem better let's look at an example.


Without Factory Pattern
Imagine you have a pizza store that creates different types of pizzas like CheesePizza, PepperoniPizza, and VeggiePizza. Without the factory pattern, your code might look something like this:

```Java
public class Pizzeria {
 public Pizza orderPizza(String type) {
  Pizza pizza;


  //Based on the type of pizza we instantiate the correct concrete class
  if (type.equals("cheese")) {
   pizza = new CheesePizza();
  } else if (type.equals("pepperoni")) {
   pizza = new PepperoniPizza();
  } else if (type.equals("veggie")) {
   pizza = new VeggiePizza();
  } else {
   throw new IllegalArgumentException("Unknown pizza type");
  }

  //Once we have the pizza we prepare it, cut it and then box it
  //This is what we expect to stay the same. 
  // For the most part, preparing, baking and cutting a pizza has remained the same for years and years
  pizza.prepare();
  pizza.bake();
  pizza.cut();
  pizza.box();

  return pizza;
 }
}
```


Now let's imagine that your competitors have recently added exciting new pizzas to their menus: the Marshmallow Pizza and the Kiwi Pizza. Obviously you need to keep up with the competition, so you’ll add these items to your menu. And you haven’t been selling many Pepperoni Pizzas lately, so you decide to take that off the menu.

- The problem with this is that the “orderPizza” code is **NOT closed for modification**. If the Pizza restaurant  changes its pizza offerings (add or remove any new pizza), we have to get into the “orderPizza” code and modify it over and over. As the number of pizza types grows, the orderPizza method becomes harder to maintain.

- We don’t expect the preparation process (prepare, bake, cut, box etc) to change. So we don’t expect that code to change, just the pizza it operates on.

We know we’d be better off moving the object creation out of the orderPizza() method. But how? Well, what we’re going to do is take the creation code and move it out into a Pizza factory which will only be concerned with creating pizzas. 

With Factory Pattern
Now, let’s refactor the code using the factory pattern:

Define the Pizza Interface:
Java

```Java
public interface Pizza {
    void prepare();
    void bake();
    void cut();
    void box();
}
```

Create Concrete Pizza Classes:

```Java 
public class CheesePizza implements Pizza {
 @Override
 public void prepare() { System.out.println("Preparing Cheese Pizza"); }
 @Override
 public void bake() { System.out.println("Baking Cheese Pizza"); }
 @Override
 public void cut() { System.out.println("Cutting Cheese Pizza"); }
 @Override
 public void box() { System.out.println("Boxing Cheese Pizza"); }
}

public class PepperoniPizza implements Pizza {
 @Override
 public void prepare() { System.out.println("Preparing Pepperoni Pizza"); }
 @Override
 public void bake() { System.out.println("Baking Pepperoni Pizza"); }
 @Override
 public void cut() { System.out.println("Cutting Pepperoni Pizza"); }
 @Override
 public void box() { System.out.println("Boxing Pepperoni Pizza"); }
}

public class VeggiePizza implements Pizza {
 @Override
 public void prepare() { System.out.println("Preparing Veggie Pizza"); }
 @Override
 public void bake() { System.out.println("Baking Veggie Pizza"); }
 @Override
 public void cut() { System.out.println("Cutting Veggie Pizza"); }
 @Override
 public void box() { System.out.println("Boxing Veggie Pizza"); }
}

```
Create the Pizza Factory:

```Java
public class PizzaFactory {
    public static Pizza createPizza(String type) {
        switch (type.toLowerCase()) {
            case "cheese":
                return new CheesePizza();
            case "pepperoni":
                return new PepperoniPizza();
            case "veggie":
                return new VeggiePizza();
            default:
                throw new IllegalArgumentException("Unknown pizza type");
        }
    }
}

```

Refactor the Pizzeria Class

 Once we have a SimplePizzaFactory, our orderPizza() method just becomes a client of that object. Now instead of creating objects directly using a constructor, we use a factory method to produce objects.  
 Any time the Restaurant class needs a pizza it asks the pizza factory to make one. Gone are the days when the orderPizza() method needs to know about Pepperoni versus Vegetarian pizzas. 
 Now the orderPizza() method just cares that it gets a pizza that extends the base class Pizza so that it can call prepare(), bake(), cut(), and box().

```Java
public class Pizzeriae {
 public Pizza orderPizza(String type) {
  Pizza pizza = PizzaFactory.createPizza(type);

  pizza.prepare();
  pizza.bake();
  pizza.cut();
  pizza.box();

  return pizza;
 }
}

```