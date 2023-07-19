## What is a functional interface?

A functional interface is an interface with a single abstract method

**_NOTE:_** An abstract method is a method which does not have a body.

Below is an example of a functional interface.

```Java
interface Vehicle {

    //I am an abstract method
    //There is only one of me
    void cleanVehicle();

    //I am a default method, you can have more than one
    default void startVehicle() {
        System.out.println("Vehicle is starting");
    }
}
```

Now we can create a class which implements the Vehicle interface. Note that our class need to implement our abstract method clean vehicle

```Java
public class Car implements Vehicle {
    @Override
    public void cleanVehicle() {
        System.out.println("Cleaning the vehicle");
    }

    public static void main(String args[]){
        Car car = new Car();
        car.cleanVehicle();
        car.startVehicle();
    }
}
```

If we add the annotation @FunctionalInterface and then try to add an additional abstract method then we will get a compilation error. This is because functional interfaces can only have one abstract method. However an interface without this annotation can have many abstract methods