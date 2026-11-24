---
alias:
---
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