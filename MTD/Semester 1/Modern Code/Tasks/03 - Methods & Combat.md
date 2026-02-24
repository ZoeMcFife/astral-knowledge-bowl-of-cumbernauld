#modern_code #methods #combat #game_development

![[MC_UE03_BUNEA 1.pdf]]
## 📚 Assignment Reference

![[combat_system_guide.pdf]]
![[A03_Methods_Combat.pdf]]

---

## 🎮 Game Overview

This is a comprehensive **turn-based combat RPG system** implemented in Java. The game features a deep combat system with character progression, inventory management, equipment, special attacks, and dynamic enemy encounters. The project demonstrates modular design, method usage, and top-down architecture planning.

### Key Features
- ✅ Full turn-based combat system
- ✅ Character creation and stat allocation
- ✅ Equipment system (Weapons, Armor, Shields)
- ✅ Inventory management with weight limits
- ✅ Power Points (PP) system for special attacks
- ✅ Dynamic enemy generation with 5 danger levels
- ✅ Experience and leveling system
- ✅ Multiple difficulty modes
- ✅ Factory pattern for item/enemy creation
- ✅ 117+ unique items and enemies

---

## 🏗️ System Architecture

### Package Structure
```
src/main/
├── Main.java                 # Entry point
├── character/                # Character classes
│   ├── GameCharacter.java    # Base character class
│   ├── Player.java           # Player character
│   ├── Enemy.java            # Enemy character
│   ├── CharacterStatus.java  # Health status enum
│   └── DangerLevel.java      # Enemy difficulty levels
├── combat/                   # Combat system
│   ├── Battle.java           # Battle management
│   └── ActionType.java       # Available actions
├── item/                     # Item system
│   ├── Item.java             # Base item class
│   ├── Weapon.java           # Weapon items
│   ├── Shield.java           # Shield items
│   ├── Armour.java           # Armor items
│   ├── HealingPotion.java    # Healing items
│   ├── ItemRarity.java       # Item rarity levels
│   └── ArmourState.java      # Equipment state
├── inventory/                # Inventory management
│   └── Inventory.java        # Inventory system
├── factory/                  # Factory pattern
│   ├── baseFactories/        # Item/Enemy factories
│   │   ├── EnemyFactory.java
│   │   ├── WeaponFactory.java
│   │   ├── ShieldFactory.java
│   │   ├── ArmourFactory.java
│   │   └── HealingPotionFactory.java
│   └── generators/           # Battle/Enemy generators
│       ├── BattleGenerator.java
│       └── EnemyGenerator.java
├── ui/                       # User interface
│   ├── UserInterface.java    # Base UI class
│   ├── UIHelper.java         # UI utilities
│   └── components/           # UI screens
│       ├── main_menu/        # Main menu screens
│       ├── battle/           # Battle UI screens
│       ├── character/        # Character management
│       └── inventory/        # Inventory UI
└── global/                   # Global configuration
    ├── GameManager.java      # Game state manager
    └── Difficulty.java       # Difficulty settings
```

---

## 👤 Character System

### GameCharacter (Base Class)
The abstract base class for all characters in the game.

**Core Stats:**
- **Health** - Current and maximum HP (dies at 0)
- **Strength** (1-10) - Affects physical weapon damage and carry capacity
- **Dexterity** (1-10) - Affects turn order and dodge chance
- **Intelligence** (1-10) - Affects magical weapon damage

**Equipment Slots:**
- Weapon (default: Fists)
- Shield (default: Fists)
- Armor (default: Clothes)

**Key Methods:**
```java
// Combat actions
void attack(GameCharacter target)
void defend()
void useHealingItem()
void useSpecial(GameCharacter target)

// Equipment management
void equipItem(Item item, boolean displayMessage, boolean ignoreRestrictions)
void unequipWeapon()
void unequipShield()
void unequipArmour()

// Health management
void heal(double amount)
void takeDamage(double damage)
boolean isAlive()

// Inventory
void addItemToInventory(Item item)
int getCarryCapacity()  // strength * 5
```

### Player Class
Extends GameCharacter with player-specific features.

**Additional Stats:**
- **Level** - Character level (starts at 1)
- **Experience** - XP toward next level
- **Available Stat Points** - Points to spend on stats
- **Current PP / Max PP** - Power Points for special attacks

**Key Features:**
```java
// Leveling system
void addExperience(int amount)
void levelUp()
void allocateStatPoint(String stat)

// Power Points
void gainPP(int amount)
boolean hasEnoughPP(int cost)

// Default constants (class-level)
static double DEFAULT_PLAYER_MAX_HEALTH = 100.0
static int DEFAULT_PLAYER_MAX_PP = 100
```

### Enemy Class
Extends GameCharacter with AI behavior.

**Properties:**
- `experienceReward` - XP given to player on death
- AI-driven action selection

**AI Behavior:**
```java
// Enemy decides actions based on health
if (health < 30% && hasHealingItem)
    action = USE_ITEM
else
    action = random(ATTACK: 90%, DEFEND: 10%)
```

### Character Status Enum
Health status indicators:
- `HEALTHY` - Above 70% HP
- `WOUNDED` - 30-70% HP
- `CRITICAL` - Below 30% HP (deals 30% less damage)
- `DEAD` - 0 HP

---

## ⚔️ Combat System

### Battle Flow

```
┌─────────────────────────────────────┐
│  1. Battle Initialization           │
│     - Generate enemies              │
│     - Order by dexterity            │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│  2. Turn Start                      │
│     - Display turn number           │
│     - Show all combatants           │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│  3. Thinking Phase                  │
│     - Player selects action         │
│     - Enemies AI chooses action     │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│  4. Defense Phase                   │
│     - Apply defense bonuses         │
│     - Grant PP for defending        │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│  5. Action Phase                    │
│     - Execute in dexterity order    │
│     - Process attacks/items/special │
└──────────────┬──────────────────────┘
               │
┌──────────────▼──────────────────────┐
│  6. Cleanup Phase                   │
│     - Remove defense bonuses        │
│     - Check for deaths              │
└──────────────┬──────────────────────┘
               │
               ▼
     Battle Over? ──No──► Next Turn
         │
        Yes
         │
         ▼
     Victory/Defeat
```

### Actions

#### 1. **Attack**
- Deals damage based on weapon and stats
- **Physical weapons** scale with Strength
- **Magical weapons** scale with Intelligence
- Damage reduced by target's armor + defense

```java
// Damage calculation
baseDamage = weapon.damage
statMultiplier = (weapon.isMagic ? intelligence : strength) * multiplier
finalDamage = (baseDamage * (1 + statMultiplier)) - targetDefense
```

#### 2. **Defend**
- Grants defense bonus for one turn
- **Players:** Gain PP based on equipped shield
- Defense resets at end of turn

```java
// Defense bonus
defense += equippedArmor.defense + equippedShield.defense

// PP gain (Player only)
if (isDefending)
    gainPP(equippedShield.ppGain)
```

#### 3. **Use Item**
- Consume healing potion from inventory
- Restores health based on potion strength
- Cannot exceed max health

#### 4. **Use Special** (Player Only)
- Requires sufficient PP
- Deducts PP cost
- Deals enhanced damage
- Shows unique flavor text

```java
// Special attack
if (currentPP >= weapon.ppCost)
{
    currentPP -= weapon.ppCost
    damage = weapon.damage + weapon.specialDamage
    // Apply stat multipliers
}
```

### Turn Order
Characters act in **descending dexterity order** (highest goes first):
```java
participants.sort((a, b) -> b.getDexterity() - a.getDexterity())
```

---

## 🎒 Item System

### Item Rarity
All equipment has rarity affecting power and cost:
- **LOW** - Common items (low stats)
- **MEDIUM** - Uncommon items (moderate stats)
- **HIGH** - Rare items (high stats)
- **LEGENDARY** - Extremely rare (highest stats)

### Weapon
**Properties:**
- `damage` - Base damage value
- `isMagic` - True if scales with Intelligence
- `specialDamage` - Bonus damage for special attacks
- `specialFlavorText` - Message shown when using special
- `ppCost` - PP required to use special

**Examples:**
```java
// Low rarity physical
Weapon("Rusty Dagger", LOW, damage: 14, magic: false, 
       specialDmg: 15, ppCost: 20)

// Legendary magical
Weapon("Warpspike Lance", LEGENDARY, damage: 62, magic: true,
       specialDmg: 70, ppCost: 55)
```

### Shield
**Properties:**
- `defense` - Damage reduction when defending
- `ppGain` - PP granted when defending

**By Rarity:**
- LOW: 3-5 PP per defend
- MEDIUM: 8-9 PP per defend
- HIGH: 11-12 PP per defend
- LEGENDARY: 15 PP per defend

### Armour
**Properties:**
- `defense` - Passive damage reduction

**Ranges:**
- LOW: 6-10 defense
- MEDIUM: 12-18 defense
- HIGH: 20-30 defense
- LEGENDARY: 35-40 defense

### Healing Potion
**Properties:**
- `healAmount` - HP restored on use

**Types:**
- Emergency Tonic (10 HP)
- Standard Salve (15 HP)
- Greater Brew (25 HP)
- Restorative Elixir (35 HP)
- Essence of Vitality (50 HP)

---

## 📦 Inventory System

### Features
- **Weight-based capacity**: `strength × 5`
- **Item management**: Add, remove, drop, use
- **Equipment handling**: Quick equip/unequip
- **Item categorization**: Weapons, shields, armor, consumables

### Key Operations
```java
// Adding items
inventory.addItem(item)  // Respects weight limit

// Using items
inventory.useItem(item)  // Consumes healing potions

// Equipment
character.equipItem(weapon)
character.unequipWeapon()

// Weight check
if (inventory.getWeight() + item.weight <= carryCapacity)
    // Can carry
```

---

## 🏭 Factory System

### Purpose
Centralized creation of items and enemies with predefined stats.

### Available Factories

#### EnemyFactory
**50 unique enemies** across 5 danger levels

```java
// Create specific enemy
Enemy boss = EnemyFactory.createEnemyById(10);
Enemy siren = EnemyFactory.createEnemyByName("Void Siren");

// Random by danger level
Enemy enemy = EnemyFactory.createRandomEnemyByDangerLevel(
    DangerLevel.EXTREME
);

// All danger levels
HARMLESS          // Very weak
MOSTLY_HARMLESS   // Weak
DANGEROUS         // Moderate
EXTREME           // Strong
DEATH             // Boss-level
```

#### WeaponFactory
**23 unique weapons** with varying stats

```java
// Specific weapon
Weapon sword = WeaponFactory.createWeaponByName("Starcore Blade");

// Random by rarity
Weapon legendary = WeaponFactory.createRandomWeaponByRarity(
    ItemRarity.LEGENDARY
);
```

#### ShieldFactory
**6 unique shields**

```java
Shield shield = ShieldFactory.createShieldByName("Echo Shard Shield");
Shield randomShield = ShieldFactory.createRandomShield();
```

#### ArmourFactory
**17 unique armour pieces**

```java
Armour armor = ArmourFactory.createRandomArmourByRarity(
    ItemRarity.HIGH
);
```

#### HealingPotionFactory
**21 unique healing items**

```java
HealingPotion potion = HealingPotionFactory.createPotionByName(
    "Emergency Tonic"
);
```

### Factory Methods Pattern
All factories support:
- `createById(int id)` - Specific item by ID
- `createByName(String name)` - Case-insensitive lookup
- `createRandom()` - Random selection
- `createRandomBy[Category]()` - Filtered random

---

## ⚡ Power Points (PP) System

### Overview
Special attack resource management system.

### Gaining PP
**Defend action** grants PP based on equipped shield:
```
LOW shield:       3-5 PP
MEDIUM shield:    8-9 PP
HIGH shield:      11-12 PP
LEGENDARY shield: 15 PP
```

### Using PP
**Special attacks** consume PP based on weapon:
```
LOW weapon:       20-25 PP cost
MEDIUM weapon:    30-35 PP cost
HIGH weapon:      40-45 PP cost
LEGENDARY weapon: 50-55 PP cost
```

### Battle Strategy
```
Turn 1: Defend → +8 PP
Turn 2: Defend → +8 PP (Total: 16)
Turn 3: Attack → Chip damage
Turn 4: Defend → +8 PP (Total: 24)
Turn 5: Use Special → Spend 24 PP for massive damage!
```

### Implementation
```java
// Player class
private int currentPP = 0;
private int maxPP = 100;

public void gainPP(int amount) {
    currentPP = Math.min(currentPP + amount, maxPP);
}

public void useSpecial(GameCharacter target) {
    if (currentPP >= weapon.ppCost) {
        currentPP -= weapon.ppCost;
        // Deal enhanced damage
    }
}
```

---

## 📊 Progression System

### Experience & Leveling

**Experience Gain:**
- Defeating enemies grants XP
- XP amount based on enemy difficulty

**Level Up Benefits:**
- +70 max HP
- +35 max PP
- +1 stat point to allocate
- Full HP/PP restore

**Stat Allocation:**
Players can spend points on:
- **Strength** → +18 max HP, +physical damage, +carry capacity
- **Intelligence** → +10 max HP, +magic damage
- **Dexterity** → +10 max HP, +dodge chance, faster turn order

*Note: While Intelligence and Dexterity provide the same HP bonus (+10), they have different primary combat benefits.*

### Health Scaling
```java
maxHealth = 100  // Base
    + (level * 70)
    + (strength * 18)
    + (intelligence * 10)
    + (dexterity * 10)
```

---

## 🎲 Difficulty System

### Modes
1. **Easy**
   - More turns before difficulty increase (12)
   - More loot items (8)

2. **Medium** (Balanced)
   - Moderate turn threshold (6)
   - Standard loot (5)

1. **Hard**
   - Quick difficulty scaling (3)
   - Limited loot (3)

---

## 🎨 User Interface

### Main Menu
- New Game
- Load Game (if implemented)
- Settings
- Exit

### Character Creation
1. Enter name
2. Allocate starting stats
3. Select difficulty

### Battle Screen
```
=================== TURN 3 ===================

ENEMIES:
[1] Shadow Wraith (HP: 45/80) - WOUNDED

PLAYER: Hero (HP: 78/100) - HEALTHY
Current PP: 16/100

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

What will you do?
1. Attack
2. Defend (Gain PP)
3. Use Item
4. Use Special (Cost: 30 PP)
> _
```

### Inventory UI
- Display all items
- Filter by type
- Equip/Use/Drop items
- Show weight: `25/50`

---

## 🔧 Game Balance Constants

Located in `GameManager.java`:

```java
// Damage scaling
DAMAGE_MULTIPLIER_PER_STRENGTH = 0.06
DAMAGE_MULTIPLIER_PER_INTELLIGENCE = 0.1

// Defense
PLAYER_BASE_DEFENCE = 11
DODGE_CHANCE_PER_DEXTERITY = 0.021

// Carry capacity
CARRY_CAPACITY_PER_STRENGTH = 5

// Status effects
DAMAGE_REDUCTION_WHEN_CRITICAL_STATUS = 0.3

// Leveling
STAT_POINTS_PER_LEVEL = 1
HEALTH_INCREASE_PER_LEVEL = 70
MAX_PP_INCREASE_PER_LEVEL = 35
```

---

## 💡 Design Patterns Used

### 1. **Factory Pattern**
Centralized object creation with consistent interfaces:
- EnemyFactory
- WeaponFactory
- ShieldFactory
- ArmourFactory
- HealingPotionFactory

### 2. **Template Method**
`GameCharacter` defines combat flow, subclasses implement specifics:
```java
abstract class GameCharacter {
    abstract void onDeath();  // Player vs Enemy differ
}
```

### 3. **Strategy Pattern**
Different AI behaviors for enemies based on state

### 4. **Singleton Pattern**
`GameManager` maintains single game state instance

### 5. **Observer Pattern**
UI components observe character state changes

---

## 🧪 Code Quality Features

### Modularity
- Single Responsibility Principle
- Each class has focused purpose
- Clear separation of concerns

### Method Design
```java
// Good: Single purpose
private void applyDamage(GameCharacter target, double amount)

// Good: Clear parameters
public Battle(List<Enemy> enemies, Player player)

// Good: Validation
public void setStrength(int strength) {
    if (strength < MIN_STAT_VALUE || strength > MAX_STAT_VALUE)
        throw new IllegalArgumentException();
    this.strength = strength;
}
```

### Documentation
- Comprehensive JavaDoc comments
- Clear parameter descriptions
- Usage examples in factory README

---

## 🎯 Learning Objectives Met

### ✅ Methods & Modularity
- Extracted combat logic into methods
- Avoided monolithic `main()` method
- Clear method responsibilities

### ✅ Top-Down Design
- Planned architecture before implementation
- Modular component structure
- Iterative development

### ✅ Object-Oriented Principles
- Inheritance (GameCharacter → Player/Enemy)
- Encapsulation (private fields, public methods)
- Polymorphism (different character behaviors)

### ✅ Advanced Features Implemented

**Exercise 2 Extensions:**
1. **Special Moves with Cooldowns** ✅
   - PP system limits special usage
   - Must defend to gain PP

2. **Battle Statistics Tracking** ✅
   - Damage dealt
   - XP earned
   - Level progression

3. **Improved Enemy AI** ✅
   - Health-based decision making
   - Item usage when low health
   - Random attack/defend mix

4. **Multiple Rounds / Tournament Mode** ✅
   - Continuous battle system
   - Difficulty escalation
   - Loot collection

5. **Difficulty Levels** ✅
   - Easy/Medium/Hard modes
   - Different loot amounts
   - Variable scaling

6. **Turn Order / Speed Initiative** ✅
   - Dexterity-based turn order
   - Faster characters go first

---

## 📁 File Statistics

**Total Components:**
- 46 Java files in main package
- 117+ unique items/enemies
- 50 enemies across 5 danger levels
- 23 weapons
- 6 shields
- 17 armor pieces
- 21 healing potions

**Architecture:** Modular, object-oriented design with comprehensive test coverage

---

## 🚀 Running the Game

### Compilation
```bash
javac -d out -sourcepath src src/main/Main.java
```

### Execution
```bash
java -cp out main.Main
```

### Running Tests
```bash
javac -d out -sourcepath src src/test/FactoryDemo.java
java -cp out test.FactoryDemo
```

---

## 📝 Sample Playthrough Output

```
==========================================
         WELCOME TO BATTLE ARENA
==========================================

Enter your character name: Hero

Allocate your starting stats (10 points total)
Strength: 4
Dexterity: 3
Intelligence: 3

Select Difficulty:
1. Easy
2. Medium
3. Hard
> 2

==========================================
           BATTLE START!
==========================================

A Shadow Wraith appears!

=================== TURN 1 ===================

ENEMIES:
[1] Shadow Wraith (HP: 80/80) - HEALTHY

PLAYER: Hero (HP: 100/100) - HEALTHY
Current PP: 0/100

What will you do?
1. Attack
2. Defend
3. Use Item
4. Use Special (Cost: 30 PP)
> 2

Hero takes a defensive stance!
Hero gained 8 PP!

Shadow Wraith attacks Hero!
Hero dodges the attack!

=================== TURN 2 ===================

[... battle continues ...]

Shadow Wraith has been defeated!
Hero gained 50 experience!

LEVEL UP! Hero is now level 2!
+70 Max HP
+35 Max PP
+1 Stat Point

Collect loot? (5 items available)
[... loot collection ...]

==========================================
         VICTORY!
==========================================
```

---

## 🔗 Related Resources

- **Assignment PDF:** `A03_Methods_Combat.pdf`
- **Combat Guide:** `combat_system_guide.pdf`
- **Factory Documentation:** `FACTORY_README.md`
- **PP System Guide:** `PP_SYSTEM_GUIDE.md`
- **Implementation Summary:** `IMPLEMENTATION_SUMMARY.md`

---

## 📌 Key Takeaways

1. **Modular design** makes code maintainable
2. **Factory pattern** simplifies object creation
3. **Top-down planning** prevents refactoring pain
4. **Clear method responsibilities** improve readability
5. **Game balance constants** enable easy tuning
6. **Comprehensive documentation** helps future development

---

*Created for Modern Code - Semester 1*
*Turn-Based Combat System Assignment*
