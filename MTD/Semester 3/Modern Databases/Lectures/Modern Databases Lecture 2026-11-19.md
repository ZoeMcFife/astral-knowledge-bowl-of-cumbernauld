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