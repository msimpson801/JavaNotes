The following generic method In the following  restricts the type parameter to the sub classes of the Number classes.

```Java
  public static <T extends Number> void displayNumbersOnly(T ele) {
        System.out.println("Number is: "+ ele);
    }
```

Because  _Integer_ and _Double_  and _Long_ are subtypes of _Number_ class we can use pass them as arguments to this method.

```Java
  public static void main(String[] args) {
        int num1 = 1;
        double num2 = 2;
        float num3 = 3;

        displayNumbersOnly(num1);
        displayNumbersOnly(num2);
        displayNumbersOnly(num3);
    }
```

However if we try to pass a String to this method we will get an exception

```
method displayNumbersOnly in class org.example.Main cannot be applied to given types;
```