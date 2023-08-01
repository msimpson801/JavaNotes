## Compleatable Future - Simple exmaple

CompletableFuture is used for asynchronous programming in Java. Asynchronous programming is a means of writing non-blocking code by running a task on a separate thread than the main application thread and notifying the main thread about its progress, completion or failure.
This way, your main thread does not block/wait for the completion of the task and it can execute other tasks in parallel.

Below is a simple example where we order a pizza, drink some beers while we wait however long it takes the pizza to arrive.

```Java
public class Main {
    public static void main(String[] args) {
        
        System.out.println("Order a pizza");

        //Create a completable future for a task that returns a string
        CompletableFuture <String> orderPizza = CompletableFuture.supplyAsync(() -> {
            //Simulate a time consuming task
            try {
                Thread.sleep(2000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

           return "pepperoni pizza";
        });

        //Doing something else while awaiting the task completing
        System.out.println("Drink beers");

        //Get the result from the completable future when it's done i.e. the pizza
        orderPizza.thenAccept(pizza ->{
            System.out.println("Your  " + pizza + " has arrived. Bon appetit");
        });


        //Keep the main thread running until the completable future is done
        try {
            Thread.sleep(3000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
    }
}
```

Output

```
Order a pizza
Drink beers
Your  pepperoni pizza has arrived. Bon appetit
```
