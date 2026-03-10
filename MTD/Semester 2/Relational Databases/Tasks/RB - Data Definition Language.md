#relational_databases

![[A01_DBCreation.pdf]]

```sql
-- 01:  
CREATE DATABASE IF NOT EXISTS gaymer_db;  
  
USE gaymer_db;  
  
-- Reset  
DROP TABLE IF EXISTS games;  
DROP TABLE IF EXISTS publishers;  
  
  
-- 02:  
CREATE TABLE games  
(  
    id INT AUTO_INCREMENT PRIMARY KEY,  
    name VARCHAR(255) NOT NULL,  
    developer VARCHAR(255),  
    genre VARCHAR(100),  
    achievements INT CHECK (achievements >= 0),  
    release_date DATE,  
    score DECIMAL(3, 1) CHECK (score >= 0 AND score <= 100),  
  
    CONSTRAINT unique_game UNIQUE (name, genre)  
);  
  
-- 03:  
  
INSERT INTO games(name, developer, genre, achievements, release_date, score) VALUES ("Celeste", "Maddy Makes Games", "Platformer", 23, "2018-01-25", 94.0);  
INSERT INTO games(name, developer, genre, achievements, release_date, score) VALUES ("Silksong", "Team Cherry", "Metroidvania", null, null, null);  
INSERT INTO games(name, developer, genre, achievements, release_date, score) VALUES ("Hades", "Supergiant Games", "Roguelite", 49, "2020-09-17", 93.0);  
INSERT INTO games(name, developer, genre, achievements, release_date, score) VALUES ("Stardew Valley", "ConcernedApe", "Simulation", 43, "2016-02-26", 89.0);  
INSERT INTO games(name, developer, genre, achievements, release_date, score) VALUES ("Neon White 2", "Angel Matrix", "Action", null, "2099-06-16", null);  
INSERT INTO games(name, developer, genre, achievements, release_date, score) VALUES ("Disco Elysium", "ZA/UM", "RPG", 35, "2019-10-15", 97.0);  
  
-- 04:  
  
CREATE TABLE publishers  
(  
    name VARCHAR(255) PRIMARY KEY,  
    country VARCHAR(100),  
    founded_year INT NOT NULL CHECK (founded_year >= 1950 AND founded_year <= 2027)  
);  
  
-- 05:  
  
INSERT INTO publishers(name, country, founded_year) VALUES ("Devolver Digital", "United States", 2009);  
INSERT INTO publishers(name, country, founded_year) VALUES ("Supergiant Games", "United States", 2009);  
INSERT INTO publishers(name, country, founded_year) VALUES ("ConcernedApe", "United States", 2012);  
INSERT INTO publishers(name, country, founded_year) VALUES ("Annapurna Interactive", "United States", 2016);  
INSERT INTO publishers(name, country, founded_year) VALUES ("THQ Nordic", "Austria", 2011);  
INSERT INTO publishers(name, country, founded_year) VALUES ("Raw Fury", "Sweden", 2015);  
  
-- 06:  
ALTER TABLE games  
ADD COLUMN publisher_name VARCHAR(255),  
ADD CONSTRAINT fk_publisher FOREIGN KEY (publisher_name) REFERENCES publishers(name) ON DELETE CASCADE;  
  
-- 07:  
-- pretty random, dont feel like like looking up the actual publishers  
  
UPDATE games SET publisher_name = "Devolver Digital" WHERE name IN ("Hades", "Neon White 2");  
UPDATE games SET publisher_name = "Annapurna Interactive" WHERE name IN ("Disco Elysium");  
UPDATE games SET publisher_name = "ConcernedApe" WHERE name IN ("Stardew Valley");  
UPDATE games SET publisher_name = "Supergiant Games" WHERE name IN ("Celeste");  
UPDATE games SET publisher_name = "Raw Fury" WHERE name IN ("Silksong");  
  
-- 08:  
ALTER TABLE games  
ADD COLUMN released BOOLEAN DEFAULT TRUE;  
  
-- 09:  
UPDATE games SET released = FALSE  
WHERE release_date IS NULL  
OR release_date > CURDATE();  
  
-- 10:  
DELETE FROM publishers  
WHERE name = "Annapurna Interactive";  
  
-- 11:  
DELETE FROM publishers  
WHERE name NOT IN (SELECT DISTINCT publisher_name FROM games);
```
