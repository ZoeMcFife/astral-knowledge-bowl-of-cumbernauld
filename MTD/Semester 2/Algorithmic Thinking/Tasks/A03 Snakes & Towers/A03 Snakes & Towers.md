#algorithmic_thinking 

- ***Name:*** Zoe McFife
- ***Legal Name and Student NR***: Bunea, S2510238021
- ***Time Spent***: 10:42:46 *(has it really been that much? jeez.)*

<hr>

# 1. Towers of Hanoi

## 1.1 PegStack

I at first just extended `java.util.Stack` but my friend told me, that it’s kinda not the point of the exercise and yeaaaah okay fair enough. I made a simple Stack implementation, that’s basically the same as the Java one, but it’s not a `Vector` and just uses an `ArrayList`. 

```java
public class PegStack extends Stack<Integer>  
{  
    @Override  
    public Integer push(Integer item)  
    {  
        if (empty())  
        {  
            add(item);  
            return item;  
        }  
  
        if (peek().compareTo(item) >= 0)  
        {  
            add(item);  
            return item;  
        }  
        else  
        {  
            throw new RuntimeException("Disk cannot be placed on smaller disk!");  
        }  
    }  
  
    public List<String> getAllPegs()  
    {  
        List<String> result = new ArrayList<>();  
  
        for (Integer i : this)  
        {  
            result.add(i.toString());  
        }  
  
        return result.reversed();  
    }  
  
    public List<String> getPegListWithSize(int size)  
    {  
        List<String> result = new ArrayList<>();  
  
        for (int i = 0; i < size - size(); i++)  
        {  
            result.add("|");  
        }  
  
        result.addAll(getAllPegs());  
  
        return result;  
    }  
}
```

### 1.1.1 Test

``` java
PegStack p =  new PegStack();  
  
// this works  
p.push(1);  
  
IO.println(p);  
  
// this is evil and throws an exception  
p.push(3);
```

Yeah, as expected, trying to push `3` onto `1` throws the `RuntimeException`! Yippie!

``` java
Exception in thread "main" java.lang.RuntimeException: Disk cannot be placed on smaller disk!
	at gay.fox.towerOfHanoi.PegStack.push(PegStack.java:26)
	at gay.fox.Main.main(Main.java:31)

[1]
```

## 1.2 TowerOfHanoi

The `TowerOfHanoi` class represents the towers and adds a `move` method that lets you move a disk from a peg to another and handles all exceptions and everything nicely.

``` java
public class TowerOfHanoi  
{  
    private final PegStack[] pegStacks = new PegStack[3];  
  
    private final PegStack a = new PegStack();  
    private final PegStack b  = new PegStack();  
    private final PegStack c  = new PegStack();  
  
    private final int size;  
  
    public int getSize()  
    {  
        return size;  
    }  
  
    public TowerOfHanoi(int size)  
    {  
        this.size = size;  
  
        for (int i = this.size; i > 0; i--)  
        {  
            a.push(i);  
        }  
  
        pegStacks[0] = a;  
        pegStacks[1] = b;  
        pegStacks[2] = c;  
    }  
  
    public boolean isSolved()  
    {  
        return c.size() == size;  
    }  
  
    public void move(int from, int to)  
    {  
        if (from <= 0 || from > 3)  
        {  
            throw new IndexOutOfBoundsException("from must be between 1 and 3");  
        }  
  
        if (to <= 0 || to > 3)  
        {  
            throw new IndexOutOfBoundsException("to must be between 1 and 3");  
        }  
  
        if (from == to)  
        {  
            throw new IllegalArgumentException("from and to must be equal");  
        }  
  
        from = from - 1;  
        to = to - 1;  
  
        if (pegStacks[from].empty())  
        {  
            throw new IllegalArgumentException("from is empty");  
        }  
  
        Integer disk = pegStacks[from].pop();  
  
        try  
        {  
            pegStacks[to].push(disk);  
        }  
        catch (Exception e)  
        {  
            pegStacks[from].push(disk);  
  
            throw new RuntimeException(e);  
        }  
    }  
  
    public void printStacks()  
    {  
		// not displayed for brevity
    }  
}
```

## 1.3 HanoiSolver

This solver just uses the classic recursive algorithm, but uses `TowerOfHanoi.move()` to do the moves! *(I know the assignment says it should call push and pop directly, but that’s a little annoying and would make the code less clear)* 

``` java
public class HanoiSolver  
{  
    private final TowerOfHanoi towerOfHanoi;  
    private int moves;  
    private final boolean printSteps;  
  
    public HanoiSolver(int size, boolean printSteps)  
    {  
        this.towerOfHanoi = new TowerOfHanoi(size);  
        this.printSteps = printSteps;  
    }  
  
    public HanoiSolver(TowerOfHanoi towerOfHanoi, boolean printSteps)  
    {  
        this.towerOfHanoi = towerOfHanoi;  
        this.printSteps = printSteps;  
    }  
  
    public void solve()  
    {  
        moves = 0;  
  
        towerOfHanoi.printStacks();  
  
        solveTower(towerOfHanoi.getSize(), 1, 3, 2);  
  
        towerOfHanoi.printStacks();  
  
        IO.println("Moves: " + moves);  
    }  
  
    private void solveTower(int n, int from, int to, int aux)  
    {  
        if (n == 1)  
        {  
            towerOfHanoi.move(from, to);  
  
            if (printSteps)  
            {  
                towerOfHanoi.printStacks();  
            }  
  
            moves++;
            
            return;  
        }  
  
        solveTower(n - 1, from, aux, to);  
  
        towerOfHanoi.move(from, to);  
  
        if (printSteps)  
        {  
            towerOfHanoi.printStacks();  
        }  
  
        moves++;  
  
        solveTower(n - 1, aux, to, from);  
    }  
}
```

### 1.3.1 HanoiSolver Test Runs

``` java
// I know this could've been a loop shhhhh

HanoiSolver h1 = new HanoiSolver(1, false);  
HanoiSolver h2 = new HanoiSolver(2, false);  
HanoiSolver h3 = new HanoiSolver(3, false);  
HanoiSolver h4 = new HanoiSolver(4, false);  
HanoiSolver h5 = new HanoiSolver(5, false);  
  
h1.solve();  
h2.solve();  
h3.solve();  
h4.solve();  
h5.solve();
```

#### Output

``` java
  |  |  |
  1  |  |
--1--2--3--


  |  |  |
  |  |  1
--1--2--3--

Moves: 1

  |  |  |
  1  |  |
  2  |  |
--1--2--3--


  |  |  |
  |  |  1
  |  |  2
--1--2--3--

Moves: 3

  |  |  |
  1  |  |
  2  |  |
  3  |  |
--1--2--3--


  |  |  |
  |  |  1
  |  |  2
  |  |  3
--1--2--3--

Moves: 7

  |  |  |
  1  |  |
  2  |  |
  3  |  |
  4  |  |
--1--2--3--


  |  |  |
  |  |  1
  |  |  2
  |  |  3
  |  |  4
--1--2--3--

Moves: 15

  |  |  |
  1  |  |
  2  |  |
  3  |  |
  4  |  |
  5  |  |
--1--2--3--


  |  |  |
  |  |  1
  |  |  2
  |  |  3
  |  |  4
  |  |  5
--1--2--3--

Moves: 31

```

#### Table

| n   | Expected | Actual |
| --- | -------- | ------ |
| 1   | 1        | 1      |
| 2   | 3        | 3      |
| 3   | 7        | 7      |
| 4   | 15       | 15     |
| 5   | 31       | 31     |

## 1.4 HanoiGame

`HanoiGame` uses some of my own console UI stuff I made for previous exercises.

``` java
public class HanoiGame extends Screen  
{  
    private TowerOfHanoi towerOfHanoi;  
    private int moveCount = 0;  
  
    @Override  
    public void startScreen()  
    {  
        HanoiCreatorScreen hanoiCreatorScreen = new HanoiCreatorScreen();  
        hanoiCreatorScreen.startScreen();  
  
        towerOfHanoi = hanoiCreatorScreen.getTowerOfHanoi();  
  
        UI.clearScreen();  
  
        while (!towerOfHanoi.isSolved())  
        {  
            towerOfHanoi.printStacks();  
  
            getMove();  
  
            UI.clearScreen();  
        }  
  
        UI.clearScreen();  
        towerOfHanoi.printStacks();  
        UI.printGreen("You did it!");  
        UI.printRed(" " + moveCount + " moves.");  
        UI.printBlankSeparatorLine();  
  
        UI.waitForEnterKey();  
    }  
  
    public void getMove()  
    {  
        int source = 0;  
        int destination = 0;  
  
        boolean moveComplete = false;  
  
        while (!moveComplete)  
        {  
  
            UI.printCyan("Select Source Tower: ");  
            source = UI.getIntInput(1, 3);  
  
            UI.printCyan("Select Destination Tower: ");  
            destination = UI.getIntInput(1, 3);  
  
            try  
            {  
                towerOfHanoi.move(source, destination);  
                moveComplete = true;  
                moveCount++;  
            }  
            catch (Exception e)  
            {  
                UI.printlnRed("Cannot move! dik too small");  
            }  
        }  
    }  
}

```

### 1.4.1 Example Run

``` java
Main Menu
--------------------------------------------------
1. Play Hanoi
2. Try the solver!
Select 1 - 2: > 1


Select your hanoi size!> 2


  |  |  |
  1  |  |
  2  |  |
--1--2--3--

Select Source Tower: > 1
Select Destination Tower: > 2



  |  |  |
  2  1  |
--1--2--3--

Select Source Tower: > 1
Select Destination Tower: > 2
Cannot move! dik too small
Select Source Tower: > 1
Select Destination Tower: > 3


  |  |  |
  |  1  2
--1--2--3--

Select Source Tower: > 2
Select Destination Tower: > 3


  |  |  |
  |  |  1
  |  |  2
--1--2--3--

You did it! 3 moves.
Press Enter to continue...
```

One little change is, that I don’t even let the player do an invalid move, so I don’t print out the stacks again after an invalid try.
## 1.5 Analysis

Stack works because the disks of the Hanoi game are quite literally stacked on each other. You cannot remove disks when a disk is already on top of it, and you can only access the top most disk. It’s a stack. If you’d use a queue instead of a stack, it wouldn’t really work, since a queue is the exact opposite of a stack (**FIFO** instead of **FILO**). You’d place a few disks, but you could only access the bottom most one instead of the top one. You could technically work around it, but it would be fighting against the point of a queue. 

<hr>
# 2 Snake

## 2.1 LinkedList

I made a generic doubly linked list implementation. It had some issues because I’m an idiot sometimes, but I haven’t noticed any issues in testing anymore! *(Update, found an issue. Unit testing is important)*

``` java
public class LinkedList<E> implements Iterable<E>  
{  
    private Node<E> head;  
    private Node<E> tail;  
  
    private int size;  
  
    public LinkedList()  
    {  
        head = null;  
        tail = null;  
        size = 0;  
    }  
  
    public void addFirst(E item)  
    {  
        Node<E> newHead = new Node<>(item);  
  
        if (head != null)  
        {  
            newHead.next = head;  
            head.prev = newHead;  
        }  
  
        head = newHead;  
  
        size++;  
    }  
  
    public void append(E item)  
    {  
        addLast(item);  
    }  
  
    public void set(int index, E item)  
    {  
        Node<E> node = getNode(index);  
        node.data  = item;  
    }  
  
    public void addLast(E item)  
    {  
        Node<E> newTail = new Node<>(item);  
  
        if (size == 0)  
        {  
            addFirst(item);  
            return;  
        }  
  
        if (tail != null)  
        {  
            newTail.prev = tail;  
            tail.next = newTail;  
        }  
  
        if (tail == null)  
        {  
            newTail.prev = head;  
            head.next = newTail;  
        }  
  
        tail = newTail;  
  
        size++;  
    }  
  
    public void addItem(E  item, int index)  
    {  
        if (index < 0 || index > size)  
        {  
            throw new IndexOutOfBoundsException();  
        }  
  
        if (index == 0)  
        {  
            addFirst(item);  
        }  
        else if (index == size)  
        {  
            addLast(item);  
        }  
        else  
        {  
            Node<E> oldNode = getNode(index - 1);  
  
            Node<E> newNode = new Node<>(item);  
            newNode.prev = oldNode;  
            newNode.next = oldNode.next;  
            oldNode.next = newNode;  
  
            size++;  
        }  
    }  
  
    public void remove(int index)  
    {  
        if (isEmpty())  
        {  
            throw new IndexOutOfBoundsException("List is empty");  
        }  
  
        if  (index < 0 || index >= size)  
        {  
            throw new IndexOutOfBoundsException();  
        }  
  
        if (index == 0)  
        {  
            removeFirst();  
        }  
        else if (index == size - 1)  
        {  
            removeLast();  
        }  
        else  
        {  
            Node<E> node = getNode(index);  
  
            node.remove();  
            size--;  
        }  
    }  
  
    public void removeFirst()  
    {  
        if (isEmpty())  
        {  
            throw new IndexOutOfBoundsException("List is empty");  
        }  
  
        Node<E> oldHead = head;  
  
        head = oldHead.next;  
  
        oldHead.remove();  
  
        size--;  
    }  
  
    public void removeLast()  
    {  
        if (isEmpty())  
        {  
            throw new IndexOutOfBoundsException("List is empty");  
        }  
  
        if (tail == null)  
        {  
            removeFirst();  
            return;  
        }  
  
        Node<E> oldTail = tail;  
  
        tail = oldTail.prev;  
  
        oldTail.remove();  
  
        size--;  
    }  
  
    private Node<E> getNode(int index)  
    {  
        if (isEmpty())  
        {  
            throw new IndexOutOfBoundsException("List is empty");  
        }  
  
        if(index < 0 || index >= size)  
        {  
            throw new IndexOutOfBoundsException();  
        }  
  
        if (index == 0)  
        {  
            return head;  
        }  
  
        if  (index == size - 1)  
        {  
            if (tail == null)  
            {  
                return head;  
            }  
  
            return tail;  
        }  
  
        Node<E> current;  
  
        if (index <= size / 2)  
        {  
            current = head;  
  
            for (int i = 0; i < index; i++)  
            {  
                current = current.next;  
            }  
        }  
        else  
        {  
            current = tail;  
  
            for (int i = size - 1; i > index; i--)  
            {  
                current = current.prev;  
            }  
        }  
  
        return current;  
    }  
  
    public E get(int index)  
    {  
        return getNode(index).data;  
    }  
  
    public E getFirst()  
    {  
        return get(0);  
    }  
  
    public E getLast()  
    {  
        return get(size - 1);  
    }  
  
    public int countOccurrences(E item)  
    {  
        if (isEmpty())  
        {  
            return 0;  
        }  
  
        int count = 0;  
  
        Node<E> current;  
  
        current = head;  
  
        for (int i = 0; i < getSize(); i++)  
        {  
            if (current.data.equals(item))  
            {  
                count++;  
            }  
  
            current = current.next;  
        }  
  
        return count;  
    }  
  
    public E find(E item)  
    {  
        if (isEmpty())  
        {  
            return null;  
        }  
  
        Node<E> current;  
  
        current = head;  
  
        for (int i = 0; i < getSize(); i++)  
        {  
            if (current.data.equals(item))  
            {  
                return current.data;  
            }  
  
            current = current.next;  
        }  
  
        return null;  
    }  
  
    public boolean contains(E item)  
    {  
        if (isEmpty())  
        {  
            return false;  
        }  
  
        Node<E> current;  
  
        current = head;  
  
        for (int i = 0; i < getSize(); i++)  
        {  
            if (current.data.equals(item))  
            {  
                return true;  
            }  
  
            current = current.next;  
        }  
  
        return false;  
    }  
  
    @Override  
    public String toString()  
    {  
        StringBuilder result = new StringBuilder("[");  
  
        Node<E>  current = head;  
  
        for (int i = 0;  i != size; i++)  
        {  
            result.append(current.data);  
  
            if (current.next != null)  
            {  
                current = current.next;  
            }  
  
            if (i != size - 1)  
            {  
                result.append(", ");  
            }  
        }  
  
        return result.append("]").toString();  
    }  
  
    public String toNodeString()  
    {  
        StringBuilder result = new StringBuilder("[");  
  
        Node<E>  current = head;  
  
        for (int i = 0;  i != size; i++)  
        {  
            result.append(current);  
  
            if (current.next != null)  
            {  
                current = current.next;  
            }  
  
            if (i != size - 1)  
            {  
                result.append(", ");  
            }  
        }  
  
        return result.append("]").toString();  
    }  
  
    public boolean isEmpty()  
    {  
        return size == 0;  
    }  
  
    public int getSize()  
    {  
        return size;  
    }  
  
    @Override  
    public Iterator<E> iterator()  
    {  
        return new LinkedListIterator<>(head);  
    }  
}
```

### 2.1.1 `removeLast()`

In class we did a simple regular linked implementation, where nodes only knew the next node. The `removeLast()` method would need to go through all nodes before reaching the last, making it O(n). A simple change to make it O(1) is to keep a reference of the tail node. 

## 2.2 Snake

The snake game is split into two parts, the `Snake` and the `World`. The snake is a linked list and the world is a 2D array of tiles. 
### 2.2.1 SnakeList

It’s not the prettiest, but it works.

Moving shifts every position node, feeding adds a tail node. 

```java
public class Snake extends LinkedList<Position>  
{  
    private World world;  
    private Direction direction;  
    private final int initialSnakeSize = 3;  
    private int score = 0;  
  
    public Snake(World world)  
    {  
        setWorld(world);  
  
        Position center = world.getCenter();  
  
        for (int i = 0; i < initialSnakeSize; i++)  
        {  
            append(new Position(center.getX() - i, center.getY()));  
        }  
  
        direction = Direction.RIGHT;  
  
        wrapAround();  
    }  
  
    public boolean isSelfColliding()  
    {  
        return countOccurrences(getFirst()) > 1;  
    }  
  
    public void feed()  
    {  
        Position newTail = new Position(0,0);  
        newTail.copy(getLast());  
        newTail.shift(getOppositeDirection(direction));  
  
        append(newTail);  
  
        score++;  
    }  
  
    public int getScore()  
    {  
        return score;  
    }  
  
    public void move()  
    {  
        move(direction);  
    }  
  
    public void move(Direction direction)  
    {  
        for (int i = getSize() - 1; i > 0; i--)  
        {  
            set(i, new Position(get(i - 1).getX(), get(i - 1).getY()));  
        }  
  
        getFirst().shift(direction);  
  
        wrapAround();  
  
        if (getFirst().equals(world.getCurrentFoodPosition()))  
        {  
            world.spawnFood();  
            feed();  
        }  
        if (world.getTile(getFirst()).equals(Tile.WALL))  
        {  
            world.setIsGameOver(true);  
        }  
    }  
  
  
    // These could've been done more efficiently but oh well lol  
    public void moveLeft()  
    {  
        if (direction == Direction.RIGHT)  
        {  
            return;  
        }  
  
        direction = Direction.LEFT;  
    }  
  
    public void moveRight()  
    {  
        if (direction == Direction.LEFT)  
        {  
            return;  
        }  
  
        direction = Direction.RIGHT;  
    }  
  
    public void moveUp()  
    {  
        if (direction == Direction.DOWN)  
        {  
            return;  
        }  
  
        direction = Direction.UP;  
    }  
  
    public void moveDown()  
    {  
        if (direction == Direction.UP)  
        {  
            return;  
        }  
  
        direction = Direction.DOWN;  
    }  
  
    private void wrapAround()  
    {  
        for (Position p : this)  
        {  
            p.copy(world.wrapAroundPosition(p));  
        }  
    }  
  
    private void setWorld(World world)  
    {  
        this.world = world;  
    }  
  
    public World getWorld()  
    {  
        return world;  
    }  
  
    private Direction getOppositeDirection(Direction direction)  
    {  
        return switch (direction) {  
            case UP -> Direction.DOWN;  
            case LEFT -> Direction.RIGHT;  
            case RIGHT -> Direction.LEFT;  
            default -> Direction.UP;  
        };  
    }  
}
```

### 2.2.2 World

`World` handles updates, food and drawing logic. It draws the positions of food and the snake inside a `Tile` grid. Thus the world can be drawn as ASCII.

``` java
public class World  
{  
    private Tile[][] grid;  
    private final int gridSize;  
  
    private Snake snake;  
  
    private final int MINIMUM_GRID_SIZE = 5;  
    private Position currentFoodPosition;  
  
    private boolean isGameOver = false;  
    private boolean isGameWon = false;  
  
    private boolean createWall = false;  
  
    public World(int gridSize, boolean createWall)  
    {  
        if (gridSize < MINIMUM_GRID_SIZE)  
        {  
            throw new IllegalArgumentException("Grid size must be greater than or equal to 10.");  
        }  
  
        this.gridSize = gridSize;  
        this.createWall = createWall;  
        grid = new Tile[gridSize][gridSize];  
  
        fillTraversalTiles();  
  
        spawnFood();  
    }  
  
    public void spawnFood()  
    {  
        Random rand = new Random();  
        Position food = new Position(rand.nextInt(0, gridSize), rand.nextInt(0, gridSize));  
  
        if (getTile(food).equals(Tile.WALL))  
        {  
            spawnFood();  
            return;  
        }  
  
        setTile(food, Tile.FOOD);  
        currentFoodPosition = food;  
    }  
  
    public void spawnFood(Position position)  
    {  
        setTile(position, Tile.FOOD);  
        currentFoodPosition = position;  
    }  
  
    public Position getCurrentFoodPosition()  
    {  
        return currentFoodPosition;  
    }  
  
    public Position wrapAroundPosition(Position position)  
    {  
        if (position.getX() < 0)  
        {  
            position.setX(gridSize - 1);  
        }  
        else if (position.getX() >= gridSize)  
        {  
            position.setX(0);  
        }  
  
        if (position.getY() < 0)  
        {  
            position.setY(gridSize - 1);  
        }  
        else if (position.getY() >= gridSize)  
        {  
            position.setY(0);  
        }  
  
        return new Position(position.getX(), position.getY());  
    }  
  
    private void fillTraversalTiles()  
    {  
        for (int i = 0; i < gridSize; i++)  
        {  
            for (int j = 0; j < gridSize; j++)  
            {  
                grid[i][j] = Tile.TRAVERSABLE;  
            }  
        }  
  
        if (!createWall)  
            return;  
  
        for (int i = 0; i < gridSize; i++)  
        {  
            grid[0][i] = Tile.WALL;             // top row  
            grid[gridSize - 1][i] = Tile.WALL;  // bottom row  
            grid[i][0] = Tile.WALL;             // left column  
            grid[i][gridSize - 1] = Tile.WALL;  // right column  
        }  
    }  
  
    private void drawSnake()  
    {  
        for (Position snakePos : snake)  
        {  
            if (snakePos.equals(snake.getFirst()))  
            {  
                setTile(snakePos, Tile.SNAKE_HEAD);  
            }  
            else  
            {  
                setTile(snakePos, Tile.SNAKE_TAIL);  
            }  
        }  
    }  
  
    private void drawFood()  
    {  
        setTile(currentFoodPosition, Tile.FOOD);  
    }  
  
    private void setTile(Position pos, Tile tile)  
    {  
        grid[pos.getY()][pos.getX()] = tile;  
    }  
  
	public void update()  
	{  
	    fillTraversalTiles();  
	    drawFood();  
	    snake.move();  
	    drawSnake();  
	  
	    if (snake.isSelfColliding())  
	    {  
	        isGameOver = true;  
	    }  
	  
	    if (createWall)  
	    {  
	        if (snake.getSize() >= gridSize * gridSize - (gridSize * 4 - 4))  
	        {  
	            isGameWon = true;  
	        }  
	    }  
	    else  
	    {  
	        if (snake.getSize() >= gridSize * gridSize)  
	        {  
	            isGameWon = true;  
	        }  
	    }  
	}
  
    public void setIsGameOver(boolean isGameOver)  
    {  
        this.isGameOver = isGameOver;  
    }  
  
    public boolean isGameOver()  
    {  
        return isGameOver;  
    }  
  
    public boolean isGameWon()  
    {  
        return isGameWon;  
    }  
  
    public Tile getTile(Position pos)  
    {  
        return grid[pos.getY()][pos.getX()];  
    }  
  
    public void addSnake(Snake snake)  
    {  
        this.snake = snake;  
    }  
  
    public Position getCenter()  
    {  
        return new Position(gridSize / 2, gridSize / 2);  
    }  
  
    @Override  
    public String toString()  
    {  
        StringBuilder sb = new StringBuilder();  
  
        for (int y = 0; y < gridSize; y++)  
        {  
            for (int x = 0; x < gridSize; x++)  
            {  
                Tile tile = grid[y][x];  
  
                char c;  
                if (tile == null)  
                {  
                    c = '.'; // empty/uninitialized  
                }  
                else  
                {  
                    switch (tile)  
                    {  
                        case SNAKE_HEAD -> c = 'H';  
                        case SNAKE_TAIL -> c = 'o';  
                        case FOOD -> c = '*';  
                        case TRAVERSABLE -> c = '·';  
                        case WALL -> c = '#';  
                        default -> c = '?';  
                    }  
                }  
  
                sb.append(c).append(' ');  
            }  
            sb.append('\n');  
        }  
  
        return sb.toString();  
    }  
}
```

### 2.2.3 Demonstration of `move()` and `feed()`

I set up a simple demo world making the snake move into a manually placed piece of food. `move()` and `feed()` are called in the world method.

```java
World world = new World(15, true);  
Snake snake = new Snake(world);  
world.addSnake(snake);  
world.spawnFood(new Position(10, 7));  
  
world.update();  
IO.println(world);  
  
world.update();  
IO.println(world);  
  
world.update();  
IO.println(world);
```

**Output**

```java
# # # # # # # # # # # # # # # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · o o H · * · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# # # # # # # # # # # # # # # 

# # # # # # # # # # # # # # # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · o o H * · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# # # # # # # # # # # # # # # 

# # # # # # # # # # # # # # # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · * · · # 
# · · · · · · · · · · · · · # 
# · · · · · · o o o H · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# · · · · · · · · · · · · · # 
# # # # # # # # # # # # # # # 
```

## 2.3 Snake Game

### 2.3.1 Snake View

The `World` gets drawn tile by tile onto a canvas. Update speed, walls and world size can be adjusted in the UI.

![[Pasted image 20260412115301.png]]

### 2.3.2 Snake Controller

The snake controller updates the `World` with a timeline and the canvas reacts to key events to change the snakes direction. 

If the win or lose condition is met, the game loop stops.

```java
public class SnakeController  
{  
    @FXML  
    public Label snakeTitle;  
  
    @FXML  
    public Canvas snakeCanvas;  
    @FXML  
    public Button startButton;  
    @FXML  
    public Label currentDifficultyLabel;  
  
    @FXML  
    public CheckBox worldSpawnWithWallsToggle;  
    @FXML  
    public Slider worldSizeSlider;  
    @FXML  
    public Label scoreLabel;  
  
    private World world;  
    private Snake snake;  
  
    private int gridSize;  
    private int pixelGridSize;  
    private GraphicsContext gc;  
  
    private final Color traversableTileColor = Color.BLACK;  
    private final Color foodTileColor = Color.RED;  
    private final Color snakeHeadColor = Color.YELLOW;  
    private final Color snakeTailColor = Color.GREEN;  
    private final Color wallColor = Color.BLUEVIOLET;  
  
    private final int EASY_TICK_TIME = 500;  
    private final int MEDIUM_TICK_TIME = 300;  
    private final int HARD_TICK_TIME = 150;  
    private final int SUICIDE_TICK_TIME = 30;  
  
    private int current_tick = EASY_TICK_TIME;  
  
    private Timeline gameLoop;  
  
    private boolean hasPressedKeyThisTick = false;  
  
    @FXML  
    public void onStartButtonPressed(ActionEvent actionEvent)  
    {  
        initialize();  
    }  
  
    private void update()  
    {  
        hasPressedKeyThisTick = false;  
        world.update();  
  
        scoreLabel.setText("Score: " + snake.getScore());  
  
        drawBoard();  
  
        if (world.isGameOver())  
        {  
            gameLoop.stop();  
            snakeTitle.setText("GAME OVER");  
            startButton.setText("Play Again");  
        }  
  
        if (world.isGameWon())  
        {  
            gameLoop.stop();  
            snakeTitle.setText("GAME WON");  
            startButton.setText("Play Again");  
        }     
    }  
  
    private void drawBoard()  
    {  
        gc.setFill(traversableTileColor);  
        gc.fillRect(0, 0, snakeCanvas.getWidth(), snakeCanvas.getHeight());  
  
        for (int i = 0; i < gridSize; i++)  
        {  
            for (int  j = 0; j < gridSize; j++)  
            {  
                Tile t = world.getTile(new Position(i, j));  
  
                switch (t)  
                {  
                    case FOOD:  
                        drawTileAtPositionInCanvas(foodTileColor, new Position(i, j));  
                        break;  
                    case SNAKE_HEAD:  
                        drawTileAtPositionInCanvas(snakeHeadColor, new Position(i, j));  
                        break;  
                    case SNAKE_TAIL:  
                        drawTileAtPositionInCanvas(snakeTailColor, new Position(i, j));  
                        break;  
                    case WALL:  
                        drawTileAtPositionInCanvas(wallColor, new Position(i, j));  
                        break;  
                }  
            }  
        }  
    }  
  
    private void drawTileAtPositionInCanvas(Color color, Position position)  
    {  
        gc.setFill(color);  
        gc.fillRect(position.getX() * pixelGridSize, position.getY() * pixelGridSize, pixelGridSize, pixelGridSize);  
    }  
  
    private void initialize()  
    {  
        scoreLabel.setText("Score: " + 0);  
        startButton.setText("RESTART");  
        snakeTitle.setText("Snake");  
  
        gridSize = (int) worldSizeSlider.getValue();  
  
        // Snake Game  
        world = new World(gridSize, worldSpawnWithWallsToggle.isSelected());  
        snake = new Snake(world);  
        world.addSnake(snake);  
  
        // Game Loop  
        if (gameLoop != null)  
            gameLoop.stop();  
  
        gameLoop = new Timeline(new KeyFrame(Duration.millis(current_tick), e -> update()));  
        gameLoop.setCycleCount(Timeline.INDEFINITE);  
        gameLoop.play();  
  
        // Canvas  
        gc = snakeCanvas.getGraphicsContext2D();  
  
        pixelGridSize = (int) (snakeCanvas.getHeight() / gridSize);  
  
        snakeCanvas.setFocusTraversable(true);  
        snakeCanvas.setOnKeyPressed(event ->  
        {  
            if (hasPressedKeyThisTick)  
            {  
                return;  
            }  
  
            switch (event.getCode())  
            {  
                case W:  
                    snake.moveUp();  
                    hasPressedKeyThisTick = true;  
                    break;  
                case A:  
                    snake.moveLeft();  
                    hasPressedKeyThisTick = true;  
                    break;  
                case S:  
                    snake.moveDown();  
                    hasPressedKeyThisTick = true;  
                    break;  
                case D:  
                    snake.moveRight();  
                    hasPressedKeyThisTick = true;  
                    break;  
            }  
        });  
  
        snakeCanvas.requestFocus();  
    }  
  
    @FXML  
    public void onEasyButtonPressed(ActionEvent actionEvent)  
    {  
        currentDifficultyLabel.setText("EASY");  
        current_tick = EASY_TICK_TIME;  
    }  
  
    @FXML  
    public void onMediumButtonPressed(ActionEvent actionEvent)  
    {  
        currentDifficultyLabel.setText("MEDIUM");  
        current_tick = MEDIUM_TICK_TIME;  
    }  
  
    @FXML  
    public void onHardButtonPressed(ActionEvent actionEvent)  
    {  
        currentDifficultyLabel.setText("HARD");  
        current_tick = HARD_TICK_TIME;  
    }  
  
    @FXML  
    public void onSuicideButtonPressed(ActionEvent actionEvent)  
    {  
        currentDifficultyLabel.setText("SUICIDE");  
        current_tick = SUICIDE_TICK_TIME;  
    }  
}
```

### 2.3.3 Example Game

![[Pasted image 20260412151317.png]]

![[Pasted image 20260412151409.png]]

![[Pasted image 20260412151402.png]]

![[Pasted image 20260412151431.png]]

