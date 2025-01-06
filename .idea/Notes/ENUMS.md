## Enums

An enum is a special "class" that represents a group of constants (unchangeable variables, like final variables).  Days of the week, Colors are some of the common examples of Enum.

To create an enum, use the enum keyword (instead of class or interface), and separate the constants with a comma. Note that they should be in uppercase letters:

```Java
enum TurtleName {  
   LEONARDO,  
   RAPHAEL,  
   DONATELLO,  
   MICHAELANGELO  
}
```


You can access enum constants with the dot syntax:

System.out.println(TurtleName.DONATELLO);

Enums are often used in switch statements to check for corresponding values,  as shown below:

```Java
  public static void main(String[] args) {

        TurtleName turtleName = TurtleName.DONATELLO;
        
        switch(turtleName) {
            case LEONARDO:
                System.out.println("Radical Leonardo");
                break;
            case RAPHAEL:
                System.out.println("Cowabunga Raphael");
                break;
            case DONATELLO:
                System.out.println("Bodacious Donatello");
                break;
            case MICHAELANGELO:
                System.out.println("Gnarly Michaelangelo");
                break;
        }

    }
```


### How to Use valueOf() Method of Enum Java Class?

Java provides a valueOf(String) method for all enum types, through which we can create an enum object from String.

```Java
enum Colour {
   RED, GREEN, BLUE, PINK, YELLOW;
}
```

```Java
enum Colour {
    RED, GREEN, BLUE, PINK, YELLOW;
}

public class Main {
    public static void main(String[] args) {

        Colour colour = Colour.valueOf("RED");
        System.out.println(colour); // Output will be RED

    }
}
```



 It throws IllegalArgumentException if the specified enum type has no constant with the specified name, or the specified class object does not represent an enum type

```Java
enum Colour {
    RED, GREEN, BLUE, PINK, YELLOW;
}

public class Main {
    public static void main(String[] args) {

        Colour colour = Colour.valueOf("PURPLE");
    }
}

```

Output

```
Exception in thread "main" java.lang.IllegalArgumentException: No enum constant Colour.PURPLE
```

### Comparing ENUMs

When it comes to comparing enum constants, you have two options: using the “==” operator or the “equals” method to check the equality of the enums, they both do the same thing although   the “==” operator is generally considered to be a better approach to check the enums:

```Java
public class Main {
    public static void main(String[] args) {

        Colour myColour = Colour.YELLOW;

        if (myColour == Colour.YELLOW) {
            System.out.println("THEY CALL IT MELLOW YELLOW");
        }

        if (myColour.equals(Colour.YELLOW)) {
            System.out.println("WE ALL LIVE IN A YELLOW SUBMARINE");
        }

    }
}


```


### Comparing an enum with a string

Directly comparing enum value with a string won't work as they both will be of different types.

```Java
enum Colour {
    RED, GREEN, BLUE, PINK, YELLOW;
}

public class Main {
    public static void main(String[] args) {
        Colour favColour = Colour.PINK;

//        This won't output anything
        if (favColour.equals("PINK")) {
            System.out.println("Your fav colour is PINK");
        }

    }

}
```

For comparing String to Enum type you should convert enum to string and then compare them. For that you can use toString() method or name() method. They are both different ways of achieving the same thing

```Java
   public static void main(String[] args) {
        Colour favColour = Colour.PINK;
        if (favColour.name().equals("PINK")){
            System.out.println("Your fav colour is PINK");
        }

        if (favColour.toString().equals("PINK")){
            System.out.println("Your fav colour is PINK");
        }

    }
```

The main difference between name() and toString() is that name() is a final method, so it cannot be overridden. The toString() method returns the same value that name() does by default, but toString() can be overridden by subclasses of Enum.

In short both achieve the same thing, but toString can be overridden

###  How to Create Enum with Multiple Values

The syntax to create an enum with multiple values is very similar to the syntax of enum with a single value assigned to it. we should do the following steps to have an enum with different values:

* Create enum constructor which accepts multiple values  
* Assign each constructor argument to a member field in the enum definition  
* Create getter methods so we can access any of the values assigned to a particular enum constant  
* Create a reverse lookup method so we can get the enum constant from any given enum value assigned to it

```Java
enum WeekDays {

    SUNDAY("Sunday Funday", true),
    MONDAY("Moany Monday", false),
    TUESDAY("Terrible Tuesday", false),
    WEDNESDAY("Wallow in self pity Wednesday", false),
    THURSDAY("Thirsty Thursday", false),
    FRIDAY("Fantastic Friday", false),
    SATURDAY("Sassy Saturday", true);
    
    private String daysGreeting;
    private boolean isWeekend;


    //    Constructor
    WeekDays(final String daysGreeting, final boolean isWeekend) {
        this.daysGreeting = daysGreeting;
        this.isWeekend = isWeekend;
    }


    //    Getter for greeting
    public String getDaysGreeting() {
        return daysGreeting;
    }


    //    Getter for isWeekend boolean
    public boolean isWeekend() {
        return isWeekend;
    }
}

```

```Java
public class Main {
    public static void main(String[] args) {
        String today = "Thursday";

        //Find an enum which matches our string
        WeekDays todayAsEnum = WeekDays.valueOf(today.toUpperCase());

        //Print out today's greeting
        System.out.println(todayAsEnum.getDaysGreeting());

        if (todayAsEnum.isWeekend()) {
            System.out.println("Woo hoo");
        } else {
            System.out.println("Boo! work to do");
        }

    }
}
```