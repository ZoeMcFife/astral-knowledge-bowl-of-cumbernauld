#modern_databases 

![[03_CRUDAndQueries.pdf]]

``` sh
docker compose exec php php import_catalogue.php
```

``` sh
docker compose exec mongodb mongosh catalogue
```


```json
db.villagers.find({$and:[{ $or:[{species: "Cat"}, {species: "Dog"}] }, personality: {$ne:"Lazy"}]}, {name:1})
```

WHATS WRONG HERE I HATE THIS SYNTAX IT SUCKS I HATE IT SO MUCH

```json
db.villagers.find
(
    {
        $and: 
        [
            {$or: [{species:"Cat"}, {species:"Dog"}]},
            {personality: {$ne: "Lazy"}}
        ]
    },
    {
         name: 1
    }
)
```

```json
db.villagers.find
(
    {
        furniture: {$all: [717, 1849]}
    },
    {
        name: 1,
        furniture: 1
    }
)
```

im starting  to grasp…

```json

db.items.find
(
    {
        sell: {$gte: 1000, $lte: 2000},
        diy: {$exists: false}
    },
    {
        name: 1,
        sell: 1,
        _id: 0
    }
)
```

```json
db.items.find
(
    {
        name: /^K\.K\. A/
    },
    {
        name: 1
    }
)
```

```json
db.items.updateOne
(
    {
        _id: 717
    },
    {
        $set: { onSale: true},
        $inc: {sell: -100}
    }
)

```

```json
db.villagers.updateMany
(
    {
        furniture: 717
    },
    {
        $addToSet:  {furniture: 787}
    }
)

```

```json
db.villagers.updateOne
(
    {
        name: "Zucker"
    },
    {
        $rename: {Species: "species"}
    }
)
```

```php
<?php

require __DIR__ . '/vendor/autoload.php';

$mongo = new MongoDB\Client("mongodb://mongodb:27017/?replicaSet=rs0");

$items = $mongo->catalogue->items;


```

```php
<?php

require_once __DIR__ . '/../../connect.php';

$items = $mongo->catalogue->items;

$villagers = $mongo->catalogue->villagers;

$cursor = $items->find(
    ['type' => 'fish', 'whereHow' => 'River'],
    [
        'projection' => ['name' => 1, 'sell' => 1],
        'sort' => ['sell' => -1],
        'limit' => 3
    ]
);

foreach ($cursor as $item)
{
    echo $item->name.' : '.$item->sell."\n";
}

$admiral = $villagers->findOne(
    ['name' => 'Admiral']
);

echo $admiral->name.' : '.$admiral->home->flooring.' and owns '.count($admiral->furniture).' furniteres.'."\n";
```