tA unit can have the following attributes

| Property                 | Default  Value | Type***                           | Purpose | Typical Range* | Note                                 |
| ------------------------ | -------------- | --------------------------------- | ------- | -------------- | ------------------------------------ |
| base_movement            | 3              | int                               |         | 1-20           |                                      |
| movement_modifier        | 1              | float                             |         | 0-10           | Multiplicative                       |
| movement_left            | [Calculated]   | float\|int                        |         |                |                                      |
| can_raid                 | True           | bool                              |         | 0,1            |                                      |
| can_move                 | True           | bool                              |         | 0,1            |                                      |
| health                   | 100            | int                               |         | -1, <br>0-500  | is infinit health**                  |
| max_health               | 100            | int                               |         | 10-9001        |                                      |
| attack                   | 10             | int                               |         | 2-10000        |                                      |
| attacks                  | 1              | int^                              |         | 1-5            |                                      |
| defense                  | 1              | float                             |         | 0-100          | Multiplicative                       |
| armor_piercing           | 1              | float                             |         |                | Multiplicative                       |
| ammunition               | -1             | int                               |         |                |                                      |
| seige_damage             | 0.2            | float                             |         |                | Multiplicative                       |
| indirect_fire            | False          |                                   |         |                |                                      |
| vision_range             | 3              | int^                              |         | 1-10           | 0 = current tile only \| Tile Radius |
| attack_range             | 1              | int^                              |         | 1-30           | 1 = mele, >=2 range \| Tile Radius   |
| weapons                  | Empty          | List[Item]                        |         |                | Split for easy calculations          |
| armor                    | Empty          | List[Item]                        |         |                | ^                                    |
| support                  | Empty          | List[Item]                        |         |                | ^                                    |
| items                    | Empty          | List[Item]                        |         |                | ^                                    |
| unit_class               | Empty          | Subclass - Unitbaseclass          |         |                |                                      |
| promotion_tree_ref       |                | Subclass Type - PromotionTree     |         |                |                                      |
| promotion_tree           |                | Subclass Instance - PromotionTree |         |                |                                      |
| effects                  |                | List[Effects]                     |         |                |                                      |
| attackable_from_planes   |                | List[Planes]                      |         |                |                                      |
| can_attack_plane         |                | List[Planes]                      |         |                |                                      |
| upgrades_to              |                | Unit                              |         |                |                                      |
| upgrades_from_class      | true           | bool                              |         |                |                                      |
| upgrade_tier             | 0              | int                               |         |                |                                      |
| upgrade_into_requirement | Empty          | Requirements                      |         |                |                                      |
<footer>
-  \* As the game is dynamic and mod-able so these numbers could be vastly different. this is more for a reference on what numbers are normal.
- \** This is not really used anywhere as nothing so far needs to be something with infinite health but will just leave the logic in.
- \*** Signed otherwise `^` is suffixed to the type
</footer>

## Calculations 
### Land/Sea to Land/Sea:
Due to land and sea having both to also deal with the calculations of terrain we just group these together. 

Offensive when an attack is outgoing(attacking with unit), and Defensive is an incoming (being attack with an unit).

Retaliation is when a unit can retaliate against a other unit they don't use the same math as retaliation can have different modifiers based on unit class and type and even ability.

#### Terrain
Terrain can give effects and raw combat status effects but for this we will just take into an count the effects
#### Basic
```
U.attack - ((1 / U.armor_piercing) * E.defense)
```
#### Retaliation
`fortified_bonus` is `1` by default and `1.5` when unit is fortified.
```
(fortified_bonus + retaliation_bonus) * (U.attack - ((1 / U.armor_piercing) * E.defense))
```
#### Siege
```
siege_damage + (U.attack - ((1 / U.armor_piercing) * E.defense))
```