#relational_databases 

```sql
DROP DATABASE IF EXISTS pokemon_db;
CREATE DATABASE IF NOT EXISTS pokemon_db;

USE pokemon_db;

CREATE TABLE IF NOT EXISTS types
(
	type_name VARCHAR(20) PRIMARY KEY NOT NULL
);

CREATE TABLE IF NOT EXISTS pokemen
(
	pokedex_number INT PRIMARY KEY,
	name VARCHAR(50),
	type_1 VARCHAR(20) NOT NULL,
	type_2 VARCHAR(20),
	ability VARCHAR(1000),
	gender BOOLEAN,
	atk INT,
	sp_atk INT,
	generation INT,
	
	CONSTRAINT fk_type_1 FOREIGN KEY (type_1) REFERENCES types(type_name),
	CONSTRAINT fk_type_2 FOREIGN KEY (type_2) REFERENCES types(type_name) 
);

INSERT INTO types VALUES ("Psychic", "uwu", "Grass", "Ghost");

INSERT INTO pokemen(pokedex_number, name, type_1, generation)
			VALUES 
				(0, "Big Chungus", "Psychic", 1),
				(1, "owo", "uwu", 2);
			
SELECT * FROM pokemon;

ALTER TABLE pokemon
ADD COLUMN def INT
ADD COLUMN sp_def INT,
ADD COLUMN weight_kg DECIMAL(10, 2);

UPDATE pokemon
SET def = 100, sp_def = 100, weight_kg = 1000000
WHERE pokedex_number = 0;

DELETE FROM POKEMEN WHERE pokedex_number = 94;

SELECT * FROM pokemon 
WHERE type_1 IN 
	(SELECT * FROM types
	WHERE type_name = "Psychic");

```
