# The Heart of Light

As dawn broke over the ruins of Castle Town, Spojínek heard the soft notes of an ocarina for the first time in years. They drifted from the crumbling gates of the Temple of Time, where he had once opened a passage to the past. The air smelled of dust, but ancient magic stirred among the stones: the call of the goddess Světlička.

“The Heart of Light,” Zářava whispered, handing him a map, torn and blackened with age. “An artifact that can awaken a sleeping world. But its path has been lost to time. Find it, and restore the balance.”

The temple was not as Spojínek remembered it. Echoes of the past wandered its halls, along with the shadows of knights who had served there long ago. On the altar lay the Master’s Broken Sword, an ancient blade split in two. The moment he lifted it, the world around him changed. A portal opened into the depths beneath the temple, where no Světličan had ever set foot.

The first chamber was cold and dark. Rusted armor littered the floor, and among it pulsed Kostěj, a risen warrior with eyes like burning coals. It lunged at him without hesitation. Spojínek raised the broken sword; in his hands, it glimmered faintly, as if it recognized its former master. Every strike echoed battles long past. When Kostěj fell, an Emerald Buckle remained—a talisman that strengthened its wearer’s defense. Spojínek fastened it to his tunic and pressed on.

The next passage led through stone gates into the Lost Woods, shifting with every step. There, the Skull Kid waited, a mischievous creature wearing a bark mask. “Looking for treasure, are you? But to find the light, you must first pass through darkness.” It played a few notes on its flute, and the trees parted. Spojínek followed until he found the Mask of Silence, an old relic that let him slip past monsters unseen. As he put it on, the world around him fell quiet; only the wind whispered his name.

The path curved back toward the temple, but this time it led deeper than ever before. A muffled throbbing filled the air, perhaps an echo of the Heart of Light itself. Runes covered the vaults and glowed as he passed. Every step brought him closer to the temple’s core, where the goddess’s treasure was said to rest.

But the light he saw in the distance was not still. It trembled and flickered, and the outline of a figure emerged within it—dark, yet familiar. Spojínek gripped the sword’s hilt. The blade shuddered, as if it too recognized the one standing before them.

Dark Spojínek stepped out of the shadows: his own reflection, given form by the temple’s deepest mirror. In silence, it raised its weapon.

Spojínek stopped. His heart beat in time with the light, which called him forward even as it pulled him back. Stone slabs shifted overhead. The temple breathed. And time came undone.

Everything fell silent.

Spojínek took a step forward. The world shook, and the gate to the Heart of Light opened. For a moment, he glimpsed a radiance so brilliant he forgot to breathe.

Then the ocarina sounded again—the same tune that had once brought him here. Before the world could break apart into shadow and light, Spojínek understood: the journey was only beginning.

## Assignment

Implement `find_shortest_path`, which finds the shortest walk from any entrance to the treasure and back to any entrance that the hero can survive. Return an empty vector if no such walk exists.

### Function Parameters

- `const std::vector<Room>& rooms` — all rooms in the maze. Each `Room` contains:
  - `std::vector<RoomId> neighbors` — indices into `rooms` for rooms reachable by a corridor from this room.
  - `std::optional<Monster> monster` — the monster in the room, if any. A `Monster` has integer attributes `hp`, `off`, and `def` for hit points, attack, and defense, plus `stacking_off` and `stacking_def` for the changes to its attack and defense after each of its turns.
  - `std::vector<Item> items` — the items in the room (see [Items](#items)). If a room contains a monster, the hero can collect its items only after defeating it.
- `const std::vector<RoomId>& entrances` — indices into `rooms` that can be used to enter or leave the maze.
- `RoomId treasure` — the index of the room containing the treasure the hero must collect.

### Combat and Room Rules

Entering a room with a monster starts a fight. Combat ends only when one combatant dies; if neither can damage the other, they remain locked in combat forever. The monster attacks first, followed by the hero, then the monster again, and so on. Each attack deals `max(0, attacker_off - target_def)` damage. A combatant dies when their hit points fall to zero or below.

The provided `simulate_combat` function is efficient enough for full credit, provided the rest of the solution is reasonably efficient.

Corridors are very long, so time spent fighting and exchanging items can be ignored. The hero is fully healed when traveling to the next room. Whenever the hero revisits a room, its monster and all its items are restored.

### Actions

The return type is `std::vector<Action>`, a sequence of actions that starts and ends in an entrance room. The starting and ending entrances do not have to be the same. `Action` is `std::variant<Move, Pickup, Drop>`:

- `Move` moves to the specified room, which must be adjacent to the current room.
- `Pickup` collects an item in the current room. If the hero already carries an item of the same type, that item is automatically dropped.
- `Drop` discards the carried item of the specified type.

### Items

An `Item` has the following attributes:

- `Type type` — the item category: weapon, armor, or rubber duck. The hero can carry at most one item of each type at a time. Items can be dropped at any time outside combat. A dropped item is lost and cannot be picked up again unless another copy is found in a room.
- `std::string name` — the item’s name, which is not necessarily unique.
- `int hp`, `off`, `def` — the changes to the hero’s hit points, attack, and defense while carrying the item. Bonuses can be negative, and the resulting attack or defense can also be negative. If the hero’s maximum hit points would be non-positive, they instead have 1 hit point.
- `int stacking_off`, `stacking_def` — the changes to the hero’s attack and defense after each of their turns. These effects last until the current combat ends.
- `bool first_attack` — if `true`, the hero attacks first in combat instead of the monster.
- `bool stealth` — if `true`, the hero may choose to sneak through a room and avoid fighting its monster. While sneaking, the hero cannot collect items, including the treasure.

There are few distinct items in the maze, but the same item may appear in many rooms. Without equipment, the hero has 10,000 hit points, an attack of 3, and a defense of 2.