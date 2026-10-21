#modern_databases 

![[04_AggregationI.pdf]]

*from mongodb compass*

```json
[
  {
    $match:
      /**
       * query: The query in MQL.
       */
      {
        type: "fish"
      }
  },
  {
    $group:
      /**
       * _id: The id of the group.
       * fieldN: The first field name.
       */
      {
        _id: "$whereHow",
        n: {
          $sum: 1
        },
        maxSell: {
          $max: "$sell"
        }
      }
  },
  {
    $sort:
      /**
       * Provide any number of field/order pairs.
       */
      {
        maxSell: -1,
        n: -1
      }
  }
]
```

# Query 1

```json
db.items.aggregate
(
	{
		$group: 
		{
			_id: "$type",
			avgSell: { $avg: "$sell" }
		}
	},
	{
		$match:
		{
			avgSell: { $gt: 5000}
		}
	}
)
```

```json
 { _id: 'tool', avgSell: 2660.859375 },  
 { _id: 'shoes', avgSell: 925.7586206896551 },  
 { _id: 'wallmounted', avgSell: 2232.4728682170544 },  
 { _id: 'wallpaper', avgSell: 1510.8846153846155 },  
 { _id: 'bag', avgSell: 1008 },  
 { _id: 'dress', avgSell: 1009.5762711864406 },  
 { _id: 'rug', avgSell: 606.1515151515151 },  
 { _id: 'umbrella', avgSell: 505.35820895522386 },  
 { _id: 'misc', avgSell: 5076.639686684073 },  
 { _id: 'headwear', avgSell: 3590.598086124402 },  
 { _id: 'floor', avgSell: 1550.6647727272727 },  
 { _id: 'fish', avgSell: 3745 },  
 { _id: 'art', avgSell: 1245 },  
 { _id: 'accessory', avgSell: 278.69444444444446 },  
 { _id: 'other', avgSell: 287.25531914893617 },  
 { _id: 'poster', avgSell: 250 },  
 { _id: 'fence', avgSell: 1492.421052631579 },  
 { _id: 'bottoms', avgSell: 298.34507042253523 },  
 { _id: 'fossil', avgSell: 3563.013698630137 },  
 { _id: 'houseware', avgSell: 5274.366541353384 }
```

# Query 2

```json
db.villagers.aggregate
(
	{
		$group:
		{
			_id: 
			{ 
				species: "$species",
				personality: "$personality"
			},
			n:
			{
				$sum: 1
			}
		}
	},
	{
		$sort: { n: -1 }
	}
)
```

```json
[  
 { _id: { species: 'Rabbit', personality: 'Peppy' }, n: 8 },  
 { _id: { species: 'Dog', personality: 'Lazy' }, n: 6 },  
 { _id: { species: 'Bear', personality: 'Cranky' }, n: 5 },  
 { _id: { species: 'Wolf', personality: 'Cranky' }, n: 5 },  
 { _id: { species: 'Frog', personality: 'Jock' }, n: 5 },  
 { _id: { species: 'Squirrel', personality: 'Normal' }, n: 5 },  
 { _id: { species: 'Cat', personality: 'Peppy' }, n: 5 },  
 { _id: { species: 'Cat', personality: 'Snooty' }, n: 5 },  
 { _id: { species: 'Bird', personality: 'Jock' }, n: 4 },  
 { _id: { species: 'Rabbit', personality: 'Lazy' }, n: 4 },  
 { _id: { species: 'Eagle', personality: 'Cranky' }, n: 4 },  
 { _id: { species: 'Cub', personality: 'Lazy' }, n: 4 },  
 { _id: { species: 'Squirrel', personality: 'Snooty' }, n: 4 },  
 { _id: { species: 'Mouse', personality: 'Peppy' }, n: 4 },  
 { _id: { species: 'Elephant', personality: 'Lazy' }, n: 4 },  
 { _id: { species: 'Duck', personality: 'Peppy' }, n: 4 },  
 { _id: { species: 'Penguin', personality: 'Lazy' }, n: 4 },  
 { _id: { species: 'Duck', personality: 'Snooty' }, n: 4 },  
 { _id: { species: 'Sheep', personality: 'Snooty' }, n: 4 },  
 { _id: { species: 'Cub', personality: 'Normal' }, n: 4 }  
]
```

# Query 3

```json
db.items.aggregate
(
[
	{
		$match:
		{
			buy: { $exists: true}
		}
	},
	{
		$project: 
		{
			_id: 0,
			name: 1,
			margin: 
			{
				$subtract: ["$buy", "$sell"]
			}
		}
	}
]
)
```

```json
[  
 { name: '3D glasses', margin: 368 },  
 { name: 'bandage', margin: 105 },  
 { name: 'beak', margin: 368 },  
 { name: 'bottom-rimmed glasses', margin: 825 },  
 { name: 'browline glasses', margin: 780 },  
 { name: 'bubblegum', margin: 105 },  
 { name: 'bunny nose', margin: 420 },  
 { name: 'butterfly shades', margin: 780 },  
 { name: 'cat nose', margin: 420 },  
 { name: 'cucumber pack', margin: 368 },  
 { name: 'curly mustache', margin: 525 },  
 { name: 'cyber shades', margin: 975 },  
 { name: "doctor's mask", margin: 105 },  
 { name: 'dog nose', margin: 420 },  
 { name: 'double-bridge glasses', margin: 825 },  
 { name: 'drinking-straw glasses', margin: 420 },  
 { name: 'eye mask', margin: 420 },  
 { name: 'eye patch', margin: 105 },  
 { name: 'facial mask', margin: 420 },  
 { name: 'fake nose', margin: 368 }  
]
```

# Query 4

```json
db.items.aggregate
([
	{
		$match:
		{
			type: "houseware", 
			diy: true
		}
	},
	{
		$project: 
		{
			_id: 0,
			name: 1,
			colors: { $concatArrays: ["$colors", ["Rainbow"]] }
		}
	}
])
```

```json
[  
 { name: 'acoustic guitar', colors: [ 'Beige', 'Brown', 'Rainbow' ] },  
 { name: 'angled signpost', colors: [ 'Beige', 'Brown', 'Rainbow' ] },  
 { name: 'apple chair', colors: [ 'Red', 'Yellow', 'Rainbow' ] },  
 { name: 'Aquarius urn', colors: [ 'White', 'Yellow', 'Rainbow' ] },  
 {  
   name: 'Aries rocking chair',  
   colors: [ 'White', 'Yellow', 'Rainbow' ]  
 },  
 { name: 'asteroid', colors: [ 'Black', 'Rainbow' ] },  
 { name: 'astronaut suit', colors: [ 'White', 'Black', 'Rainbow' ] },  
 { name: 'bamboo basket', colors: [ 'Green', 'Rainbow' ] },  
 { name: 'bamboo bench', colors: [ 'Green', 'Rainbow' ] },  
 { name: 'bamboo doll', colors: [ 'Green', 'Rainbow' ] },  
 { name: 'bamboo floor lamp', colors: [ 'Green', 'Rainbow' ] },  
 {  
   name: 'bamboo noodle slide',  
   colors: [ 'Green', 'Brown', 'Rainbow' ]  
 },  
 { name: 'bamboo partition', colors: [ 'Green', 'White', 'Rainbow' ] },  
 { name: 'bamboo shelf', colors: [ 'Green', 'White', 'Rainbow' ] },  
 { name: 'bamboo speaker', colors: [ 'Green', 'Beige', 'Rainbow' ] },  
 { name: 'bamboo stool', colors: [ 'Green', 'Rainbow' ] },  
 { name: 'bamboo stopblock', colors: [ 'Green', 'Black', 'Rainbow' ] },  
 { name: 'barbell', colors: [ 'Black', 'Gray', 'Rainbow' ] },  
 { name: 'barrel', colors: [ 'Brown', 'Black', 'Rainbow' ] },  
 { name: "beekeeper's hive", colors: [ 'Beige', 'Rainbow' ] }  
]
```