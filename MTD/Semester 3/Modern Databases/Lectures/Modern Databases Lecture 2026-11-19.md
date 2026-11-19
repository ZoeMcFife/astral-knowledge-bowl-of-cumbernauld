#modern_databases 

![[05_AggregationII.pdf]]

## Mongo Stuff

```js
db.villagers.aggregate
([
	{
		$unwind: '$furniture'
	},
	{
		$group: 
		{
			_id: '$furniture',
			owners: 
			{
				$sum: 1
			}
		}
	},
	{
		$sort:
		{
			owners: -1
		}
	},
	{
		$lookup:
		{
			from: 'items',
			localField: '_id',
			foreignField: '_id',
			as: 'item'
		}
	},
	{
		$project: 
		{
			item: '$item.name',
			owners: 1
		}
	}
])
```

```js
db.villagers.aggregate
([
	{
		$match: 
		{
			name: 'Admiral'
		}
	},
	{
		$lookup:
		{
			from: 'items', 
			localField: 'furniture',
			foreignField: '_id',
			as: 'owned'
		}
	},
	{
		$project:
		{
			name: 1,
			n: { $size: '$owned' },
			first:
			{
				$first: '$owned.name'
			}
		},
	}
])

```

```js
db.items.aggregate
([
	{
		$match: 
		{
			sell: { $exists: true }
		}
	},
	{
		$bucket:
		{
			groupBy: '$sell',
			boundaries: [0, 1000, 10000, 100000],
			default: 'other',
			output: 
			{
				n: 
				{
					$sum: 1
				},
				examples: 
				{
					$firstN: 
					{
						n: 3,
						input: '$name'
					}
				}
			}
		}
	}
])
```

## outs 

```js
db.items.aggregate
([
	{
		$out: 
		{
			db: 'sandbox',
			coll: 'items'
		}
	}
])
```

```js
db.villagers.aggregate
([
	{
		$unwind: '$furniture'
	},
	{
		$group: 
		{
			_id: '$furniture',
			owners: { $sum: 1}
		}
	},
	{
		$merge:
		{
			into: 'itemPopularity',
			whenMatched: 'replace',
			whenNotMatched: 'insert'
		}
	}
])
```

this one should just be an update 

```js
db.villagers.aggregate
([
	{
		$project: 
		{
			furnitureCount: 
			{
				$size: '$furniture'
			}
		}
	},
	{
		$merge:
		{
			into: 'villagers',
			on: '_id',
			whenMatched: 'merge',
			whenNotMatched: 'discard'
		}
	}
])
```

```js
db.villagers.aggregate
([
	{
		$unwind: '$furniture'
	},
	{
		$group: 
		{
			_id: '$furniture',
			owners: { $sum: 1}
		}
	},
	{
		$merge:
		{
			into: 'items',
			on: '_id',
			whenMatched: 'merge',
			whenNotMatched: 'discard'
		}
	}
])

db.items.find
(
	{
		owners: { $gte: 1 }
	}
)

```

```js
db.items.updateMany
(
	{
		buy: 
		{
			$exists: true
		}
	},
	[
		{
			$set: 
			{ 
				profit: 
				{ 
					$subtract: ['$sell', '$buy']
				} 
			}
		}
	]
)
```

```js
db.items.find
(
	{
		profit: { $gt: 0 }
	},
	{
		name: 1,
		profit: 1
	}
)

```


![[06_IndexesAndValidation.pdf]]