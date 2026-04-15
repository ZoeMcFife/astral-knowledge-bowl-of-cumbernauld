#algorithmic_thinking #java #trees #maps

- ***Name:*** Zoe McFife
- ***Legal Name and Student NR:*** Bunea, S2510238021
- ***Time Spent***: 

<hr>

# 1 Ye Old Item Shoppe — A BST-Powered Shop


## 1.1 `remove(T value)` on BinarySearchTree

BST remove Implementation.

```java
public void remove(T value)  
{  
    if (!contains(value))  
    {  
        return;  
    }  
  
    root = remove(root, value);  
}  
  
private TreeNode<T> remove(TreeNode<T> node, T value)  
{  
    int cmp = value.compareTo(node.value);  
      
    if (cmp < 0)  
    {  
        node.left = remove(node.left, value);  
    }  
    else if (cmp > 0)  
    {  
        node.right = remove(node.right, value);  
    }  
    else  
    {  
        if (node.left == null)  
        {  
            return node.right;  
        }  
  
        if (node.right == null)  
        {  
            return node.left;  
        }  
          
        // Two Children  
        node.value = findSmallest(node.right).value;  
        node.right = remove(node.right, node.value);  
    }  
      
    return node;  
}  
  
private TreeNode<T> findSmallest(TreeNode<T> node)  
{  
    while (node.left != null)  
        node = node.left;  
  
    return node;  
}
```


For cases `no children` and `only one child`, I used two if clauses.

If either right or left node is null, return the other node. If both are null, null gets returned regardless. The parent node gets set to  null or the child node.

```java
if (node.left == null)  
{  
	return node.right;  
}  


if (node.right == null)  
{  
	return node.left;  
}  
          
```

If the node has two child nodes, I set its value to the smallest value in the right subtree. I created a helper method to find the smallest value. 

```java
	// Two Children  
	node.value = findSmallest(node.right).value;  
	node.right = remove(node.right, node.value);  
```

## 1.2 `rangeSearch(T min, T max)` on BinarySearchTree

I use `inorder()` to first create a sorted list of all values. I filter that list using `compareTo` and return it. 

```java
public List<T> rangeSearch(T min, T max)  
{  
    if (min.compareTo(max) >= 0)  
    {  
        throw new IllegalArgumentException("Invalid range; Min: " + min + ", Max: " + max + "; max must be larger than min!");  
    }  
  
    return inorder().stream()  
            .filter(v -> v.compareTo(min) >= 0 && v.compareTo(max) <= 0)  
            .toList();  
}
```

## 1.3 Shop Simulation

### 1.3.1 `ShopItem`

```java
public class ShopItem extends Item implements Comparable<ShopItem>  
{  
    private final int price;  
    private final int offense;  
    private final int defense;  
    private int stock;  
  
    public ShopItem(String name, String type, int weight, int value, int price, int offense, int defense, int stock)  
    {  
        super(name, type, weight, value);  
        this.price = price;  
        this.offense = offense;  
        this.defense = defense;  
        setStock(stock);  
    }  
  
    public void increaseStock()  
    {  
        setStock(getStock() + 1);  
    }  
  
    public void decreaseStock()  
    {  
        setStock(getStock() - 1);  
    }  
  
    private void setStock(int stock)  
    {  
        if (stock < 0)  
        {  
            this.stock = 0;  
            return;  
        }  
  
        this.stock = stock;  
    }  
  
    public int getStock()  
    {  
        return stock;  
    }  
  
    public int getPrice()  
    {  
        return price;  
    }  
  
    public int getOffense()  
    {  
        return offense;  
    }  
  
    public int getDefense()  
    {  
        return defense;  
    }  
  
    @Override  
    public int compareTo(ShopItem o)  
    {  
        // Price  
        int cmp = Integer.compare(this.price, o.price);  
  
        if (cmp != 0) return cmp;  
  
        // if same price, compare name  
        cmp = this.getName().compareTo(o.getName());  
        if (cmp != 0) return cmp;  
  
        // same name, compare type  
        return this.getType().compareTo(o.getType());  
    }  
}
```

For comparing, I used the name and type as secondary comparisons, in case the price is equal. 

<hr>

# 2 Dungeon Run Log — Loops vs. Streams

## 2.1 DungeonRun and random generator

## 2.2 Loop-based queries

## 2.3 Stream rewrites and advanced queries

