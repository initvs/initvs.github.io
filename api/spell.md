# Spell 技能

> 所有的函数在使用时都会以`ni.spell.`作为前缀。

---

## available

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

## cast

参数:

- **spell** `id|string` _技能id 或 名称_
- **target** `token|guid`

Returns: `void`

施放指定的法术。如果提供了目标，将对该目标施放，否则将对自己施放。

```lua
ni.spell.cast("Shadow Bolt", "target")
```

## delaycast

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

## bestaoeloc

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

这个函数将在符合要求的最佳位置施放指定的法术。每个参数是什么，见上文。

```lua
ni.spell.cast("Hurricane", 36, 4, 4); 
--在一个德鲁伊身上，这将在最佳位置施放飓风 
--在玩家的36码范围内，至少有4个小怪要被击中
```

## casthelpfulatbest

- **spell** `id|string`
- **distance** `number`
- **radius** `number`
- **minimumcount** `number` _optional_
- **inc** `number` _optional_
- **zindex_inc** `number` _optional_

Returns: `void`

这个函数将在符合要求的最佳位置施放指定的法术。每个参数是什么，见上文。

```lua
ni.spell.cast("Healing Rain", 36, 4, 5); 
--在萨满身上，这将在最佳位置施放治疗之雨
--在玩家的36码范围内，至少有5个友军被治疗击中。
```

## castat

参数:

- **spell** `id|string`
- **target** `token|guid|mouse`
- **offset** `number`

Returns: `void`

施放需要点击地面的指定法术（如死亡和腐烂，火雨，暴风雪）。

```lua
ni.spell.castat("Rain of Fire", "target")
```

## castatqueue

参数:

- **spell** `id|string`
- **target** `token|guid|mouse`

Returns: `void`

排队等待一个指定的法术，一旦它可用，就在地上施放。

```lua
ni.spell.castatqueue("Blizzard", "target")
```

## castqueue

参数:

- **spell** `id|string`
- **target** `token|guid`

Returns: `void`

排队等待一个指定的法术，一旦它可用，就会被施放。

```lua
ni.spell.castqueue("Fear", "target")
```

## castspells

参数:

- **spell** `id|string`
- **target** `token|guid`

Returns: `void`

施放由（`|`）分隔的指定法术。
如果提供了目标，它将对该目标施放、
否则，将对自己施放法术。
如果一个以上的法术触发了全局冷却时间，则不起作用。

```lua
ni.spell.castspells("Heroic Strike|Bloodthirst", "target")
```

## casttime

参数:

- **spell** `id|string`

Returns: `number`

计算指定法术的施法时间。

```lua
local casttime = ni.spell.casttime("Immolate") -- 1.25
```

## cd

参数:

- **spell** `id|string`

Returns: `number`

计算指定法术的冷却时间。如果该法术不在冷却时间内，则返回0。

```lua
if not ni.spell.cd(47891) then
  -- Shadow Ward 不在冷却时间内
end
```

## gcd

参数:

Returns: `boolean`

检查是否触发了全局冷却时间。

```lua
if not ni.spell.gcd() then
  -- 全局冷却时间没有激活，我们可以做一些事情
end
```

## id

参数:

- **spellname** `string`

Returns: `number|nil`

将法术的名称转换为法术的ID。如果法术不存在，则返回nil。

```lua
local spellid = ni.spell.id("Life Tap") -- 57946
```

## isinstant

参数:

- **spell** `string|id`

Returns: `boolean`

检查通过的法术是否为即时施法。

```lua
if ni.spell.isinstant(57946) then
  -- Life Tap 是即时法术，没有施法时间。
end
```

## stopcasting

参数:

Returns: `void`

停止施法。

```lua
ni.spell.stopcasting()
```

## stopchanneling

参数:

Returns: `void`

停止引导。

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

这个功能确保一个法术可以在特定的目标上施展。它包括检查，如：

!> [`ni.spell.valid`](api/spell.md#valid) is not the same as [`ni.spell.available`](api/spell.md#available).

- [`ni.unit.exists`](api/unit.md#exists)
- [`ni.player.los`](api/player.md)
- [`ni.player.isfacing`](api/player.md)

```lua
if ni.spell.valid("Fear", "target") then
  -- Fear 恐惧符合所有有效的标准
end
```
