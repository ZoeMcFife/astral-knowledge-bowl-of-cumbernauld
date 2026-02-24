#modern_code 

![[Kurztest_UE1-9_Reference.pdf]]

# Solution 

```java
private String[] countAnimals(String[] animals)  
{  
    HashMap<String, Integer> animalCounts = new LinkedHashMap<>();  
  
    for (String animal : animals)  
    {  
        int animal_count = Integer.parseInt(animal.split(" ")[0]);  
        String animal_type = animal.split(" ")[1];  
  
        animalCounts.merge(animal_type, animal_count, Integer::sum);  
    }  
  
    String[] result = new String[animalCounts.size()];  
    int index = 0;  
  
    for (String key : animalCounts.keySet())  
    {  
        String animalEntry = animalCounts.get(key).toString() + " " + key;  
        result[index] = animalEntry;  
        index++;  
    }  
  
    return result;  
}
```

```java
private String[] processArray(String[] array)  
{  
    String[] modifiedArray = Arrays.copyOf(array, array.length);  
  
    String key = "mtd";  
    boolean containsKey = false;  
  
    for (String element : modifiedArray)  
    {  
        if (element.equals(key))  
        {  
            containsKey = true;  
            break;  
        }  
    }  
  
    if (containsKey)  
    {  
        for (int i = 0; i < modifiedArray.length; i++)  
        {  
            modifiedArray[i] = reverseString(modifiedArray[i]);  
        }  
  
    }  
  
    if (!containsKey)  
    {  
        for (int i = 0; i < modifiedArray.length; i++)  
        {  
            modifiedArray[i] = modifiedArray[i].toUpperCase();  
        }  
    }  
  
    return modifiedArray;  
}
```

```java
private String reverseString(String str)  
{  
    StringBuilder sb = new StringBuilder();  
  
    for (int i = str.length() - 1; i >= 0; i--)  
    {  
        sb.append(str.charAt(i));  
    }  
  
    return sb.toString();  
}
```

## Excalidraw stufff

![[Modern Code _Short test 2]]