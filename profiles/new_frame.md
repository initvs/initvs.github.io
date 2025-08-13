

## 并列排序的图标并支持勾选框的界面配置方案

<img src="https://raw.githubusercontent.com/initvs/initvs.github.io/refs/heads/main/_media/NEWFRAMEXAMPLE.png" alt="NEW FRAME">

## 调用说明

参数:

- **type** `固定类型|string` `iconradio`
- **key** `键值|string` 
- **single** `布尔值|true|false`
- **selected** `布尔值`

返回: `1|0`, `1则为true, 0则为false`

说明：界面交互，生成菜单界面 如·上图·所示.


## 示例：

```lua

--#############菜单中的items部分#############--

{   type = "iconradio",
    key = "kmp",
    single = true,-- 单选模式：只能选一项 false 多选模式：可以勾选多个图标 如 判断技能是否启用。
    options = {
    { id = 1, text = skills.Obliterate.iconname, tooltip = "KM 消耗优先：湮灭 > 冰霜打击", selected = true },
    { id = 2, text = skills.HowlingBlast.iconname, tooltip = "KM 消耗优先：凛风冲击(无消耗) > 湮灭 > 冰霜打击", selected = false  },
    { id = 3, text = skills.FrostStrike.iconname, tooltip = "KM 消耗优先：冰霜打击", selected = false  },
},

--############# GetSetting部分 #############--
--///////////////////////////
local function GetSetting(name)
    for k, v in ipairs(items) do
      if v.type == "entry" and v.key ~= nil and v.key == name then
        return v.value, v.enabled
      end
      if v.type == "dropdown" and v.key ~= nil and v.key == name then
        for k2, v2 in pairs(v.menu) do
          if v2.selected then
            return v2.value
          end
        end
      end
      if v.type == "input" and v.key ~= nil and v.key == name then
        return v.value
      end
-------#############添加该类型的界面交互：#############-------------------------
      if v.type == "iconradio" and v.key ~= nil and v.key == name then
        if v.single then
          for _, opt in ipairs(v.options) do
              if opt.selected then
                  return opt.id
              end
          end
        else
          local result = {}
          for i, opt in ipairs(v.options) do
              result[i] = opt.selected
          end
          return result
        end
      end
-----------------------------------------------------------------------------



--------############# callback回调函数部分 #############

local function GUICallback(key, item_type, value)
    if item_type == "enabled" then
      enables[key] = value
    elseif item_type == "value" then
      values[key] = value
    elseif item_type == "input" then
      inputs[key] = value
    elseif item_type == "menu" then
      menus[key] = value
-------#############添加该类型的界面交互：#############==-----------------------
    elseif item_type == "iconradio" then
      options[key] = value
    end
------------------------------------------------------------------------------
end

}


```
