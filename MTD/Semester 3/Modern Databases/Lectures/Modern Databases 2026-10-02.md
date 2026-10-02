#modern_databases #mongodb

![[Semester 3/Modern Databases/Lectures/attachments/00_Introduction.pdf]]


![[01_DataThatHurtsInSQL.pdf]]

```js
// Session 1 starter: the documents to paste into mongosh, one block at a time.
// Open the shell first:  docker compose exec mongodb mongosh sandbox
// The queries between the blocks are typed in the session; they are short on purpose.

// Step 1: the 3D glasses
db.items.insertOne({
  name: "3D glasses", type: "accessory", buy: 490, sell: 122,
  variations: ["White", "Black"], colors: ["White", "Colorful"],
  source: "Able Sisters", style: "Active"
})

// Step 2: two more shapes
db.items.insertOne({
  name: "angelfish", type: "fish", sell: 3000, where: "River", shadow: "Small",
  available: { north: { May: "4 PM - 9 AM", Jun: "4 PM - 9 AM",
                        Jul: "4 PM - 9 AM", Aug: "4 PM - 9 AM",
                        Sep: "4 PM - 9 AM", Oct: "4 PM - 9 AM" } }
})
db.items.insertOne({
  name: "Agent K.K.", type: "music", buy: 3200, sell: 800, source: "K.K. concert"
})

// Step 5: Admiral and Boone
db.villagers.insertOne({
  name: "Admiral", species: "Bird", personality: "Cranky", hobby: "Nature",
  birthday: { month: 1, day: 27 }, catchphrase: "aye aye",
  favoriteSong: "Steep Hill", styles: ["Cool"], colors: ["Black", "Blue"],
  home: { wallpaper: "dirt-clod wall", flooring: "tatami" },
  furniture: [717, 1849, 7047, 2736, 787, 5970, 3449, 3622, 3802, 4106, 3438, 4029]
})
db.villagers.insertOne({
  name: "Boone", species: "Gorilla", personality: "Jock", hobby: "Fitness",
  birthday: { month: 9, day: 12 }, catchphrase: "baboom",
  favoriteSong: "K.K. Rally", styles: ["Active"], colors: ["Blue", "Black"],
  home: { wallpaper: "kitchen wall", flooring: "brown wood-block flooring" },
  furniture: [717, 4029, 3449]
})

// Step 10: the broken pochette (type it exactly like this, capital N and the quotes included)
db.items.insertOne({ Name: "acorn pochette", type: "bag", sell: "2400" })

```

``` json
❯ sudo docker compose exec mongodb mongosh sandbox  
Current Mongosh Log ID: 6abfae9f02e697c79b13d9e8  
Connecting to:          mongodb://127.0.0.1:27017/sandbox?directConnection=true&serverSelecti  
onTimeoutMS=2000&appName=mongosh+2.9.2  
Using MongoDB:          8.2.12  
Using Mongosh:          2.9.2  
  
For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/  
  
  
To help improve our products, anonymous usage data is collected and sent to MongoDB periodica  
lly (https://www.mongodb.com/legal/privacy-policy).  
You can opt-out by running the disableTelemetry() command.  
  
------  
  The server generated these startup warnings when booting  
  2026-10-02T11:52:09.887+00:00: Access control is not enabled for the database. Read and write access to data and configuration is unrestricted  
  2026-10-02T11:52:09.887+00:00: Soft rlimits for open file descriptors too low  
  2026-10-02T11:52:09.887+00:00: We suggest setting the contents of sysfsFile to 0.  
  2026-10-02T11:52:09.887+00:00: We suggest setting swappiness to 0 or 1, as swapping can cause performance problems.  
------  
```

```json  
rs0 [direct: primary] sandbox> db.items.insertOne({  
|   name: "3D glasses", type: "accessory", buy: 490, sell: 122,  
|   variations: ["White", "Black"], colors: ["White", "Colorful"],  
|   source: "Able Sisters", style: "Active"  
| })  
{  
 acknowledged: true,  
 insertedId: ObjectId('6abfb2d402e697c79b13d9e9')  
}  
rs0 [direct: primary] sandbox> db.items.insertOne({  
|   name: "angelfish", type: "fish", sell: 3000, where: "River", shadow: "Small",  
|   available: { north: { May: "4 PM - 9 AM", Jun: "4 PM - 9 AM",  
|                         Jul: "4 PM - 9 AM", Aug: "4 PM - 9 AM",  
|                         Sep: "4 PM - 9 AM", Oct: "4 PM - 9 AM" } }  
| })  
| db.items.insertOne({  
|   name: "Agent K.K.", type: "music", buy: 3200, sell: 800, source: "K.K. concert"  
| })  
{  
 acknowledged: true,  
 insertedId: ObjectId('6abfb3be02e697c79b13d9eb')  
}  

rs0 [direct: primary] sandbox> db.items.find()  
[  
 {  
   _id: ObjectId('6abfb2d402e697c79b13d9e9'),  
   name: '3D glasses',  
   type: 'accessory',  
   buy: 490,  
   sell: 122,  
   variations: [ 'White', 'Black' ],  
   colors: [ 'White', 'Colorful' ],  
   source: 'Able Sisters',  
   style: 'Active'  
 },  
 {  
   _id: ObjectId('6abfb3be02e697c79b13d9ea'),  
   name: 'angelfish',  
   type: 'fish',  
   sell: 3000,  
   where: 'River',  
   shadow: 'Small',  
   available: {  
     north: {  
       May: '4 PM - 9 AM',  
       Jun: '4 PM - 9 AM',  
       Jul: '4 PM - 9 AM',  
       Aug: '4 PM - 9 AM',  
       Sep: '4 PM - 9 AM',  
       Oct: '4 PM - 9 AM'  
     }  
   }  
 },  
 {  
   _id: ObjectId('6abfb3be02e697c79b13d9eb'),  
   name: 'Agent K.K.',  
   type: 'music',  
   buy: 3200,  
   sell: 800,  
   source: 'K.K. concert'  
 }  
]  
  
rs0 [direct: primary] sandbox> db.items.find({sell: {$lt: 800}})  
[  
 {  
   _id: ObjectId('6abfb2d402e697c79b13d9e9'),  
   name: '3D glasses',  
   type: 'accessory',  
   buy: 490,  
   sell: 122,  
   variations: [ 'White', 'Black' ],  
   colors: [ 'White', 'Colorful' ],  
   source: 'Able Sisters',  
   style: 'Active'  
 }  
]  
rs0 [direct: primary] sandbox> db.items.find()  
[  
 {  
   _id: ObjectId('6abfb2d402e697c79b13d9e9'),  
   name: '3D glasses',  
   type: 'accessory',  
   buy: 490,  
   sell: 122,  
   variations: [ 'White', 'Black' ],  
   colors: [ 'White', 'Colorful' ],  
   source: 'Able Sisters',  
   style: 'Active'  
 },  
 {  
   _id: ObjectId('6abfb3be02e697c79b13d9ea'),  
   name: 'angelfish',  
   type: 'fish',  
   sell: 3000,  
   where: 'River',  
   shadow: 'Small',  
   available: {  
     north: {  
       May: '4 PM - 9 AM',  
       Jun: '4 PM - 9 AM',  
       Jul: '4 PM - 9 AM',  
       Aug: '4 PM - 9 AM',  
       Sep: '4 PM - 9 AM',  
       Oct: '4 PM - 9 AM'  
     }  
   }  
 },  
 {  
   _id: ObjectId('6abfb3be02e697c79b13d9eb'),  
   name: 'Agent K.K.',  
   type: 'music',  
   buy: 3200,  
   sell: 800,  
   source: 'K.K. concert'  
 }  
]  

rs0 [direct: primary] sandbox> db.items.find({type: "fish"})  
[  
 {  
   _id: ObjectId('6abfb3be02e697c79b13d9ea'),  
   name: 'angelfish',  
   type: 'fish',  
   sell: 3000,  
   where: 'River',  
   shadow: 'Small',  
   available: {  
     north: {  
       May: '4 PM - 9 AM',  
       Jun: '4 PM - 9 AM',  
       Jul: '4 PM - 9 AM',  
       Aug: '4 PM - 9 AM',  
       Sep: '4 PM - 9 AM',  
       Oct: '4 PM - 9 AM'  
     }  
   }  
 }  
]  
rs0 [direct: primary] sandbox> db.items.find({type: "fish"}, {name:1})  
[ { _id: ObjectId('6abfb3be02e697c79b13d9ea'), name: 'angelfish' } ]  

rs0 [direct: primary] sandbox> db.items.find({type: "fish"}, {name:1, "available.north":1})  
[  
 {  
   _id: ObjectId('6abfb3be02e697c79b13d9ea'),  
   name: 'angelfish',  
   available: {  
     north: {  
       May: '4 PM - 9 AM',  
       Jun: '4 PM - 9 AM',  
       Jul: '4 PM - 9 AM',  
       Aug: '4 PM - 9 AM',  
       Sep: '4 PM - 9 AM',  
       Oct: '4 PM - 9 AM'  
     }  
   }  
 }  
]  
rs0 [direct: primary] sandbox> db.villagers.insertOne({  
|   name: "Admiral", species: "Bird", personality: "Cranky", hobby: "Nature",  
|   birthday: { month: 1, day: 27 }, catchphrase: "aye aye",  
|   favoriteSong: "Steep Hill", styles: ["Cool"], colors: ["Black", "Blue"],  
|   home: { wallpaper: "dirt-clod wall", flooring: "tatami" },  
|   furniture: [717, 1849, 7047, 2736, 787, 5970, 3449, 3622, 3802, 4106, 3438, 4029]  
| })  
| db.villagers.insertOne({  
|   name: "Boone", species: "Gorilla", personality: "Jock", hobby: "Fitness",  
|   birthday: { month: 9, day: 12 }, catchphrase: "baboom",  
|   favoriteSong: "K.K. Rally", styles: ["Active"], colors: ["Blue", "Black"],  
|   home: { wallpaper: "kitchen wall", flooring: "brown wood-block flooring" },  
|   furniture: [717, 4029, 3449]  
| })  
{  
 acknowledged: true,  
 insertedId: ObjectId('6abfb64a02e697c79b13d9ed')  
}  
rs0 [direct: primary] sandbox> db.villagers.find()  
[  
 {  
   _id: ObjectId('6abfb64a02e697c79b13d9ec'),  
   name: 'Admiral',  
   species: 'Bird',  
   personality: 'Cranky',  
   hobby: 'Nature',  
   birthday: { month: 1, day: 27 },  
   catchphrase: 'aye aye',  
   favoriteSong: 'Steep Hill',  
   styles: [ 'Cool' ],  
   colors: [ 'Black', 'Blue' ],  
   home: { wallpaper: 'dirt-clod wall', flooring: 'tatami' },  
   furniture: [  
      717, 1849, 7047,  
     2736,  787, 5970,  
     3449, 3622, 3802,  
     4106, 3438, 4029  
   ]  
 },  
 {  
   _id: ObjectId('6abfb64a02e697c79b13d9ed'),  
   name: 'Boone',  
   species: 'Gorilla',  
   personality: 'Jock',  
   hobby: 'Fitness',  
   birthday: { month: 9, day: 12 },  
   catchphrase: 'baboom',  
   favoriteSong: 'K.K. Rally',  
   styles: [ 'Active' ],  
   colors: [ 'Blue', 'Black' ],  
   home: { wallpaper: 'kitchen wall', flooring: 'brown wood-block flooring' },  
   furniture: [ 717, 4029, 3449 ]  
 }  
]  
rs0 [direct: primary] sandbox> db.villagers.find({furniture: 717})  
[  
 {  
   _id: ObjectId('6abfb64a02e697c79b13d9ec'),  
   name: 'Admiral',  
   species: 'Bird',  
   personality: 'Cranky',  
   hobby: 'Nature',  
   birthday: { month: 1, day: 27 },  
   catchphrase: 'aye aye',  
   favoriteSong: 'Steep Hill',  
   styles: [ 'Cool' ],  
   colors: [ 'Black', 'Blue' ],  
   home: { wallpaper: 'dirt-clod wall', flooring: 'tatami' },  
   furniture: [  
      717, 1849, 7047,  
     2736,  787, 5970,  
     3449, 3622, 3802,  
     4106, 3438, 4029  
   ]  
 },  
 {  
   _id: ObjectId('6abfb64a02e697c79b13d9ed'),  
   name: 'Boone',  
   species: 'Gorilla',  
   personality: 'Jock',  
   hobby: 'Fitness',  
   birthday: { month: 9, day: 12 },  
   catchphrase: 'baboom',  
   favoriteSong: 'K.K. Rally',  
   styles: [ 'Active' ],  
   colors: [ 'Blue', 'Black' ],  
   home: { wallpaper: 'kitchen wall', flooring: 'brown wood-block flooring' },  
   furniture: [ 717, 4029, 3449 ]  
 }  
]  
rs0 [direct: primary] sandbox> db.villagers.find({"birthday.month":9}, {name:1}, {birthday:1}  
)  
[ { _id: ObjectId('6abfb64a02e697c79b13d9ed'), name: 'Boone' } ]  
rs0 [direct: primary] sandbox> db.villagers.find({"birthday.month":9}, {name:1,birthday:1})  
[  
 {  
   _id: ObjectId('6abfb64a02e697c79b13d9ed'),  
   name: 'Boone',  
   birthday: { month: 9, day: 12 }  
 }  
]  
rs0 [direct: primary] sandbox>
  
rs0 [direct: primary] sandbox> db.villagers.find({"available":{$exists: true}})  
  
rs0 [direct: primary] sandbox> db.items.find({"available":{$exists: true}})  
[  
 {  
   _id: ObjectId('6abfb3be02e697c79b13d9ea'),  
   name: 'angelfish',  
   type: 'fish',  
   sell: 3000,  
   where: 'River',  
   shadow: 'Small',  
   available: {  
     north: {  
       May: '4 PM - 9 AM',  
       Jun: '4 PM - 9 AM',  
       Jul: '4 PM - 9 AM',  
       Aug: '4 PM - 9 AM',  
       Sep: '4 PM - 9 AM',  
       Oct: '4 PM - 9 AM'  
     }  
   }  
 }  
]  
rs0 [direct: primary] sandbox> db.items.find({"available.north":{$exists: true}})  

[  
 {  
   _id: ObjectId('6abfb3be02e697c79b13d9ea'),  
   name: 'angelfish',  
   type: 'fish',  
   sell: 3000,  
   where: 'River',  
   shadow: 'Small',  
   available: {  
     north: {  
       May: '4 PM - 9 AM',  
       Jun: '4 PM - 9 AM',  
       Jul: '4 PM - 9 AM',  
       Aug: '4 PM - 9 AM',  
       Sep: '4 PM - 9 AM',  
       Oct: '4 PM - 9 AM'  
     }  
   }  
 }  
]  

rs0 [direct: primary] sandbox> db.items.find({"available.south":{$exists: true}})
```
``` json
rs0 [direct: primary] sandbox> db.villagers.updateOne({name: "Boone"}, {$push: {furniture: 67  
}})  
{  
 acknowledged: true,  
 insertedId: null,  
 matchedCount: 1,  
 modifiedCount: 1,  
 upsertedCount: 0  
}  
rs0 [direct: primary] sandbox>

```

