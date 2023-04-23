# Spell 技能

> 所有的函数在使用时都会以`ni.spell.`作为前缀。

---

## available 可用

参数:

- **spell** `id|string` _技能id 或 名称_
- **stutter** `boolean` _default: true_ _默认：false_

Returns: `boolean` _返回值：类型：布尔值（true or false）_

检查指定的法术是否可以使用。包括检查，如：

- [`ni.spell.gcd`](api/spell.md#gcd)
- [`ni.vars.combar.casting`](api/vars.md)
- [`ni.spell.cd`](api/spell.md#cd)
- [`ni.stopcastingtracker.shouldstop`](api/stopcasting.md)
- [`ni.player.powerraw`](api/player.md)
- [`ni.player.hpraw`](api/player.md)

!> [`ni.spell.available`](api/spell.md#available) 与 [`ni.spell.valid`](api/spell.md#valid)是不同的.

```lua
if ni.spell.available(686) then
  -- 暗影箭 通过了所有的检查，可以使用。
end
```

## cast 施法

参数:

- **spell** `id|string` _技能id 或 名称_
- **target** `token|guid`

Returns: `void`

施放指定的法术。如果提供了目标，将对该目标施放，否则将对自己施放。

```lua
ni.spell.cast("Shadow Bolt", "target")
```

## delaycast 延迟施法

参数:

- **spell** `id|string` _技能id 或 名称_
- **target** `token|guid`
- **delay** `number` _optional_

Returns: `boolean` _返回值：类型：布尔值（true or false）_

就像施放一样，但是你可以指定延迟，如果距离上次施放技能的时间超过了延迟，它就会施放技能，同时返回`true`。如果低于延迟时间，该函数将返回`false`。

```lua
if ni.spell.delaycast("Shadow Bolt", "target", 1.5) then
	--距离我们上次施放已经超过了1.5秒，所以它施放了。
else
	--技能没有施放，因为距离上次施放的时间少于1.5秒。
end
--其他用法与普通施放一样，只是为了确保在延迟时间内不会施放。
if true then
	ni.spell.delaycast("Shadow Bolt", "target", 1.5)
end
```

## bestaoeloc 最佳AOE地点

- **distance** `number` _数值_
- **radius** `number` _数值_
- **friendly** `boolean` _optional_ _布尔值_
- **minimumcount** `number` _optional_ _数值_
- **inc** `number` _optional_ _数值_
- **zindex_inc** `number` _optional_ _数值_

Returns: `X/Y/Z` _返回：坐标：x 坐标, y 坐标,z 坐标_

这个函数使用内部检查来扫过玩家周围，以搜索最佳的X/Y/Z坐标来放置一个AOE。
**distance**和**radius**是唯一需要的两项，其余的是可选的。
**friendly**是检查你可以协助的单位（如果是真的），或者你可以攻击的单位（如果是假的）（默认：假的）。 
**minimumcount**是半径内被算作好位置的最小单位数（默认：2）。
**inc**是用来做增量循环的，数字越大，扫描的效率越低，但完成得越快；
例如，如果距离是30，增量是1。 zindex_inc是用于重新调整，为每个被检查的位置获得一个新的Z，意思是地面实际所在的点，每个点使用玩家Z的+增量和-增量，并使用击中的位置作为新的Z（默认：20）。

```lua
local x, y, z = ni.spells.bestaoeloc(30, 4, false, 6);
--如果没有好的位置，这将返回nil
--否则x、y、z将是最佳位置，至少可以击中一个位置内的6个目标
--距离玩家至少30码，并且AoE半径为4码的位置。
```

## castharmfulatbest

- **spell** `id|string`
- **distance** `number`
- **radius** `number`
- **minimumcount** `number` _optional_
- **inc** `number` _optional_
- **zindex_inc** `number` _optional_

Returns: `void`

This function will cast the spell specified at the best location matching the requirements. See above for what each argument is.

```lua
ni.spell.cast("Hurricane", 36, 4, 4); --On a druid this would cast hurricane at the best location within 36 yards of the player that has at least 4 mobs to be hit
```

## casthelpfulatbest

- **spell** `id|string`
- **distance** `number`
- **radius** `number`
- **minimumcount** `number` _optional_
- **inc** `number` _optional_
- **zindex_inc** `number` _optional_

Returns: `void`

This function will cast the spell specified at the best location matching the requirements. See above for what each argument is.

```lua
ni.spell.cast("Healing Rain", 36, 4, 5); --On a shaman this would cast healing rain at the best location within 36 yards of the player that has at least 5 friendlies to be hit by the heal
```

## castat

参数:

- **spell** `id|string`
- **target** `token|guid|mouse`
- **offset** `number`

Returns: `void`

Casts specified spell which required click on the ground (e.g. Death and Decay, Rain of Fire, Blizzard).

```lua
ni.spell.castat("Rain of Fire", "target")
```

## castatqueue

参数:

- **spell** `id|string`
- **target** `token|guid|mouse`

Returns: `void`

Queues a specified spell to be casted on the ground once it's available.

```lua
ni.spell.castatqueue("Blizzard", "target")
```

## castqueue

参数:

- **spell** `id|string`
- **target** `token|guid`

Returns: `void`

Queues a specified spell to be casted once it's available.

```lua
ni.spell.castqueue("Fear", "target")
```

## castspells

参数:

- **spell** `id|string`
- **target** `token|guid`

Returns: `void`

Casts specified spells separated by pipe (`|`). If the target is provided it'll cast on that target, otherwise spells wll be casted on self. Does not work if spells more than one spell triggers global cooldown.

```lua
ni.spell.castspells("Heroic Strike|Bloodthirst", "target")
```

## casttime

参数:

- **spell** `id|string`

Returns: `number`

Calculates the cast time of specified spell.

```lua
local casttime = ni.spell.casttime("Immolate") -- 1.25
```

## cd

参数:

- **spell** `id|string`

Returns: `number`

Calculates specified spell's cooldown. If the spell is not on cooldown returns 0.

```lua
if not ni.spell.cd(47891) then
  -- Shadow Ward is not on cooldown
end
```

## gcd

参数:

Returns: `boolean`

Checks if global cooldown is triggered.

```lua
if not ni.spell.gcd() then
  -- Global cooldown is not active, we can do something
end
```

## id

参数:

- **spellname** `string`

Returns: `number|nil`

Converts spell's name into spell id. If spell doesn't exist returns nil.

```lua
local spellid = ni.spell.id("Life Tap") -- 57946
```

## isinstant

参数:

- **spell** `string|id`

Returns: `boolean`

Checks if passed spell is instant cast.

```lua
if ni.spell.isinstant(57946) then
  -- Life Tap is instant spell
end
```

## stopcasting

参数:

Returns: `void`

Stops casting.

```lua
ni.spell.stopcasting()
```

## stopchanneling

参数:

Returns: `void`

Stops channeling.

```lua
ni.spell.stopchanneling()
```

## valid

参数:

- **spell** `id|string`
- **target** `token|guid`
- **facing** `boolean` _default: false_
- **los** `boolean` _default: false_
- **friendly** `boolean` _default: false_

Returns: `boolean`

This functions ensures that a spell can be casted at specific target. It includes checks such as:

!> [`ni.spell.valid`](api/spell.md#valid) is not the same as [`ni.spell.available`](api/spell.md#available).

- [`ni.unit.exists`](api/unit.md#exists)
- [`ni.player.los`](api/player.md)
- [`ni.player.isfacing`](api/player.md)

```lua
if ni.spell.valid("Fear", "target") then
  -- Fear meets all of the critera to be valid
end
```
