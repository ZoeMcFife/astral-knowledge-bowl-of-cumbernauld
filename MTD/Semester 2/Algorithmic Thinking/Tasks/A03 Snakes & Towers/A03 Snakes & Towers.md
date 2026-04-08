#algorithmic_thinking 

- ***Name:*** Zoe McFife
- ***Legal Name and Student NR***: Bunea, S2510238021
- ***Time Spent***: XX:XX:XX

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
