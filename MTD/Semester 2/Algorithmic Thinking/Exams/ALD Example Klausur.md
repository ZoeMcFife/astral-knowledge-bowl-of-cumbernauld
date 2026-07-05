#algorithmic_thinking 

![[Semester 2/Algorithmic Thinking/Exams/attachments/SampleExam.pdf]]

<hr>

## Question 1 (20 points)
You want shapes in your drawing app to be drawable and savable, and you are deciding between an abstract class and interfaces.

Explain:
- The difference between an interface and an abstract class (state, constructors, multiple implementation)

> [!ANSWER]
> Interfaces only provide method signatures. (*Default Methods are an exception to this*). They cannot define properties. This is unlike abstract classes, which can define properties, define methods with a body (or purely abstract methods that have to be defined in child classes). Abstract classes can have constructors, but they cannot be instantiated. 
> 
> A big difference is that a class can implement many different interfaces, but only extend one class. 

- Why a class can implement Drawable, Saveable but cannot extend two classes

> [!ANSWER]
> Java doesn’t allow multiple class inheritance, like some other languages like Python allow. There is no other technical limitation to this, just language design choice.

- What "programming to an interface" means and why Drawable\[] items is more flexible than Circle\[] items

> [!Answer]
> Instead of using a concrete class as a type, you use the interface. This allows you to use any class, as long as it implements that specific interface. You don’t have to do any type checks either. 

- What a default method in an interface is, and what the "diamond problem" is when two interfaces define the same default method

> [!Answer]
> A default method is a method in an interface, that provides a default implementation. This is often used when extending existing interfaces in bigger projects. 
> 
> **The diamond problem:**
> 
> Two interfaces can have two identical default method signatures, how does a class figure out which one to use? Java solves it by having you manually define from which interface you’re using the method from.


<hr>

## Question 2 (20 points)
A small game stores players and scores in a database. Explain how a 3-layer architecture (Presentation, Business Logic, Data Access) organises this code.

- What is the responsibility of each layer, with a concrete example for each?

> [!Answer]
> **Data Access**
> Data Access contains all the code necessary to access information from a database. DAOs, REST API clients, and the likes.
> 
> **Business Logic**
> The business layer contains all the business rules and logic. In our example, how scores might be calculated, logic for player name validation, or how a scoreboard should sort itself. 
> 
> **Presentation**
> The presentation layer is purely for the UI. It displays information like player profiles or scores. It gets its information from the business layer and doesn’t communicate with the data access layer at all.  

- State the "golden rule" about which layer may call which

> [!Answer]
> A layer can only access the layer below itself. No calling above or around other layers. 

- Why should there be no SQL in the model classes and no JDBC outside the data-access layer?

> [!Answer]
> You don’t want to mix in responsibilities and you don’t want to hard-code DB specific queries in not easy to swap out sections like model classes. The idea behind a three layer architecture is that you can swap out layers easily. You lose that ability if you mix the layer priorities. If you have JDBC in your business layer, then you have to rework the business layer if you swap out your database.

- Where does a business rule like "a negative score is clamped to 0" belong, and why is that not the DAO's job?

> [!Answer]
> A business rule, obviously belongs in the business layer. That’s where such data validation should happen. A DAO is not responsible for the validation of data, outside of type mismatches and the likes. A DAO is simple used to transfer data and should not implement logic.

<hr>

## Question 3 (18 points)
Tree traversals produce different orderings, and one of them is special for binary search trees (BSTs).

- Describe inorder, preorder, and postorder traversal

> [!Answer]
> **Inorder**
> Left → Root → Right
> Produces a sorted sequence
> 
> **Preorder** *Don’t preorder games kids*
> Root → Left → Right
> Sequences start with the rood node and then all of its child nodes, sorted from left-most to the right-most node.
> 
> **Postorder**
> Left → Right → Root
> Children first, Parents later.

- Which traversal of a BST yields the values in sorted order, and why?

> [!Answer]
> Inorder produces a sorted sequence, since it naturally follows the BSTs sorting. Smaller values go left, larger values go right *(For example)*. 

- Give a typical use for preorder (copy/serialise) and for postorder (deletion / expression evaluation)

> [!Answer]
> Preorder → Copying trees
> Postorder → Deleting a tree, evaluation tree bases expressions

- Sketch the recursive pattern shared by all three (base case + recurse left/right + visit)

> [!Answer]
> ![[tree stuff.excalidraw]]
> 
> ```java
> 
> public void recurse(Node node)
> {
> 	if (baseCase):
> 		return;
> 		
> 	recurse(node.left);
> 	doSomething(node)
> 	recurse(node.right);
> }
> ```

<hr>

## Question 4 (12 points)
Your client uses Java's HttpClient and Jackson to talk to the leaderboard.

- What roles do HttpClient, HttpRequest, and HttpResponse play?

> [!Answer]
> **HttpClient**
> The clienthandles all requests and responses.
> 
> **HttpRequest**
> Used to build a HTTP request. Can have a body with data.
> 
> **HttpResponse**
> Stores HTTP responses with a status code and body. 

- How does ObjectMapper convert between JSON text and Java objects (serialise vs deserialise)?

> [!Answer]
> **Serialization**
> e.g. Converting a POJO to JSON
> 
> **Deserialization**
> e.g. Convert JSON to a POJO
> 
> Object Mapper usually assumes Java classes have lowerCamelCase properties and JSON uses snake_case. This can be manually overwritten in the config or in the java classes with annotations. It uses these assumptions, amongs other things, to convert JSON strings to Java objects.

- Why is it useful to configure the mapper to ignore unknown JSON properties when consuming a third-party API?
  
> [!Answer]
> The domain objects in your project might not implement all the properties that the API gives you. Ignoring unknown properties lets you do this without causing errors.


<hr>

## Question 5 (18 points)
Explain the difference between composition ("has-a") and inheritance ("is-a"), and why "favour composition over inheritance" is common advice.

- Give a clear "has-a" example (e.g. a Player has a Weapon)

> [!Answer]
> A car has an engine

- Give a clear "is-a" example (e.g. a Circle is a Shape)
  
> [!Answer]
> A truck is a vehicle 

- Explain how you decide which relationship to use when modelling a problem

> [!Answer]
> Depends on the context. If I need to model something that contains other objects, then composition is the way to go. But if I need to model a subtype of something, where it wouldn’t make sense to use composition (e.g. Doctor — A doctor is a person and not a person having a doctor, though you could make it so a doctor is a person that has a medical license? See? it all depends again. It’s way too vague of a question to properly answer because it often can be both or neither).

- Give an example where someone uses inheritance but composition would have been the better choice, and explain why

> [!ANSWER]
> Someone wants to model cars and makes a Sedan and a SUV different classes. Composition would separate them, by having body type and co. be a property of a car class. 

<hr>

## Question 6 (12 points)
You implement an inventory as a `List<Item>` (each Item has a name, weight, and value) and need to: remove all items over a weight limit, then list the remaining items sorted by value.

Explain how these concepts work together:
- Collections: why you use `removeIf` instead of removing inside a for-each loop

> [!ANSWER]
> removeIf uses the collections API instead of looping through each object in the list. Probably uses some internal magic to optimize. (Also concurrent modification stuff, you cannot remove an item in a for each loop, you must use an iterator)

- Generics: what `List<Item>` gives you over a raw List

> [!ANSWER]
> A raw list doesn’t exist in Java, so I assume an array is meant.
> 
> Lists implement many additional conveniences, like methods to add / remove specific indices, automatically enlarging the list, sorting, shuffling, etc. 
> 
> With an array, you’d have to manually allocate the memory and do a lot of things manually.

- Ordering: how a Comparator sorts by value, and why equals on Item matters for removal by object
  
> [!ANSWER]
> It uses the the `comparing()` method on types. Which usually returns either -1, 0, or 1. Implementing `equals()` is important, since many Collections depend on these methods for certain actions. `.remove()` is an example, where if you drop in an object, the collection has to know if an object in itself, is the same as the one you want to remove. If `equals()` is undefined or improper, such a comparison may lead to unforseen consequences.
