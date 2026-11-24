#modern_databases 

![[06_IndexesAndValidation.pdf]]

## Indexes

```js
db.items.find
(
	{
		type: 'fish'	
	},
)
.sort
(
	{
		sell: -1
	}
)
.explain
(
	"executionStats"
)
```

```js
db.items.createIndex
(
	{
		type: 1,
		sell: -1
	}
)
```

```js
db.items.getIndexes()
```

```js
db.items.dropIndex
(
	{
		sell: 1
	}
)
```

```js
db.items.dropIndexes()
```

## Schema Validation

```js
db.villagers.insertOne
(
	{
		name: 'Zargothrax'
	}
)
```

```js
db.runCommand
({
	collMod: 'villagers',
	validator: 
	{
		$jsonSchema: 
		{
			bsonType: 'object',
			required: ['name', 'species']
		}
	}
})
```

```js
db.villagers.updateOne
(
	{},
	{
		$set: { name: 'NOT Zargothrax' }
	}
)
```

```js
db.runCommand
({
	collMod: 'villagers',
	validator: 
	{
		$jsonSchema: 
		{
			bsonType: 'object',
			required: ['name', 'species', 'birthday', 'furniture'],
			properties:
			{
				name: 
				{
					bsonType: 'string',
					minLength: 1
				},
				species:
				{
					bsonType: 'string'
				},
				personality:
				{
					enum: 
					[
						'Lazy', 'Normal', 'Cranky', 
						'Snooty', 'Jock', 'Peppy', 
						'Smug', 'Big Sister'
					]
				},
				birthday:
				{
					bsonType: 'object',
					required: ['month', 'day'],
					properties:
					{
						month:
						{
							bsonType: 'int',
							minimum: 1,
							maximum: 12
						},
						day:
						{
							bsonType: 'int',
							minimum: 1,
							maximum: 31
						}
					}
				},
				furniture:
				{
					bsonType: 'array',
					items: 
					{
						bsonType: 'int'
					},
					maxItems: 30
				}
			}
		}
	},
	validationLevel: 'strict',
	validationAction: 'error'
})
```

```js

db.villagers.insertOne({
  name: "dlkjghnsdfhfsdhfsh", species: "Gorilla", personality: "Jock", hobby: "Fitness",
  birthday: "25-1", catchphrase: "baboom",
  favoriteSong: "K.K. Rally", styles: ["Active"], colors: ["Blue", "Black"],
  home: { wallpaper: "kitchen wall", flooring: "brown wood-block flooring" },
  furniture: [717, 4029, 3449],
  schemaVersion: 2
})
```

```js
db.runCommand
({
	collMod: 'villagers',
	validator: 
	{
		$jsonSchema: 
		{
			bsonType: 'object',
			required: ['name', 'species', 'birthday', 'furniture'],
			properties:
			{
				name: 
				{
					bsonType: 'string',
					minLength: 1
				},
				species:
				{
					bsonType: 'string'
				},
				personality:
				{
					enum: 
					[
						'Lazy', 'Normal', 'Cranky', 
						'Snooty', 'Jock', 'Peppy', 
						'Smug', 'Big Sister'
					]
				},
				furniture:
				{
					bsonType: 'array',
					items: 
					{
						bsonType: 'int'
					},
					maxItems: 30
				},
			},
			oneOf:
			[
				{
					not: { required: ['schemaVersion'] },
					properties: 
					{
						birthday:
						{
							bsonType: 'object',
							required: ['month', 'day'],
							properties:
							{
								month:
								{
									bsonType: 'int',
									minimum: 1,
									maximum: 12
								},
								day:
								{
									bsonType: 'int',
									minimum: 1,
									maximum: 31
								}
							}
						}
					}
				},
				{
					required: ['schemaVersion'],
					properties:
					{
						schemaVersion: 
						{
							enum: [2]
						},
						birthday:
						{
							bsonType: 'string',
							pattern: '^[0-9]{1,2}-[0-9]{1,2}$'
						}
					}
				}
			]
		}
	},
	validationLevel: 'strict',
	validationAction: 'error'
})
```
idk it doenst work

```js
db.villagers.updateMany
(
	{
		schemaVersion: { $exists: false },
		species: { $exists: true },
		name: { $type: 'string' }
	},
	[
		{
			$set: 
			{ 
				schemaVersion: 2,
				birthday:
				{
					$concat:
					[
						{
							$toString: '$birthday.day'
						},
						"-",
						{
							$toString: '$birthday.month'
						}
					]
				}
			}
		}
	]
)
```
# Transactions and Consistency 

![[07_TransactionsAndConsistency.pdf]]
# New Sql

![[08_NewSQLI.pdf]]

```sh
sudo docker compose exec cockroach1 cockroach sql --insecure
```

```sql
CREATE DATABASE catalogue;
```

```sql
CREATE TABLE items
(
	id INT PRIMARY KEY,
	name STRING NOT NULL,
	type STRING NOT NULL,
	buy DECIMAL(10,1),
	sell DECIMAL(10,1),
	source STRING
);

CREATE TABLE villagers (
    id INT PRIMARY KEY, name STRING NOT NULL UNIQUE, species STRING NOT NULL,
    personality STRING NOT NULL,
    birthday_month INT NOT NULL CHECK (birthday_month BETWEEN 1 AND 12),
    birthday_day INT NOT NULL);

CREATE TABLE villager_furniture (
    villager_id INT NOT NULL REFERENCES villagers(id), position INT NOT NULL,
    item_id INT NOT NULL REFERENCES items(id),
    PRIMARY KEY (villager_id, position));
```

```sql
IMPORT INTO items CSV DATA 
(
	'nodelocal://1/s4/items.csv'
)
WITH skip = '1', nullif = '';
```

```sql
IMPORT INTO villagers CSV DATA 
('nodelocal://1/s4/villagers.csv') 
WITH skip = '1';
```

```sql
IMPORT INTO villager_furniture CSV DATA ('nodelocal://1/s4/villager_furniture.csv') 
WITH skip = '1';
```

```sql

-- the jobs, and what IMPORT did not check
SELECT split_part(description, ' ', 3) AS target, status FROM [SHOW JOBS] WHERE job_type = 'IMPORT' ORDER BY created DESC LIMIT 3;
SELECT constraint_name, validated FROM [SHOW CONSTRAINTS FROM villager_furniture];
```

```sql
ALTER TABLE villager_furniture VALIDATE CONSTRAINT villager_furniture_villager_id_fkey;
ALTER TABLE villager_furniture VALIDATE CONSTRAINT villager_furniture_item_id_fkey;
ALTER TABLE villagers VALIDATE CONSTRAINT check_birthday_month;
```

```sql
ANALYZE items;
ANALYZE villagers;
ANALYZE villager_furniture;
```
