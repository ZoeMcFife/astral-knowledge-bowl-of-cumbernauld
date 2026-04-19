#relational_databases #sql 

![[A05_ProceduresTrigger.pdf]]

```sql
USE Steam_Games;

-- 01:
-- Write a transaction that adds a new game to the database and links it to two genres (Action and Strategy)
-- and a developer (Bethesda Softworks). Ensure that the developer and genres exist before adding.
-- Rollback if any step fails. Use a dedicated test steam appid that does not collide with existing rows.

START TRANSACTION;

SELECT COUNT(*) INTO @action_exists FROM genres WHERE genre_name = 'Action';
SELECT COUNT(*) INTO @strategy_exists FROM genres WHERE genre_name = 'Strategy';
SELECT COUNT(*) INTO @bethesda_exists FROM developers WHERE developer_name = 'Bethesda Softworks';

IF @action_exists = 0 THEN
    ROLLBACK;
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Action Genre is non-existent';
END IF;

IF @strategy_exists = 0 THEN
    ROLLBACK;
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Strategy Genre is non-existent';
END IF;

IF @bethesda_exists = 0 THEN
    ROLLBACK;
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Bethesda Softworks is non-existent';
END IF;

INSERT INTO games VALUES (200000, 'The Elder Scrolls V: Skyrim, 8th Edition', 10, 4, 1, '2026-01-01', 102, 122, 12, 10, 'Very Positive', 100, 100, 0, 69.99);

SELECT * FROM games WHERE name LIKE '%elder scrolls%';

ROLLBACK;

-- Failing Case

START TRANSACTION;

DELETE FROM developers WHERE developer_name LIKE '%Bethesda%'; -- goodbye bethesda </3

SELECT COUNT(*) INTO @action_exists FROM genres WHERE genre_name = 'Action';
SELECT COUNT(*) INTO @strategy_exists FROM genres WHERE genre_name = 'Strategy';
SELECT COUNT(*) INTO @bethesda_exists FROM developers WHERE developer_name = 'Bethesda Softworks';

IF @action_exists = 0 THEN
    ROLLBACK;
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Action Genre is non-existent';
END IF;

IF @strategy_exists = 0 THEN
    ROLLBACK;
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Strategy Genre is non-existent';
END IF;

IF @bethesda_exists = 0 THEN
    ROLLBACK;
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Bethesda Softworks is non-existent';
END IF;

INSERT INTO games VALUES (200000, 'The Elder Scrolls V: Skyrim, 8th Edition', 10, 4, 1, '2026-01-01', 102, 122, 12, 10, 'Very Positive', 100, 100, 0, 69.99);

SELECT * FROM games WHERE name LIKE '%elder scrolls%';

ROLLBACK;

-- 02:
-- Write a transaction to delete a game, and ensure that all related genre, developer, platform,
-- and category associations are removed. If the game does not exist or any step fails, rollback.

START TRANSACTION;

-- 22320 goodbye morrowind
SET @gameToBeDeleted = 22320;

SELECT COUNT(*) INTO @gameExists
                FROM games
                WHERE steam_appid = @gameToBeDeleted;

IF @gameExists = 0 THEN
    ROLLBACK;
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Game does not exist!';
END IF;

-- there is no need to do rollbacks or checks here. if the id exists, it'll be deleted. if it doesnt exist, then good, job already done!
DELETE FROM games WHERE steam_appid = @gameToBeDeleted;
DELETE FROM game_developers WHERE steam_appid = @gameToBeDeleted;
DELETE FROM game_genres WHERE steam_appid = @gameToBeDeleted;
DELETE FROM game_publishers WHERE steam_appid = @gameToBeDeleted;
DELETE FROM game_categories WHERE steam_appid = @gameToBeDeleted;
DELETE FROM game_platforms WHERE steam_appid = @gameToBeDeleted;

SELECT * FROM games WHERE steam_appid = @gameToBeDeleted;

ROLLBACK;

-- failure

START TRANSACTION;

SET @gameToBeDeleted = 654656564456;

SELECT COUNT(*) INTO @gameExists
FROM games
WHERE steam_appid = @gameToBeDeleted;

IF @gameExists = 0 THEN
    ROLLBACK;
    SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Game does not exist!';
END IF;

DELETE FROM games WHERE steam_appid = @gameToBeDeleted;
DELETE FROM game_developers WHERE steam_appid = @gameToBeDeleted;
DELETE FROM game_genres WHERE steam_appid = @gameToBeDeleted;
DELETE FROM game_publishers WHERE steam_appid = @gameToBeDeleted;
DELETE FROM game_categories WHERE steam_appid = @gameToBeDeleted;
DELETE FROM game_platforms WHERE steam_appid = @gameToBeDeleted;

SELECT * FROM games WHERE steam_appid = @gameToBeDeleted;

ROLLBACK;

-- 03:
-- Add a fulltext index on the ‘name‘ and ‘review score desc‘ columns of the ‘games‘ table.
-- Then, write a query that finds games containing both keywords ”Overwhelmingly” and ”Positive” using ‘MATCH ... AGAINST‘.

ALTER TABLE games DROP INDEX IF EXISTS ft_game_name;
ALTER TABLE games DROP INDEX IF EXISTS ft_game_review;

ALTER TABLE games ADD FULLTEXT INDEX ft_game_name (name);
ALTER TABLE games ADD FULLTEXT INDEX ft_game_review (review_score_desc);

SELECT name, review_score_desc
FROM games
WHERE MATCH (review_score_desc) AGAINST ( 'Overwhelmingly Positive' IN NATURAL LANGUAGE MODE);

-- 04:
-- Compare the performance of a query that searches for a game by name using ‘LIKE ’%RPG%’‘
-- versus using a fulltext index (you can use the fulltext index from the previous exercise).

ANALYZE
SELECT name
FROM games
WHERE name LIKE '%RPG%';

-- 70932 rows
-- 71429.00 r_rows
-- 100 filtered
-- 0.34 r_filtered

ANALYZE
SELECT name
FROM games
WHERE MATCH (name) AGAINST ('RPG' IN NATURAL LANGUAGE MODE);

-- 1 rows
-- 200.00 r_rows
-- 100 filtered
-- 100 r_filtered


-- 05:
-- Create a stored function is underrated(appid INT) that returns 1 if a game has at least 80% positive user reviews
-- but a Metacritic score of 55 or lower. It should return 0 otherwise.
-- Make sure to return 0 as well if the game has no user review.

DELIMITER $$

CREATE OR REPLACE FUNCTION is_underrated(IN app_id INT) RETURNS INT

BEGIN
    SELECT games.total_reviews INTO @review_count
    FROM games WHERE steam_appid = app_id;

    IF @review_count = 0 THEN
        RETURN 0;
    END IF;

    SELECT positive_percentual INTO @positive_percentual
    FROM games WHERE steam_appid = app_id;

    SELECT metacritic INTO @metacritic
    FROM games WHERE steam_appid = app_id;

    IF @positive_percentual >= 80.0 AND @metacritic <= 55 THEN
        RETURN 1;
    END IF;

    RETURN 0;
END;
$$
DELIMITER ;

SELECT is_underrated(22340);

-- 06:
-- Write a stored procedure ‘add genre to game(appid INT, genreName VARCHAR(255))‘
-- that checks if the genre exists, and if so, links it to the game. If not, throw an error using ‘SIGNAL‘.

DELIMITER $$

CREATE OR REPLACE PROCEDURE add_genre_to_game(IN app_id INT, IN genre_name VARCHAR(255))

BEGIN
    SELECT COUNT(*) INTO @genre_exists FROM genres WHERE genres.genre_name = genre_name;

    IF @genre_exists = 0 THEN
        ROLLBACK;
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Genre is non-existent';
    END IF;

    SELECT genre_id INTO @genre_id FROM genres WHERE genres.genre_name = genre_name;

    INSERT INTO game_genres VALUES (app_id, @genre_id);
END;
$$
DELIMITER ;

CALL add_genre_to_game(92220, 'Nudity');
CALL add_genre_to_game(92220, 'Bazinga'); -- unfortunately, bazinga does not exist, this till create an error

-- 07:
-- Write a trigger that logs the deletion of a game (name, date, reason) into a ‘game deletion log‘ table.

CREATE TABLE game_deletion_log
(
    log_id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255),
    date DATETIME,
    reason VARCHAR(1000)
);

CREATE OR REPLACE TRIGGER deletion_logger
    AFTER DELETE ON games
    FOR EACH ROW
INSERT INTO game_deletion_log(name, date, reason) VALUES (OLD.name, now(), 'Unknown');

START TRANSACTION;

DELETE FROM games WHERE name LIKE '%Skyrim%';

SELECT * FROM game_deletion_log;

ROLLBACK;

-- 08:
-- Write a ‘BEFORE INSERT‘ trigger that ensures games have a ‘price initial‘ value of 0 if ‘is free = 1‘,
-- otherwise throws an error.

DELIMITER //
CREATE OR REPLACE TRIGGER make_price_0_if_free
    BEFORE INSERT ON games
    FOR EACH ROW
BEGIN
    IF NEW.is_free = 1 AND NEW.price_initial != 0.0 THEN
        SIGNAL SQLSTATE '45000' SET MESSAGE_TEXT = 'Inconsistent Pricing!';
    END IF;
END //
DELIMITER ;

START TRANSACTION;

INSERT INTO games(steam_appid, name, is_free, price_initial)
VALUES (200001, 'Test Game', 1, 0.0);

-- This throws an error.
INSERT INTO games(steam_appid, name, is_free, price_initial)
VALUES (200002, 'Test Failure', 1, 54645);

ROLLBACK;
```