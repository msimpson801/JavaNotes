## Factory pattern

A factory pattern is a design pattern in programming which helps us create objects in a more organized and flexible way.

Imagine you have a pizza restaurant that produces different kinds of pizzas.  Let's say we have a Pizza class with several subclasses, such as Pepperoni and Hawaiian, each with its own unique ingredients and preparation methods.

```Java
class Pizza {
    private String name;

    public Pizza(String name) {
        this.name = name;
    }

    public void prepare() {
        System.out.println("Prepare");
    }

    public void bake () {
        System.out.println("Bake");
    }

    public void cut () {
        System.out.println("Cutting");
    }

    @Override
    public String toString() {
        return "Pizza{" +
                "name='" + name + '\'' +
                '}';
    }
}


class PepperoniPizza extends Pizza {
    public PepperoniPizza() {
        super("Pepperoni");
    }
}

class HawaiianPizza extends Pizza {
    public HawaiianPizza() {
        super("Hawaiian");
    }
    
}
```

Now let's create a restuarant class with a orderPizza method which _determines_ the appropriate type of pizza and then goes about _making_ the pizza:

```Java
class Restaurant {
    public Pizza orderPizza (String customerRequest) {
        Pizza customerPizza = null;

		//Based on the type of pizza we instantiate the correct concrete class
        if ("Pepperoni".equals(customerRequest)) {
            customerPizza =  new PepperoniPizza();
        }
        else if ("Hawaiian".equals(customerRequest)) {
             customerPizza = new HawaiianPizza();
        }

		//Once we have the pizza we prepare it, cut it and then box it
		//Each pizza sub type knows how to prepare itself
        customerPizza.prepare();
        customerPizza.bake();
        customerPizza.cut();

        return customerPizza;
    }
}
```


Let's imagine that your competitors have recently added exciting new pizzas to their menus: the Marshmallow Pizza and the Kiwi Pizza. Obviously you need to keep up with the competition, so you’ll add these items to your menu. And you haven’t been selling many Pepperoni Pizzas lately, so you decide to take that off the menu.

- The problem with this is that the “orderPizza” code is **NOT closed for modification**. If the Pizza restaurant  changes its pizza offerings (add or remove any new pizza), we have to get into the “orderPizza” code and modify it over and over

- We don’t expect the preparation process (prepare, bake, cut, box etc) to change. So we don’t expect that code to change, just the pizza it operates on.

We know we’d be better off moving the object creation out of the orderPizza() method. But how? Well, what we’re going to do is take the creation code and move it out into a Pizza factory which will only be concerned with creating pizzas.

```Java
class SimplePizzaFactory {
    public Pizza createPizza (String type){
        Pizza pizza = null;

        if ("Pepperoni".equals(type)) {
            pizza =  new PepperoniPizza();
        }
        else if ("Hawaiian".equals(type)) {
            pizza = new HawaiianPizza();
        }

        return pizza;
    }
}
```

Once we have a SimplePizzaFactory, our orderPizza() method just becomes a client of that object. Now instead of creating objects directly using a constructor, we use a factory method to produce objects.  Any time the Restaurant class needs a pizza it asks the pizza factory to make one. Gone are the days when the orderPizza() method needs to know about Pepperoni versus Kiwi pizzas. Now the orderPizza() method just cares that it gets a pizza that extends the base class Pizza so that it can call prepare(), bake(), cut(), and box().

```Java
class Restaurant {

    SimplePizzaFactory simplePizzaFactory;

    public Restaurant(SimplePizzaFactory simplePizzaFactory) {
        this.simplePizzaFactory = simplePizzaFactory;
    }

    public Pizza orderPizza (String customerRequest) {
        Pizza customerPizza = null;

        simplePizzaFactory.createPizza(customerRequest);

        customerPizza.prepare();
        customerPizza.bake();
        customerPizza.cut();


        return customerPizza;
    }
}
```