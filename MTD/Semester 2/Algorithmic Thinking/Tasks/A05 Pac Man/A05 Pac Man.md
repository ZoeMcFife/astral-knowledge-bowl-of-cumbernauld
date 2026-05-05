#algorithmic_thinking #java #pacman #javafx 

- ***Name:*** Zoe McFife
- ***Legal Name and Student NR:*** Bunea, S2510238021
- ***Time Spent***: 06:42:14

<hr>

# Pac.Man

> “_**Pac-Man,**_ originally titled _**Puck Man**_ in Japan, is a 1980 [maze video game](https://en.wikipedia.org/wiki/Maze_video_game "Maze video game") developed and published by [Namco](https://en.wikipedia.org/wiki/Namco "Namco") for [arcades](https://en.wikipedia.org/wiki/Arcade_video_game "Arcade video game"). It was released in Japan on May 22, 1980 and by [Midway Manufacturing](https://en.wikipedia.org/wiki/Midway_Manufacturing "Midway Manufacturing") in North America in August 1980. The player controls [Pac-Man](https://en.wikipedia.org/wiki/Pac-Man_\(character\) "Pac-Man (character)"), who must eat all the dots inside an enclosed maze while avoiding [four colored ghosts](https://en.wikipedia.org/wiki/Ghosts_\(Pac-Man\) "Ghosts (Pac-Man)"). Eating large flashing dots called "Power Pellets" causes the ghosts to temporarily turn blue and vulnerable, allowing Pac-Man to eat the ghosts for bonus points.”

I created some spaghetti code this time, don’t code while tired folks. 
# Maze

Every Pac-Man needs a maze! I decided to make the maze a 2D `Tile` array, split into multiple layers. 

![[Pasted image 20260505123940.png]]
## Maze Parser

I didn’t feel like hard-coding the maze manually, so I created a `MazeParser` class that has an text representation of the maze, that converts into a `Tile` maze.

```java
public class MazeParser  
{  
    private static final String mazeAscii =  
        "############################" +  
        "#············##············#" +  
        "#·####·#####·##·#####·####·#" +  
        "#@#  #·#   #·##·#   #·#  #@#" +  
        "#·####·#####·##·#####·####·#" +  
        "#··························#" +  
        "#·####·##·########·##·####·#" +  
        "#·####·##·########·##·####·#" +  
        "#······##····##····##······#" +  
        "######·##### ## #####·######" +  
        "######·##### ## #####·######" +  
        "######·##          ##·######" +  
        "######·## ###  ### ##·######" +  
        "######·## #      # ##·######" +  
        "      ·   #      #   ·      " +  
        "######·## #      # ##·######" +  
        "######·## ######## ##·######" +  
        "######·##          ##·######" +  
        "######·## ######## ##·######" +  
        "######·## ######## ##·######" +  
        "#············##············#" +  
        "#·####·#####·##·#####·####·#" +  
        "#·####·#####·##·#####·####·#" +  
        "#@··##·······  ·······##··@#" +  
        "###·##·##·########·##·##·###" +  
        "###·##·##·########·##·##·###" +  
        "#······##····##····##······#" +  
        "#·##########·##·##########·#" +  
        "#·##########·##·##########·#" +  
        "#··························#" +  
        "############################";  
  
    public static Tile[][] createTraversalLayer()  
    {  
        Tile[][] traversalLayer = new Tile[Maze.MAZE_ROWS][Maze.MAZE_COLUMNS];  
  
        int i = 0;  
  
        for (int r = 0; r < Maze.MAZE_ROWS; r++)  
        {  
            for (int c = 0; c < Maze.MAZE_COLUMNS; c++)  
            {  
                if (mazeAscii.charAt(i) == '#' || mazeAscii.charAt(i) == ' ')  
                {  
                    traversalLayer[r][c] = parseTileString(mazeAscii.charAt(i), new TilePosition(r, c));  
                }  
                else  
                {  
                    traversalLayer[r][c] = new Tile(TileType.EMPTY, new TilePosition(r, c));  
                }  
                i++;  
            }  
        }  
  
        return traversalLayer;  
    }  
  
    public static Tile[][] createPelletLayer()  
    {  
        Tile[][] pelletLayer = new Tile[Maze.MAZE_ROWS][Maze.MAZE_COLUMNS];  
  
        int i = 0;  
  
        for (int r = 0; r < Maze.MAZE_ROWS; r++)  
        {  
            for (int c = 0; c < Maze.MAZE_COLUMNS; c++)  
            {  
                if (mazeAscii.charAt(i) == '·' || mazeAscii.charAt(i) == '@')  
                {  
                    pelletLayer[r][c] = parseTileString(mazeAscii.charAt(i), new TilePosition(r, c));  
                }  
                i++;  
            }  
        }  
  
        return pelletLayer;  
    }  
  
  
    private static Tile parseTileString(Character tileAscii, TilePosition position)  
    {  
        return switch (tileAscii)  
        {  
            case '#' -> new Tile(TileType.WALL,  position);  
            case 'G' -> new Tile(TileType.GHOST,  position);  
            case 'P' -> new Tile(TileType.PACMAN,  position);  
            case 'F' -> new Tile(TileType.GHOST_FRIGHTENED,  position);  
            case 'E' -> new Tile(TileType.GHOST_EATEN,  position);  
            case 'Ü' -> new Tile(TileType.PACMAN_POWER,  position);  
            case '·' -> new Tile(TileType.PELLET,  position);  
            case '@' -> new Tile(TileType.POWER_PELLET,  position);  
            default -> new Tile(TileType.EMPTY,  position);  
        };  
    }  
  
    public static String getMazeAscii()  
    {  
        StringBuilder sb = new StringBuilder();  
  
        int i = 0;  
  
        for (int r = 0; r < Maze.MAZE_ROWS; r++)  
        {  
            for (int c = 0; c < Maze.MAZE_COLUMNS; c++)  
            {  
                sb.append(mazeAscii.charAt(i));  
                i++;  
            }  
            sb.append("\n");  
        }  
  
        return sb.toString();  
    }  
}
```
## Maze Functionality

The `Maze` class is kinda a mess to be honest.

There’s `Layers` for the maze itself, the pellets, the ghosts and the players. I’m not that happy with this approach but I just stuck with it for this exercise. 

The `Maze` only handles adding the player and ghosts, calling the update method of each ghost, distance measurements and if the player is overlapping or can eat a ghost.

```java
public class Maze  
{  
    public static final int MAZE_ROWS = 31;  
    public static final int MAZE_COLUMNS = 28;  
  
    public static final TilePosition playerStart = new TilePosition(17, 14);  
  
    /**  
     * Used to determine when the player loops around the maze!     */    private static final int MAZE_TELEPORT_ROW = 14;  
  
    private Layer traversalLayer = new Layer(0, MazeParser.createTraversalLayer(), this);  
    private Layer pelletLayer = new Layer(1,  MazeParser.createPelletLayer(), this);  
    private Layer pathFindingPreviewLayer = new Layer(2, this);  
    private List<ActorLayer<Ghost>> ghostLayers = new ArrayList<>();  
    private ActorLayer<Player> playerLayer;  
    private List<Ghost> ghosts = new ArrayList<>();  
  
    public Maze()  
    {  
        addGhosts();  
    }  
  
    private void addGhosts()  
    {  
        ghostLayers.clear();  
        ghosts.clear();  
  
        Ghost blinky = new Ghost("Blinky", new TilePosition(14, 11));  
        Ghost pinky = new Ghost("Pinky", new TilePosition(14, 13));  
        Ghost inky = new Ghost("Inky", new TilePosition(14, 15));  
        Ghost clyde = new Ghost("Clyde", new TilePosition(15, 13));  
  
        blinky.previewPathfinding = true;  
        pinky.previewPathfinding = true;  
        inky.previewPathfinding = true;  
        clyde.previewPathfinding = true;  
  
        addGhost(blinky);  
        addGhost(pinky);  
        addGhost(inky);  
        addGhost(clyde);  
  
        ghosts.add(blinky);  
        ghosts.add(pinky);  
        ghosts.add(inky);  
        ghosts.add(clyde);  
    }  
  
    private boolean canPlayerEatGhost(Ghost ghost)  
    {  
        return ghost.getCurrentState().getName().equals("Fear") && ((Player) playerLayer.getActor()).isSuperPowered();  
    }  
  
    public boolean ghostOverlapsWithPlayer()  
    {  
        for (Ghost ghost : ghosts)  
        {  
            if (ghost.getActorTile().getPos().equals(playerLayer.getActor().getActorTile().getPos()))  
            {  
                if (canPlayerEatGhost(ghost))  
                {  
                    ghost.eatGhost();  
                    return false;  
                }  
  
                if (ghost.getCurrentState().getName().equals("Eaten"))  
                    return false;  
  
                return true;  
            }  
        }  
  
        return false;  
    }  
  
    public void updateGhosts()  
    {  
        pathFindingPreviewLayer.layer = new Tile[MAZE_ROWS][MAZE_COLUMNS];  
  
        for (Ghost ghost : ghosts)  
        {  
            ghost.update();  
  
            if (ghost.previewPathfinding)  
            {  
                for (TilePosition pos : ghost.getCurrentPath())  
                {  
                    pathFindingPreviewLayer.addTile(new Tile(TileType.PATH_FINDING_PREVIEW, pos));  
                }  
            }  
        }  
  
    }  
  
    public List<Layer> getLayersInDrawOrder()  
    {  
        List<Layer> layers = new ArrayList<>();  
  
        layers.add(traversalLayer);  
        layers.add(pathFindingPreviewLayer);  
        layers.add(pelletLayer);  
        layers.addAll(ghostLayers);  
        layers.add(playerLayer);  
  
        return layers;  
    }  
  
    @Override  
    public String toString()  
    {  
        Tile[][] mergedMaze = getMergedMaze();  
  
        StringBuilder sb = new StringBuilder();  
  
        for (int r = 0; r < MAZE_ROWS; r++)  
        {  
            for (int c = 0; c < MAZE_COLUMNS; c++)  
            {  
                sb.append(mergedMaze[r][c].getTileAsciiAppearance());  
            }  
  
            sb.append('\n');  
        }  
  
        return sb.toString();  
    }  
  
    private Tile[][] getMergedMaze()  
    {  
        List<Layer> layers = new ArrayList<>();  
  
        layers.add(traversalLayer);  
        layers.add(pelletLayer);  
        layers.addAll(ghostLayers);  
  
        if (playerLayer != null)  
            layers.add(playerLayer);  
  
        Tile[][] mergedLayer = new Tile[MAZE_ROWS][MAZE_COLUMNS];  
  
        for (Layer layer : layers)  
        {  
            for (int r = 0; r < MAZE_ROWS; r++)  
            {  
                for (int c = 0; c < MAZE_COLUMNS; c++)  
                {  
                    if (layer.layer[r][c] != null)  
                    {  
                        mergedLayer[r][c] = layer.layer[r][c];  
                    }  
                }  
            }  
        }  
  
        return mergedLayer;  
    }  
  
    public TileType getTileType(TilePosition pos)  
    {  
        return getMergedMaze()[pos.getRow()][pos.getCol()].getType();  
    }  
  
    public TileType getTileType(Layer layer, TilePosition pos)  
    {  
        if (layer.layer[pos.getRow()][pos.getCol()] == null)  
            return TileType.EMPTY;  
  
        return layer.layer[pos.getRow()][pos.getCol()].getType();  
    }  
  
    private boolean isTileValidSpawnPosition(TilePosition pos)  
    {  
        return getTileType(pos) == TileType.EMPTY;  
    }  
  
    public boolean isValidTraversableTile(TilePosition pos)  
    {  
        if (getTileType(pos) == TileType.WALL)  
            return false;  
  
        if (pos.getCol() >= MAZE_COLUMNS || pos.getRow() >= MAZE_ROWS)  
            return false;  
  
        if (pos.getCol() < 0 || pos.getRow() < 0)  
            return false;  
  
        return true;  
    }  
  
    public TilePosition getFurthestTileFromPlayer()  
    {  
        List<TilePosition> traversableTiles = new ArrayList<>();  
  
        for (Tile t : traversalLayer.flatten())  
        {  
            if (t.getType().equals(TileType.EMPTY))  
                traversableTiles.add(t.getPos());  
        }  
  
        TilePosition playerPos = getPlayerPostion();  
        TilePosition furthestPosition = new TilePosition();  
        int maxDistance = 0;  
  
        for (TilePosition pos : traversableTiles)  
        {  
            int distance = playerPos.getManhattanDistance(pos);  
  
            if (distance > maxDistance)  
            {  
                maxDistance = distance;  
                furthestPosition = pos;  
            }  
        }  
  
        return furthestPosition;  
    }  
  
    public boolean isValidTraversableTile(TilePosition pos, Layer layer)  
    {  
        if (pos.getCol() >= MAZE_COLUMNS || pos.getRow() >= MAZE_ROWS)  
            return false;  
  
        if (pos.getCol() < 0 || pos.getRow() < 0)  
            return false;  
  
        if (getTileType(layer, pos) == TileType.WALL || getTileType(layer, pos) == TileType.PACMAN)  
            return false;  
  
        return true;  
    }  
  
    public void reset()  
    {  
        addGhosts();  
        resetPlayerPositon();  
    }  
  
    private void resetPlayerPositon()  
    {  
        playerLayer.getActor().getActorTile().setPos(playerStart);  
        playerLayer.getActor().setCurrentDirection(Direction.NONE);  
    }  
  
    public Player createPlayer()  
    {  
        Player p = new Player("Pac Man", playerStart);  
  
        addPlayer(p);  
  
        return p;  
    }  
  
    private void addPlayer(Player player)  
    {  
        if (!isTileValidSpawnPosition(player.getActorTile().getPos()))  
        {  
            throw new IllegalStateException("Player tile has invalid spawn position");  
        }  
  
        playerLayer = new ActorLayer<>(3, player, this);  
        player.setLayer(playerLayer);  
  
        playerLayer.getActor().setCurrentDirection(Direction.NONE);  
    }  
  
    public void addGhost(Ghost ghost)  
    {  
        if (!isTileValidSpawnPosition(ghost.getActorTile().getPos()))  
        {  
            throw new IllegalStateException("Ghost tile has invalid spawn position");  
        }  
  
        ActorLayer<Ghost> ghostLayer = new ActorLayer<>(ghostLayers.size() + 100, ghost, this);  
        ghost.setLayer(ghostLayer);  
        ghostLayers.add(ghostLayer);  
  
        ghost.activateGhost();  
    }  
  
  
    public TilePosition getNextTile(TilePosition pos, Direction direction)  
    {  
        TilePosition nextTilePosition = new TilePosition(pos);  
  
        switch (direction)  
        {  
            case UP:  
                nextTilePosition.setRow(pos.getRow() - 1);  
                break;  
            case DOWN:  
                nextTilePosition.setRow(pos.getRow() + 1);  
                break;  
            case LEFT:  
                if (pos.getRow() == MAZE_TELEPORT_ROW && pos.getCol() == 0)  
                {  
                    nextTilePosition.setRow(MAZE_TELEPORT_ROW);  
                    nextTilePosition.setCol(MAZE_COLUMNS - 1);  
                }  
                else  
                {  
                    nextTilePosition.setCol(pos.getCol() - 1);  
                }  
                break;  
            case RIGHT:  
                if (pos.getRow() == MAZE_TELEPORT_ROW && pos.getCol() == MAZE_COLUMNS - 1)  
                {  
                    nextTilePosition.setRow(MAZE_TELEPORT_ROW);  
                    nextTilePosition.setCol(0);  
                }  
                else  
                {  
                    nextTilePosition.setCol(pos.getCol() + 1);  
                }  
  
                break;  
        }  
  
        return nextTilePosition;  
    }  
  
    public TileType collect(TilePosition pos)  
    {  
        TileType type = getTileType(pelletLayer, pos);  
  
        if (type == TileType.PELLET || type == TileType.POWER_PELLET)  
        {  
            pelletLayer.removeTile(pos);  
            return type;  
        }  
  
        return TileType.EMPTY;  
    }  
  
    public TilePosition getPlayerPostion()  
    {  
        return playerLayer.getActor().getActorTile().getPos();  
    }  
  
    public Layer getTraversalLayer()  
    {  
        return traversalLayer;  
    }  
  
    public boolean isPlayerSuperPowered()  
    {  
        return ((Player) playerLayer.getActor()).isSuperPowered();  
    }  
}
```

### Tiles

Tiles have a type and a position. Nothing fancy.

```java
public enum TileType  
{  
    PACMAN,  
    PACMAN_POWER,  
    GHOST,  
    GHOST_FRIGHTENED,  
    GHOST_EATEN,  
    WALL,  
    PELLET,  
    POWER_PELLET,  
    EMPTY,  
    PATH_FINDING_PREVIEW  
}
```
## Layers

`Layers` are where a lot of the logic actually happens.

They have methods to modify the layer and other methods. I’m not sure why I gave them an id? I think the `Relational Databases` course went a bit to my head. 

```java
public class Layer  
{  
    protected final int layerId;  
    public Tile[][] layer;  
    protected Maze maze;  
  
    public Layer(int layerId, Tile[][] layer, Maze maze)  
    {  
        this.layerId = layerId;  
        this.layer = layer;  
        this.maze = maze;  
    }  
  
    public Layer(int layerId, Maze maze)  
    {  
        this.layerId = layerId;  
        this.maze = maze;  
        this.layer = new Tile[Maze.MAZE_ROWS][Maze.MAZE_COLUMNS];  
    }  
  
    public void addTile(Tile tile)  
    {  
        layer[tile.getPos().getRow()][tile.getPos().getCol()] = tile;  
    }  
  
    public void removeTile(TilePosition position)  
    {  
        layer[position.getRow()][position.getCol()] = null;  
    }  
  
    public TileType getTileType(TilePosition position)  
    {  
        if (layer[position.getRow()][position.getCol()] == null)  
            return TileType.EMPTY;  
  
        return layer[position.getRow()][position.getCol()].getType();  
    }  
  
    public int getLayerId()  
    {  
        return layerId;  
    }  
  
    @Override  
    public String toString()  
    {  
        StringBuilder sb = new StringBuilder();  
  
        sb.append("Layer id: ").append(getLayerId()).append("\n");  
  
        for (int r = 0; r < Maze.MAZE_ROWS; r++)  
        {  
            for (int c = 0; c < Maze.MAZE_COLUMNS; c++)  
            {  
                if (layer[r][c] != null)  
                {  
                    sb.append(layer[r][c].getTileAsciiAppearance());  
                }  
                else  
                {  
                    sb.append(" ");  
                }  
            }  
  
            sb.append('\n');  
        }  
  
        return sb.toString();  
    }  
  
    public List<Tile> flatten()  
    {  
        List<Tile> tiles = new ArrayList<>();  
  
        for (int r = 0; r < Maze.MAZE_ROWS; r++)  
        {  
            for (int c = 0; c < Maze.MAZE_COLUMNS; c++)  
            {  
                if (layer[r][c] != null)  
                    tiles.add(layer[r][c]);  
            }  
        }  
  
        return tiles;  
    }  
  
    public Maze getMaze()  
    {  
        return maze;  
    }  
}
```

### Actor Layer

`ActorLayer`s are a special kind of `Layer` for `Actors`. They handle movement! 

*It’s a little messy…*

```java
public class ActorLayer<T extends Actor> extends Layer  
{  
    protected final T actor;  
  
    public ActorLayer(int layerId, T actor, Maze maze)  
    {  
        super(layerId, maze);  
        this.actor = actor;  
  
        addTile(actor.getActorTile());  
    }  
  
    public void moveActorAlongCurrentDirection()  
    {  
        TilePosition nextPosition = maze.getNextTile(actor.getActorTile().getPos(), actor.getCurrentDirection());  
  
        if (!maze.isValidTraversableTile(nextPosition))  
        {  
            return;  
        }  
  
        TilePosition currentPosition = actor.getActorTile().getPos();  
  
        actor.getActorTile().setPos(nextPosition);  
  
        removeTile(currentPosition);  
        addTile(actor.getActorTile());  
    }  
  
    public void moveActor(TilePosition nextPosition)  
    {  
        if (!maze.isValidTraversableTile(nextPosition))  
        {  
            return;  
        }  
  
        TilePosition currentPosition = actor.getActorTile().getPos();  
  
        actor.getActorTile().setPos(nextPosition);  
  
        removeTile(currentPosition);  
        addTile(actor.getActorTile());  
    }  
  
    public TileType collect()  
    {  
        return maze.collect(actor.getActorTile().getPos());  
    }  
  
    public Actor getActor()  
    {  
        return actor;  
    }  
  
}
```
# Actor

## Player

## Ghost

### State Machine

// plan

#### States

#### Unit Tests


# Gameplay


# JavaFX