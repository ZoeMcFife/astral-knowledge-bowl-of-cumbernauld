#modern_databases #mongodb #php 

- ***Name:*** Zoe McFife
- ***Legal Name and Student NR:*** Bunea, S2510238021
- ***Time Spent***: XX:XX:XX

<hr>

# 1 Model the Island

## 1.1 Requirements

> An island has a **name**, the player who **owns** it, a **hemisphere** (north or south), a **creation date** and, if known, a **native fruit**. **Residents**: at most **ten villagers** of the catalogue live on an island; the island page always shows their **names**. **Inventory**: **items** of the **catalogue** with a **count**, a few **hundred entries at most**, changed **many times a day**. **Visits**: every time another player visits, the game records **who came and when**, and keeps that forever.

## 1.2 JSON Example

**Islands**

``` json
[
	{
		"_id": ObjectId("6ad3622d82f911e9df683b1d"),
		"name": 'Zoe\'s Bizarre Islands',
		"owner": 'Zoe McFife',
		"hemisphere": 'north',
		"creation_date": ISODate('2024-10-17T10:00:00Z'),
		"native_fruit": 'Strawberry',
		"residents": ['Ankha', 'Audie'],
		"inventory":
		[
			{
				"itemId": 4483,
				"count": 242
			},
			{
				"itemId": 4672,
				"count": 1
			}
		],
		visits: ObjectId('665f1d0a8a1b2c3d4e5f6b17')
	}
]
```

**Visits**

```json
[
	{
		_id: ObjectId('6ad3631a5be09c155729ba16'),
		islandId: ObjectId("6ad3622d82f911e9df683b1d"),
		player: 'Kirsikka Lumikettu',
		visit_time: ISODate('2025-11-11T05:25:21Z')
	},
	{
		_id: ObjectId('6ad3631dbf4803c16c56b1da'),
		islandId: ObjectId("6ad3622d82f911e9df683b1d"),
		player: 'Blerg',
		visit_time: ISODate('2025-01-11T15:05:21Z')
	},
	{
		_id: ObjectId('6ad36320547a1b52da9227d5'),
		islandId: ObjectId("6ad3622d82f911e9df683b1d"),
		player: 'Mina Airfryer',
		visit_time: ISODate('2026-10-17T03:00:21Z')
	},
]
```

## 1.3 Explanation

### **Residents** - Embedded 

The island doesn’t need to know more about the residents than their name (which is unique). When resident information is needed, should be fetched from the villagers collection.

- **Read with parent?** — Yes (only name needed)
- **How many / bounded?** — Few, bounded
- **Shared?** — Yes
- **Changes** — On its own
### **Inventories** - Embedded

Inventories belong to the island. Doesn’t grow that large. Items in the inventory referenced through itemIds.

- **Read with parent?** — Yes
- **How many / bounded?** — A bunch but bounded
- **Shared?** — Not really
- **Changes** — many times a day
### **Visits**

Visits should be their own collection. They reference which island they belong to.

- **Read with parent?** — No
- **How many / bounded?** — Many, not bounded
- **Shared?** — No
- **Changes** — On its own
# 2 The Data Access Layer


# 3 Queries


# 4 Reports