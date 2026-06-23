#algorithmic_thinking #java #rest_api

- ***Name:*** Zoe McFife
- ***Legal Name and Student NR:*** Bunea, S2510238021
- ***Time Spent***: 05:27:01

<hr>

# Cat API

For this exercise I chose the `Cat API`, because I couldn’t think of any other APIs at the time and thought the cat images were cute.
## API

**Base URL -** `https://api.thecatapi.com/v1`

I chose to the implement the following endpoints:

- `/breeds`
- `/breeds/{id}`
- `/images/search?breed_ids={id, ...}`

All other ones require an API key.

## Example Requests

`https://api.thecatapi.com/v1/breeds`

```json
[
	// array of breeds, see below
]
```

`https://api.thecatapi.com/v1/breeds/abob`

```json
{
  "weight": {
    "imperial": "7 - 16",
    "metric": "3 - 7"
  },
  "id": "abob",
  "name": "American Bobtail",
  "cfa_url": "http://cfa.org/Breeds/BreedsAB/AmericanBobtail.aspx",
  "vetstreet_url": "http://www.vetstreet.com/cats/american-bobtail",
  "vcahospitals_url": "https://vcahospitals.com/know-your-pet/cat-breeds/american-bobtail",
  "temperament": "Intelligent, Interactive, Lively, Playful, Sensitive",
  "origin": "United States",
  "country_codes": "US",
  "country_code": "US",
  "description": "American Bobtails are loving and incredibly intelligent cats possessing a distinctive wild appearance. They are extremely interactive cats that bond with their human family with great devotion.",
  "life_span": "11 - 15",
  "indoor": 0,
  "lap": 1,
  "alt_names": "",
  "adaptability": 5,
  "affection_level": 5,
  "child_friendly": 4,
  "dog_friendly": 5,
  "energy_level": 3,
  "grooming": 1,
  "health_issues": 1,
  "intelligence": 5,
  "shedding_level": 3,
  "social_needs": 3,
  "stranger_friendly": 3,
  "vocalisation": 3,
  "experimental": 0,
  "hairless": 0,
  "natural": 0,
  "rare": 0,
  "rex": 0,
  "suppressed_tail": 1,
  "short_legs": 0,
  "wikipedia_url": "https://en.wikipedia.org/wiki/American_Bobtail",
  "hypoallergenic": 0,
  "reference_image_id": "hBXicehMA"
}
```

`https://api.thecatapi.com/v1/images/search?breed_ids=munc`

```json
[
  {
    "id": "njaF1fyqI",
    "url": "https://cdn2.thecatapi.com/images/njaF1fyqI.jpg",
    "width": 1080,
    "height": 1080
  }
]
```

Without an API key, this endpoint can only deliver an array of one image.

![[Pasted image 20260623085726.png]]
## Modelled fields

I only chose to model to more important fields of a breed.

- id
- name
- temperament
- origin
- description
- life_span
- weight

And for the image:

- id
- url

<hr>
# DTOs

*For brevity, I’m excluding all getters / setters, etc.*
## Cat

```java
public class CatDto  
{  
    private String id;  
    private String name;  
    private List<String> temperament;  
    private String origin;  
    private String description;  
    private String lifeSpan;  
    private WeightDto weight;
    
    // ...   
}
```
## Weight

Since `weight` was an array, I decided to model it as its own class. 

```java
public class WeightDto  
{  
    private String imperial;  
    private String metric;
    
    // ...
}
```
## Image

```java
public class ImageDto  
{  
    private String id;  
    private String url;
    
    // ...
}
```

<hr>
# API Client

## REST Service

I abstracted `GET` requests into its own `RestService` class.

The `mapper` ignores non-existent fields and maps `snake_case` to `camelCase`.

```java
public RestService()  
{  
    httpClient = HttpClient.newBuilder()  
            .connectTimeout(java.time.Duration.ofSeconds(10))  
            .build();  
  
    mapper = new ObjectMapper()  
            .registerModule(new JavaTimeModule())  
            .disable(SerializationFeature.WRITE_DATES_AS_TIMESTAMPS)  
            .setPropertyNamingStrategy(PropertyNamingStrategies.SNAKE_CASE)  
            .disable(DeserializationFeature.FAIL_ON_UNKNOWN_PROPERTIES);  
}
```

### GET

Asynchronous `GET` request method with a 15 second timeout.

```java
public <T> CompletableFuture<Optional<T>> get(String url, TypeReference<T> type, int extraEmptyOnFailureCode)  
{  
    HttpRequest request = HttpRequest.newBuilder()  
            .uri(URI.create(url))  
            .timeout(java.time.Duration.ofSeconds(15))  
            .GET()  
            .build();  
  
    return httpClient.sendAsync(request, HttpResponse.BodyHandlers.ofString())  
            .thenApply(response ->  
            {  
                if (response.statusCode() == 404)  
                    return Optional.empty();  
  
                if (response.statusCode() == extraEmptyOnFailureCode)  
                    return Optional.empty();  
  
                if (response.statusCode() != 200)  
                    throw new RuntimeException("GET " + url + " failed! - " +  response.statusCode());  
  
                try  
                {  
                    return Optional.of(mapper.readValue(response.body(), type));  
                }  
                catch (JsonProcessingException e)  
                {  
                    throw new RuntimeException(e);  
                }  
            });  
}
```

There’s a second method in the class, that’s only used to receive `byte[]` data to create the images.

## API Client

The API client implements following endpoints.

```java
public CompletableFuture<List<CatDto>> getAllBreeds()  
{  
    return service.get(Config.getBreedEndpointUrl(), new TypeReference<List<CatDto>>() {})  
            .thenApply(opt -> opt.orElseGet(List::of));  
}
```

```java
public CompletableFuture<Optional<CatDto>> getBreedById(String id)  
{  
    return service.get(Config.getBreedEndpointUrl() + "/" + id, new TypeReference<>() {}, 400);  
}
```

*This is the filtered query :3*

```java
// without an api key, image endpoint returns an array with one image..., kinda annoying tbh  
public CompletableFuture<List<ImageDto>> getImagesOfBreed(String id)  
{  
    return service.get(Config.getImageEndpointUrl() + "/search?breed_ids=" + id, new TypeReference<List<ImageDto>>() {})  
            .thenApply(opt -> opt.orElseGet(List::of));  
}
```

<hr>
# Error Handling

All API requests must be wrapped in a try catch in the UI. Since I made a console app, I made them all synchronous. 

**Example: Breeds Screen**

```java
@Override  
public void startScreen()  
{  
    UI.clearScreen();  
  
    UI.printlnRed("Showing all Breeds: ");  
  
    try  
    {  
        api.getAllBreeds().thenAccept(this::displayBreeds).join();  
    }  
    catch (Exception e)  
    {  
        UI.printlnRed("Failed to show all Breeds. API ERROR.");  
    }  
  
    UI.waitForEnterKey();  
}
```

Pretty much, if the API times out or there’s an error, it throws, otherwise it returns the data or nothing.

This pattern is the same for all the API requests in the program. 

**The API never returns wrong data**
## Not Found

Searching for breeds that don’t exist just display an error and lets you continue. I also limited the length of the input, since IDs are always 4 characters long.

![[Pasted image 20260623085705.png]]

![[Pasted image 20260623085805.png]]

## Timeouts / API Errors

Simulating a network error looks like this:

![[Pasted image 20260623085953.png]]

The application just tells you there was an error. 

<hr>
# Unit Tests

Wrote unit tests using `Wiremock` this time.

![[Pasted image 20260623090814.png]]

## Example

Using `Wiremock` I stub the API responses using pre-saved JSON files. This tests both the API and RestService classes.

```java
@Test  
public void testGetBreedById()  
{  
    wireMock.stubFor(get(urlEqualTo("/breeds/abob"))  
            .willReturn(aResponse()  
                    .withHeader("Content-Type", "application/json")  
                    .withBodyFile("abob.json")));  
  
    CatDto cat = api.getBreedById("abob").join().orElse(null);  
  
    assertNotNull(cat);  
  
    assertEquals("abob", cat.getId());  
    assertEquals("American Bobtail", cat.getName());  
    assertEquals("American Bobtails are loving and incredibly intelligent cats possessing a distinctive wild appearance. They are extremely interactive cats that bond with their human family with great devotion.", cat.getDescription());  
    assertEquals("United States", cat.getOrigin());  
    assertEquals("11 - 15", cat.getLifeSpan());  
    assertEquals(List.of("Intelligent", "Interactive", "Lively", "Playful", "Sensitive"), cat.getTemperament());  
  
    assertNotNull(cat.getWeight());  
    assertEquals("7 - 16", cat.getWeight().getImperial());  
    assertEquals("3 - 7", cat.getWeight().getMetric());  
}
```

You may look at the code for all the other unit tests yourself. I don’t wanna bloat the document with more hundreds of line of code.

<hr>
# Console Demo

Wrote a simple console app. It’s simple, never crashes when the API is having problems. Error messages were seen above. 

![[Pasted image 20260623091142.png]]

## Display all Breeds

![[Pasted image 20260623091159.png]]

## Search by ID

![[Pasted image 20260623091219.png]]

## Search Image

![[Pasted image 20260623091250.png]]

![[image_12361334547008403933.jpg|355]]

![[Pasted image 20260623091307.png]]

Image gets opened using the OSs default program.

It writes it to temp file! 

![[Pasted image 20260623091816.png]]

<hr>

# Async!

I wrote a simple demo showing the programing using async requests with time measurements. This would be more helpful in GUI application.

## Sequential

![[Pasted image 20260623091545.png|447]]
## Asynchronous

Aysnc is much faster, though I can only send at most 9 requests at a time before the API give me a `429` error. First batch is slower than the second batches, presumably due to caching, etc.

![[Pasted image 20260623091608.png]]

![[Pasted image 20260623091624.png]]

