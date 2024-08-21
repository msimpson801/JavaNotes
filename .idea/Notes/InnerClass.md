## **Inner Classes**

In Java you can define a class within a class.  Such a class is known as a nested class.
The purpose of nested classes is to group classes that belong together, which makes your code more readable and maintainable.

A nested class which is not static is known as an inner class. Let’s take a look at an example of an inner class.Suppose we have two classes: a Musician class and an Album class. Since we can’t have an album without a Musician to make the album,then it makes sense to use inner classes to group these two classes together.

```Java
//outer class
class Musician {
   private String musicianName;
   
   public Musician(String musicianName) {
       this.musicianName = musicianName;
   }
   
   // inner class
   class Album {
       private String albumName;
       
       public Album(String albumName) {
           this.albumName = albumName;
       }
       
   }
   
}
```

Since the inner class exists within the outer class, you must instantiate the outer class first, in order to instantiate the inner class.  

```Java
//first create object of Outer class Musician
Musician musician = new Musician("Michael Jackson");

// create an object of inner class Album using outer class
Musician.Album album =  musician.new Album("Thriller");
```

*N.B.  We are unable to create an instance of the inner class without creating an instance of its given outer class

One advantage of inner classes, is that they can access attributes and methods of the outer class:

```Java
class Musician {
   private String musicianName;
   
   public Musician(String musicianName) {
       this.musicianName = musicianName;
   }
   
   // inner class
   class Album {
       private String albumName;


       public Album(String albumName) {
           this.albumName = albumName;
       }
       
       //getAlbumInfo can access variables in inner and outer class
       public String getAlbumInfo() {
           return "Album: " + albumName + " Artist: " + musicianName;
       }
   }

}

```

## Static nested classes

An instance of a static nested class can be created without the instance of the outer class.


```Java
ParentClass.NestedStaticClass nestedstatic = new ParentClass.NestedStaticClass();

//creating an object of static nested
// class without creating an object of the outer class.
Musician.Album album = new Musician.Album("Thriller");
```

Unlike inner class, a static nested class cannot access the member variables of the outer class.

```Java
//outer class
class Musician {
   private String musicianName;


   public Musician(String musicianName) {
       this.musicianName = musicianName;
   }


   public String getMusicianInfo() {
       return musicianName;
   }


   // inner class
   static class Album {
       private String albumName;


       public Album(String albumName) {
           this.albumName = albumName;
       }


       //non-static variable musicianName
       // can not be accessed in static inner class
       public String getAlbumInfo() {
           return "Album: " + albumName + " Artist: " + musicianName;
       }
   }


}

```