# Creating a mapper orchestrator: Animal Mapper Example


## The Problem We're Solving

Imagine we're building a system that needs to handle different types of animals. Each animal type needs to be mapped differently based on its habitat (land, water, or air).
Instead of having complex conditional logic in one place, we'll see how to organize this cleanly using separate mappers and a coordinator class.

## Project Structure

Let's break down the key components of our system:

### 1. Create Data Classes

First, we define the `AnimalData` and `Animal` classes to hold the information about the animals:

```Java
//The object we are mapping
@NoArgsConstructor  
@AllArgsConstructor  
@Data  
@Builder  
public class AnimalInfo {  
    private String animalName;  
    private String habitat;  
}

//what we are mapping it to
@AllArgsConstructor  
@NoArgsConstructor  
@Data  
@Builder  
public class Animal {  
    private String name;  
    private String type;  
    private boolean canFly;  
    private boolean canSwim;  
    private boolean canRun;  
}

```
### 2. The Mapper Interface

We define a common interface that all our animal mappers will implement. This interface ensures that all mappers will have a consistent method to map data from `AnimalData` to `Animal`.

```Java
public interface AnimalMapperInterface {  
    public Animal mapAnimalData(AnimalInfo animalInfo);  
}
```


### 3. Implement the Mappers

We then create specific mapper classes for each animal type. Each mapper implements the `AnimalMapper` interface and handles the mapping logic for its respective animal type.

```Java
@Service  
public class LandAnimalMapper implements AnimalMapperInterface{  
    @Override  
    public Animal mapAnimalData(AnimalInfo animalInfo) {  
        Animal animal = new Animal();  
        animal.setType("Land animal");  
        animal.setName(animalInfo.getAnimalName());  
        animal.setCanRun(true);  
        return animal;  
    }  
}

@Service  
public class SeaAnimalMapper implements AnimalMapperInterface{  
    @Override  
    public Animal mapAnimalData(AnimalInfo animalInfo) {  
        Animal animal = new Animal();  
        animal.setType("Sea animal");  
        animal.setName(animalInfo.getAnimalName());  
        animal.setCanSwim(true);  
        return animal;  
    }  
}

@Service  
public class FlyingAnimalMapper implements AnimalMapperInterface{  
    @Override  
    public Animal mapAnimalData(AnimalInfo animalInfo) {  
        Animal animal = new Animal();  
        animal.setType("Flying animal");  
        animal.setName(animalInfo.getAnimalName());  
        animal.setCanFly(true);  
        return animal;  
    }  
}

```

### 4. Mapper co-ordinator

Here's where the magic happens. The co-ordinator decides which mapper to use:

```Java
@Service  
@RequiredArgsConstructor  
public class AnimalMapperFactory {  
    private final SeaAnimalMapper seaAnimalMapper;  
    private final FlyingAnimalMapper flyingAnimalMapper;  
    private final LandAnimalMapper landAnimalMapper;  
  
    public AnimalMapperInterface getMapper(String habitat) {  
  
        if (habitat.equalsIgnoreCase("Sea")) {  
            return  seaAnimalMapper;  
        }  
  
        if (habitat.equalsIgnoreCase("Air")) {  
            return  flyingAnimalMapper;  
        }  
  
        return landAnimalMapper;  
    }  
  
}

```


### 5. The Service Layer

Finally, we have our service that brings it all together:

```Java
@Service  
@RequiredArgsConstructor  
public class AnimalService {  
    private final AnimalInfoService animalInfoService;  
    private final AnimalMapperFactory animalMapperFactory;  
  
    public Animal mapAnimalData(String animalName) {  
        // Imagine retrieving animal information from an API  
        var animalInfo = animalInfoService.getAnimalInfo(animalName);  
  
        // Select appropriate mapper based on the animal's habitat  
        // (e.g., LandAnimalMapper for land animals, SeaAnimalMapper for sea animals)        var animalMapper = animalMapperFactory.getMapper(animalInfo.getHabitat());  
  
        // Transform AnimalInfo into an Animal object using the selected mapper  
        return animalMapper.mapAnimalData(animalInfo);  
    }  
}
```

## Key Benefits

1. **Separation of Concerns**: Each mapper class handles only its specific type of animal.
2. **Easy to Extend**: Adding a new animal type is as simple as creating a new mapper class.
3. **Clean Code**: No messy if-else statements or switch cases.
4. **Spring Integration**: Leverages Spring's dependency injection for clean architecture.
