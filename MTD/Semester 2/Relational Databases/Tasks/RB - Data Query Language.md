#relational_databases 

![[A02_DQL.pdf]]

```sql
USE Steam_Games;

-- 01:
-- Find all games released in 2022 that cost less than $30. Show the game name, release date, and price.

SELECT name, release_date, price_initial
FROM games
WHERE YEAR(release_date) = 2022 AND price_initial < 30
ORDER BY release_date;

-- 02:
-- List all games with Adventure in their name, ordered by their release date (newest first);

SELECT * FROM games
WHERE name LIke "%Adventure%"
ORDER BY release_date;

-- 03:
-- Find the top 5 most expensive games in the database that support Mac.
-- Show their name, price, and release date.

SELECT name, price_initial, release_date FROM games
JOIN game_platforms ON games.steam_appid = game_platforms.steam_appid
WHERE game_platforms.platform_id IN (SELECT platform_id FROM platforms WHERE platform_name = "mac")
ORDER BY price_initial DESC
LIMIT 5;

-- 04:
-- Calculate the following statistics for all games:
-- average achievements, minimum achievements, maximum achievements, total number of games,
-- and average initial price.

SELECT AVG(n_achievements) AS "Average Achievements",
       MIN(n_achievements) AS "Minimum Achievements",
       MAX(n_achievements) AS "Maximum Achievements",
       COUNT(steam_appid) AS "Game Count",
       AVG(price_initial) AS "Averave Price"
FROM games;

-- 05:
-- For each year between 2019 and 2022, show the number of games released and their average price.

SELECT YEAR(release_date) AS "Release Year",
       COUNT(steam_appid) AS "Game Count",
       AVG(price_initial) AS "Average Price"
FROM games
WHERE YEAR(release_date) >= 2019
  AND YEAR(release_date) <= 2022
GROUP BY YEAR(release_date);

-- 06:
-- Find all publishers who have published more than 5 games.
-- Show the publisher name, number of games, and the average price of their games.

SELECT publisher_name AS "Publisher Name",
       COUNT(steam_appid) AS "Game Count",
       AVG(price_initial) AS "Average Price"
FROM publishers
NATURAL JOIN game_publishers
NATURAL JOIN games
GROUP BY publishers.publisher_id
HAVING `Game Count` > 5;

-- 07:
-- List all genres that have an average metacritic rating above 75
-- (exclude games that have a metacritic score of 0),
-- showing the genre name, average rating, and number of games in that genre.

SELECT genre_name,
       AVG(metacritic),
       COUNT(steam_appid)
FROM games
NATURAL JOIN game_genres
NATURAL JOIN genres
WHERE metacritic != 0
GROUP BY genre_id;

-- 08:
-- Find all games that support all three platforms (windows, mac, and linux) Show the game name.

SELECT name FROM games
NATURAL JOIN game_platforms
GROUP BY steam_appid
HAVING COUNT(platform_id) = 3;

-- 09:
-- List all developers who have worked with at least two different publishers,
-- showing the developer name and the names of the publishers they’ve worked with.

-- Note from Zoe: this one is a little cursed, sorry

SELECT developer_name,
       GROUP_CONCAT(DISTINCT publisher_name ORDER BY publisher_name SEPARATOR ", ")
FROM developers

    JOIN

    (SELECT developer_id
    FROM game_developers
    NATURAL JOIN game_publishers
    GROUP BY developer_id
    HAVING COUNT(DISTINCT publisher_id) >= 2) AS devs_with_more_than_two_publishers

    ON developers.developer_id = devs_with_more_than_two_publishers.developer_id

JOIN game_developers ON developers.developer_id = game_developers.developer_id
JOIN game_publishers ON game_publishers.steam_appid = game_developers.steam_appid
JOIN publishers ON game_publishers.publisher_id = publishers.publisher_id
GROUP BY developer_name;

-- 10:
-- Find games that have more genres than the average number of genres per game.
-- Show the game name and its genre count.

SELECT name, COUNT(genre_id) AS genre_count
FROM games
NATURAL JOIN game_genres
GROUP BY steam_appid
HAVING COUNT(genre_id) > (SELECT AVG(genre_count)
                          FROM
                              (SELECT COUNT(genre_id) AS genre_count
                               FROM games
                                        NATURAL JOIN game_genres
                               GROUP BY steam_appid) AS t_genre_count);

-- 11:
-- For each developer, show their most expensive game and its release date.
-- Only include developers who have games priced above the overall average game price.

SELECT d.developer_name, g.price_initial, g.name, g.release_date FROM
developers d
JOIN game_developers gd on d.developer_id = gd.developer_id
JOIN games g ON g.steam_appid = gd.steam_appid
WHERE d.developer_id IN
(
    SELECT developer_id
    FROM game_developers
    NATURAL JOIN games
    WHERE price_initial > (SELECT AVG(price_initial) FROM games)
)
AND g.price_initial =
    (
        SELECT MAX(price_initial)
        FROM games
        NATURAL JOIN game_developers
        WHERE developer_id = d.developer_id
    );

```


test