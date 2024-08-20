To help us understand generics let's create a class called SuperList which we will use to store integers.

```Java
class SuperList {
    private int [] items = new int[10];
    private int count;

    public void add(int item) {
        items[count++] = item;
    }

    public int get(int index) {
        return items[index];
    }

}
```

We can instantiate this class in our main method and use the add and the get method to add and get items from our array of integers.

```Java
public class Main {
    public static void main(String[] args) {

        var mySuperList = new SuperList();

        mySuperList.add(88);
        mySuperList.add(99);
        System.out.println(mySuperList.get(1));

    }
}
```
