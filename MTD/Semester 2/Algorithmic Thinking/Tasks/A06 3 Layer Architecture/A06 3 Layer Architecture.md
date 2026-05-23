#algorithmic_thinking #java #spring

- ***Name:*** Zoe McFife
- ***Legal Name and Student NR:*** Bunea, S2510238021
- ***Time Spent***: 07:41:23

<hr>

# 1 Database

```sql
SET search_path TO public;  
  
DROP TABLE IF EXISTS game_genres;  
DROP TABLE IF EXISTS genres;  
DROP TABLE IF EXISTS games;  
  
CREATE TABLE genres  
(  
    genre_id SERIAL PRIMARY KEY,  
    name VARCHAR(100) UNIQUE NOT NULL  
);  
  
CREATE TABLE games  
(  
    game_id SERIAL PRIMARY KEY,  
    name VARCHAR(100) NOT NULL,  
    release_date DATE,  
    price DECIMAL(1000,2),  
    review_score INT CHECK (review_score > -1 AND review_score < 11)  
);  
  
CREATE TABLE game_genres  
(  
    game_id INT REFERENCES games (game_id) ON DELETE CASCADE,  
    genre_id INT REFERENCES genres (genre_id) ON DELETE CASCADE ,  
  
    PRIMARY KEY (game_id, genre_id)  
);
```

## 1.1 Docker

``` yml
services:  
  db:  
    image: postgres  
    restart: always  
    shm_size: 128mb  
    volumes:  
      - steam_data:/var/lib/postgresql  
    environment:  
      POSTGRES_USER: admin  
      POSTGRES_PASSWORD: admin  
    ports:  
      - "5432:5432"  
  adminer:  
    image: adminer  
    restart: always  
    ports:  
      - "8080:8080"  
  
volumes:  
  steam_data:
```

## 1.2 Parser

Pretty cruddy *(HA GET IT?)* parser class. 

```java
public class CsvParser  
{  
    public static List<CsvResult> parseCsv(String resourcePath)  
    {  
        List<CsvResult> results = new ArrayList<>();  
  
        try (InputStream in = CsvParser.class.getResourceAsStream(resourcePath);  
             BufferedReader reader = new BufferedReader(new InputStreamReader(in)))  
        {  
            reader.readLine(); // skip header line  
            String line;  
            while ((line = reader.readLine()) != null)  
            {  
                String[] col = line.split(",(?![^\\[]*])", -1);  
  
                int gameId = Integer.parseInt(col[0].trim());  
                String name = col[1].trim();  
  
                Date date = null;  
                String dateRaw = col[10].trim();  
  
                try  
                {  
                    if (!dateRaw.equalsIgnoreCase("Not Released") && !dateRaw.isEmpty())  
                    {  
                        date = Date.valueOf(dateRaw.split(" ")[0]);  
                    }  
                }  
                catch (RuntimeException e)  
                {  
                    date = null;  
                }  
  
                double price;  
  
                try  
                {  
                    price = Double.parseDouble(col[20].trim());  
                }  
                catch (RuntimeException e)  
                {  
                    price = 0;  
                }  
  
                int review_score;  
  
                try  
                {  
                    review_score = (int) Double.parseDouble(col[15].trim());  
                }  
                catch (RuntimeException e)  
                {  
                    review_score = 0;  
                }  
  
                List<Genre> genres = new ArrayList<>();  
  
                String genreRaw = col[5].replaceAll("[\\[\\]\"']", "").trim();  
  
                for (String s : genreRaw.trim().split(","))  
                {  
                    genres.add(new Genre(s.trim()));  
                }  
  
                Game game = new Game(gameId, name, date, price, review_score);  
  
                if (game.isValid())  
                {  
                    CsvResult result = new CsvResult(game, genres);  
                    results.add(result);  
                }  
            }  
        }  
        catch (Exception e)  
        {  
            throw new RuntimeException("Failed to load CSV: " + resourcePath, e);  
        }  
  
        return results;  
    }  
}
```

## 1.3 Seeder

```java
public class DbSeeder  
{  
    public static void seed(GameService gameService, GenreService genreService)  
    {  
        List<CsvResult> list = CsvParser.parseCsv("/steam_games.csv");  
  
        for (CsvResult csvResult : list)  
        {  
            Game game = csvResult.game();  
  
            gameService.addGame(game);  
  
            for (Genre genre : csvResult.genres())  
            {  
                if (!genreService.doesGenreExist(genre.getName()))  
                {  
                    genre = genreService.addGenre(genre.getName());  
                }  
                else  
                {  
                    genre = genreService.getGenreByName(genre.getName());  
                }  
  
                gameService.addGenre(game.getGameId(), genre.getGenreId());  
            }  
        }  
    }  
}
```

![[Pasted image 20260523125950.png]]

Yeah the seeder works! Definitely some questionable games out there! 
# 2 Model Classes

Honestly not much to say here-

Just some basic model classes for each table. 
## 2.1 Game

```java
@Entity  
@Table(name = "games")  
public class Game implements Model  
{  
    @Id  
    private int gameId;  
  
    @Column(name = "name")  
    private String name;  
  
    @Column(name = "release_date")  
    private Date releaseDate;  
  
    @Column(name = "price")  
    private double price;  
  
    @Column(name = "review_score")  
    private int reviewScore;  
  
    @ManyToMany(fetch = FetchType.EAGER)  
    @JoinTable  
            (  
                    name = "game_genres",  
                    joinColumns = @JoinColumn(name = "game_id"),  
                    inverseJoinColumns = @JoinColumn(name = "genre_id")  
            )  
    private List<Genre> genres;  
  
    public Game(int gameId, String name, Date releaseDate, double price, int reviewScore)  
    {  
        setGameId(gameId);  
        setName(name);  
        setReleaseDate(releaseDate);  
        setPrice(price);  
        setReviewScore(reviewScore);  
    }  
  
    public Game()  
    {  
  
    }  
  
    public void setGameId(int gameId)  
    {  
        this.gameId = gameId;  
    }  
  
    public int getGameId()  
    {  
        return gameId;  
    }  
  
    public String getName()  
    {  
        return name;  
    }  
  
    public Date getReleaseDate()  
    {  
        return releaseDate;  
    }  
  
    public double getPrice()  
    {  
        return price;  
    }  
  
    public int getReviewScore()  
    {  
        return reviewScore;  
    }  
  
    public void setName(String name)  
    {  
        this.name = name;  
    }  
  
    public void setReleaseDate(Date releaseDate)  
    {  
        this.releaseDate = releaseDate;  
    }  
  
    public void setPrice(double price)  
    {  
        this.price = price;  
    }  
  
    public void setReviewScore(int reviewScore)  
    {  
        this.reviewScore = reviewScore;  
    }  
  
    public List<Genre> getGenres()  
    {  
        return genres;  
    }  
  
    @Override  
    public String toString()  
    {  
        return "Game{" +  
                "gameId=" + gameId +  
                ", name='" + name + '\'' +  
                ", releaseDate=" + releaseDate +  
                ", price=" + price +  
                ", reviewScore=" + reviewScore +  
                '}';  
    }  
  
    @Override  
    public boolean isValid()  
    {  
        if (name == null || releaseDate == null)  
        {  
            return false;  
        }  
  
        if (price < 0)  
        {  
            return false;  
        }  
  
        if (reviewScore < 0 || reviewScore > 10)  
        {  
            return false;  
        }  
  
        return true;  
    }  
}
```

## 2.2 Genre

```java
@Entity  
@Table(name = "genres")  
public class Genre implements Model  
{  
    @Id  
    @GeneratedValue(strategy = GenerationType.IDENTITY)  
    private int genreId;  
  
    @Column(name = "name")  
    private String name;  
  
    @ManyToMany(mappedBy = "genres")  
    private List<Game> games;  
  
    public Genre(String name)  
    {  
        setName(name);  
    }  
  
    public Genre()  
    {  
  
    }  
  
    public int getGenreId()  
    {  
        return genreId;  
    }  
  
    public String getName()  
    {  
        return name;  
    }  
  
    public void setName(String name)  
    {  
        this.name = name;  
    }  
  
    @Override  
    public String toString()  
    {  
        return "Genre{" +  
                "genreId=" + genreId +  
                ", name='" + name + '\'' +  
                '}';  
    }  
  
    @Override  
    public boolean isValid()  
    {  
        return (name != null);  
    }  
}
```

## 2.3 GameGenre

```java
@Entity  
@Table(name = "game_genres")  
public class GameGenre {  
  
    @EmbeddedId  
    private GameGenreId id;  
  
    @ManyToOne  
    @MapsId("gameId")  
    @JoinColumn(name = "game_id")  
    private Game game;  
  
    @ManyToOne  
    @MapsId("genreId")  
    @JoinColumn(name = "genre_id")  
    private Genre genre;  
  
    public GameGenre() {}  
  
    public GameGenre(Game game, Genre genre)  
    {  
        this.id = new GameGenreId(game.getGameId(), genre.getGenreId());  
        this.game = game;  
        this.genre = genre;  
    }  
  
    public GameGenreId getId()  
    {  
        return id;  
    }  
  
    public void setId(GameGenreId id)  
    {  
        this.id = id;  
    }  
  
    public Game getGame()  
    {  
        return game;  
    }  
  
    public void setGame(Game game)  
    {  
        this.game = game;  
    }  
  
    public Genre getGenre()  
    {  
        return genre;  
    }  
  
    public void setGenre(Genre genre)  
    {  
        this.genre = genre;  
    }  
}
```

```java
@Embeddable  
public class GameGenreId implements Serializable  
{  
    @Column(name = "game_id")  
    private Integer gameId;  
  
    @Column(name = "genre_id")  
    private Integer genreId;  
  
    public GameGenreId() {}  
  
    public GameGenreId(Integer gameId, Integer genreId)  
    {  
        this.gameId = gameId;  
        this.genreId = genreId;  
    }  
  
    @Override  
    public boolean equals(Object o)  
    {  
        if (this == o) return true;  
        if (!(o instanceof GameGenreId that)) return false;  
        return Objects.equals(gameId, that.gameId) &&  
                Objects.equals(genreId, that.genreId);  
    }  
  
    @Override  
    public int hashCode()  
    {  
        return Objects.hash(gameId, genreId);  
    }  
}
```
# 3 Data Access

Spring Repositories are so weird, like it just KNOWS what to do based on the method name? Disgusting….
## 3.1 GameRepository

```java
public interface GameRepository extends CrudRepository<Game, Integer>  
{  
    Optional<Game> findByName(String name);  
    List<Game> findByPriceBetween(double minPrice, double maxPrice);  
  
    @Query("SELECT g FROM Game g JOIN GameGenre gg ON g.gameId = gg.game.gameId WHERE gg.genre.genreId = :genreId")  
    List<Game> findByGenreId(@Param("genreId") int genreId);  
}
```
## 3.2 GenreRepository

```java
public interface GenreRepository extends JpaRepository<Genre,Integer>  
{  
    Optional<Genre> findByName(String name);  
}
```
## 3.3 GameGenreRepository

```java
public interface GameGenreRepository extends CrudRepository<GameGenre, GameGenreId>  
{  
    List<GameGenre> findById_GameId(Integer gameId);  
    List<GameGenre> findById_GenreId(Integer genreId);  
    void deleteById_GameId(Integer gameId);  
}
```
# 4 Services

## 4.2 Game Service

```java
@Service  
public class GameService  
{  
    private final GameRepository gameRepository;  
    private final GenreRepository genreRepository;  
    private final GameGenreRepository gameGenreRepository;  
  
    public GameService(GameRepository gameRepository, GenreRepository genreRepository, GameGenreRepository gameGenreRepository)  
    {  
        this.gameRepository = gameRepository;  
        this.genreRepository = genreRepository;  
        this.gameGenreRepository = gameGenreRepository;  
    }  
  
    @Transactional  
    public Game getGame(int id)  
    {  
        Optional<Game> game = gameRepository.findById(id);  
  
        return game.orElse(null);  
    }  
  
    @Transactional  
    public Game getGameByName(String name)  
    {  
        Optional<Game> game = gameRepository.findByName(name);  
  
        return game.orElse(null);  
    }  
  
    @Transactional  
    public List<Game> getAllGames()  
    {  
        return (List<Game>) gameRepository.findAll();  
    }  
  
    @Transactional  
    public Game addGame(Game game)  
    {  
        if (!game.isValid())  
        {  
            throw new IllegalArgumentException("Game is invalid");  
        }  
  
        return gameRepository.save(game);  
    }  
  
    @Transactional  
    public Game addGame(int gameId, String name, Date releaseDate, double price, int reviewScore)  
    {  
        Game game = new Game(gameId, name, releaseDate, price, reviewScore);  
  
        if (!game.isValid())  
        {  
            throw new IllegalArgumentException("Game is invalid");  
        }  
  
        return gameRepository.save(game);  
    }  
  
    @Transactional  
    public Game updateGame(int gameId, Game game)  
    {  
        Optional<Game> newGame = gameRepository.findById(gameId);  
  
        if (newGame.isEmpty())  
        {  
            throw new IllegalArgumentException("Game doens't exist");  
        }  
  
        if (!game.isValid())  
        {  
            throw new IllegalArgumentException("Game is invalid");  
        }  
  
        newGame.get().setName(game.getName());  
        newGame.get().setReleaseDate(game.getReleaseDate());  
        newGame.get().setPrice(game.getPrice());  
        newGame.get().setReviewScore(game.getReviewScore());  
  
        return gameRepository.save(newGame.get());  
    }  
  
    @Transactional  
    public void deleteGame(int gameId)  
    {  
        Optional<Game> game = gameRepository.findById(gameId);  
  
        if (game.isEmpty())  
        {  
            throw new IllegalArgumentException("Game doens't exist");  
        }  
  
        gameRepository.delete(game.get());  
    }  
  
    @Transactional  
    public GameGenre addGenre(int gameId, int genreId)  
    {  
        Optional<Game> game = gameRepository.findById(gameId);  
  
        if (game.isEmpty())  
        {  
            throw new IllegalArgumentException("Game doens't exist");  
        }  
  
        Optional<Genre> genre = genreRepository.findById(genreId);  
  
        if (genre.isEmpty())  
        {  
            throw new IllegalArgumentException("Genre doens't exist");  
        }  
  
        GameGenre gameGenre = new GameGenre(game.get(), genre.get());  
  
  
        return gameGenreRepository.save(gameGenre);  
    }  
  
    @Transactional  
    public void removeGenre(int gameId, int genreId)  
    {  
        Optional<Game> game = gameRepository.findById(gameId);  
  
        if (game.isEmpty())  
        {  
            throw new IllegalArgumentException("Game doens't exist");  
        }  
  
        GameGenreId gameGenreId = new GameGenreId(gameId, genreId);  
  
        gameGenreRepository.deleteById(gameGenreId);  
    }  
  
    @Transactional  
    public List<Game> findByGenre(int genreId)  
    {  
        return gameRepository.findByGenreId(genreId);  
    }  
  
    @Transactional  
    public List<Game> findByPriceRange(double minPrice, double maxPrice)  
    {  
        if (minPrice < 0 || maxPrice < 0 || minPrice > maxPrice)  
        {  
            throw new IllegalArgumentException("Invalid price range");  
        }  
  
        return gameRepository.findByPriceBetween(minPrice, maxPrice);  
    }  
}
```

## 4.2 Genre Service

```java
@Service  
public class GenreService  
{  
    private final GenreRepository genreRepository;  
  
    public GenreService(GenreRepository genreRepository)  
    {  
        this.genreRepository = genreRepository;  
    }  
  
    @Transactional  
    public Genre getGenreById(int id)  
    {  
        Optional<Genre> genre = genreRepository.findById(id);  
  
        return genre.orElse(null);  
    }  
  
    @Transactional  
    public Genre getGenreByName(String name)  
    {  
        Optional<Genre> genre = genreRepository.findByName(name);  
  
        return genre.orElse(null);  
    }  
  
    @Transactional  
    public List<Genre> getAllGenres()  
    {  
        return genreRepository.findAll();  
    }  
  
    @Transactional  
    public Genre addGenre(String name)  
    {  
        Genre genre = new Genre(name);  
  
        if (!genre.isValid())  
        {  
            throw new IllegalArgumentException("Genre is invalid");  
        }  
  
        return genreRepository.save(genre);  
    }  
  
    @Transactional  
    public Genre updateGenre(int genreId, String name)  
    {  
        Optional<Genre> genre = genreRepository.findById(genreId);  
  
        if (genre.isEmpty())  
        {  
            throw new IllegalArgumentException("Genre not found");  
        }  
  
  
        genre.get().setName(name);  
  
        if (!genre.get().isValid())  
        {  
            throw new IllegalArgumentException("Genre is invalid");  
        }  
  
        return genreRepository.save(genre.get());  
    }  
  
    @Transactional  
    public void deleteGenre(int genreId)  
    {  
        Optional<Genre> genre = genreRepository.findById(genreId);  
  
        if (genre.isEmpty())  
        {  
            throw new IllegalArgumentException("Genre not found");  
        }  
  
        genreRepository.delete(genre.get());  
    }  
  
    @Transactional  
    public boolean doesGenreExist(String name)  
    {  
        return getGenreByName(name) != null;  
    }  
}
```
# 5 Presentation

Made a simple console app.

## 5.1 Preview Screenshots

![[Pasted image 20260523130728.png]]

### 5.1.1 Games!

![[Pasted image 20260523130736.png]]

#### 5.1.1.1 Adding a game:

![[Pasted image 20260523130848.png]]

![[Pasted image 20260523130905.png]]

![[Pasted image 20260523130918.png]]

#### 5.1.1.2 Updating Games: 

![[Pasted image 20260523130958.png]]

![[Pasted image 20260523131047.png]]

![[Pasted image 20260523131034.png]]

#### 5.1.1.3 Find By Genre

![[Pasted image 20260523131247.png]]
#### 5.1.1.4 Find By Price Range

![[Pasted image 20260523131212.png]]

### 5.1.2 Genres

Yeah you can do all the crud stuff here.

![[Pasted image 20260523131414.png]]

![[Pasted image 20260523131432.png]]

![[Pasted image 20260523131443.png]]

![[Pasted image 20260523131336.png]]

# 6 Tests

Wrote the tests with Mockito again. You can look at them in the files, I don’t wanna make this document even bigger by adding another few hundred lines of boring testing code.

## 6.1 Game Service Tests

![[Pasted image 20260523131503.png]]
## 6.2 Genre Service Tests

![[Pasted image 20260523131600.png]]
