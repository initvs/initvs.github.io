# Quick start (快速开始)

Assuming you already downloaded ni from [Discord Channel] this introductory guide will help you get up to speed.\n
假设你已经从[Discord Channel]下载了ni，这个指南将帮助你尽快上手。

Lets get started with the location of profiles.

让我们从配置文件的位置开始。
```
addon
└───Core
└───Settings
└───Rotations
│	└───Data
│	└───Warlock
```

我们感兴趣的两个文件夹是 "数据"和以我们角色的职业命名的文件夹。在本指南中，我们将为 "Warlock（术士）" 创建一个简单的配置文件。这意味着我们将专注于这两个文件夹。
Two folders of our interest are `Data` and folder named by the class of our character. In this guide we will create a simple profile for `Warlock`. That means we are going to focus on these two folders.

我们的配置文件需要有一个名字--在本指南中，我们将称之为 "Warlock_Example"。
Our profile needs to have a name - and for this guide we will call it `Warlock_Example`.

请按以下步骤进行：
Please do the following steps:

#### 1. 在addon/Rotations/Warlock文件夹下创建一个名为`Warlock_Example.lua`的文件。你的文件夹应该看起来像这样：
#### 1. Create a file called `Warlock_Example.lua` inside the folder addon/Rotations/Warlock. Your folders should look like this:

```
addon
└───Core
└───Settings
└───Rotations
│	└───Data
│	└───Warlock
│	│	└───Warlock_Example.lua
```
2. 一旦文件创建完毕，用你最喜欢的文本编辑器打开它。[我们的建议](getting-started/faq.md#which-text-editor to use)
2. Once the file is created, open it using your favourite text editor. [Our recommendation](getting-started/faq.md#which-text-editor-to-use)

#### 3. 复制并粘贴以下模板代码：
#### 3. Copy and paste the following boilerplate code:

```lua
local queue = {
	"Print Hello"
}

local abilities = {
	["Print Hello"] = function()
		print("Hello")
	end
}

ni.bootstrap.profile("Warlock_Example", queue, abilities)
```

!> 确保文件名与传递给`ni.bootstrap.profile`的名称一致。
!> Make sure that the name of file matches the name passed to `ni.bootstrap.profile`

这就是**ni**运行一个配置文件所需要的全部基本内容。按下`F12`，看看是否有`Hello`字样被打印到游戏聊天窗口。
This is all that **ni** needs to run a profile. Presss `F12` and see if the word `Hello` is being printed to the console.

#### 4. 如果我们想有一个动态队列，多个队列可以实时变化，我们也可以传递一个`function'。
#### 4. In case we would like to have a dynamic queue, multiple queues which can change in real time, we can also pass a `function`.

```lua
local ishelloprinted = false

local queue = {
	"Print Hello"
}

local queue2 = {
	"Print Hello World"
}

local abilities = {
	["Print Hello"] = function()
		ishelloprinted = true
		ni.debug.log("Hello")
	end,
	["Print Hello World"] = function()
		ishelloprinted = false
		ni.debug.log("Hello World")
	end
}

local dynamicqueue = function()
	if ishelloprinted then
		return queue
	end

	return queue2
end

ni.bootstrap.profile("Warlock_Example", dynamicqueue, abilities)
```

!> 有可能只使用一种方式来设置优先级队列（静态或动态）。
!> It's possible to only use one way to set the priority queue (either static or dynamic).

!> 以下加载数据文件的方法或已被废弃，应避免使用。请看较新的方法[这里](https://github.com/initvs/ni-profiles/blob/main/Generic/GUIExample.lua#L1)
!> The following method for loading data files is deprecated and should be avoided. Please look at the newer method [here](https://github.com/initvs/ni-profiles/blob/main/Generic/GUIExample.lua#L1)

#### 5. 如果我们有一些共同的函数或变量，我们想在多个配置文件中共享 - 我们可以通过在`Data`文件夹中创建Lua文件来实现。让我们创建`Data_Example.lua`。
#### 5. In case we have some common functions or variables that we would like to share among multiple profiles - we can do it by creating Lua files in `Data` folder. Lets create `Data_Example.lua`.

```
addon
└───Core
└───Settings
└───Rotations
│	└───Data
│	│	└───Data_Example.lua
│	└───Warlock
│	│	└───Warlock_Example.lua
```

> 你可以在**Warlock**和**Data**中随意命名这两个文件，它们不需要遵循任何惯例。
> You can name both files in **Warlock** and **Data** however you like, they don't need to follow any convention.

#### 6. 在数据文件和配置文件之间传递变量和函数可以通过两种方式进行：
#### 6. Passing variables and functions between Data and Profile files can be done in two ways:

- by declaring and assigning globals (not recommended) - 通过声明和分配globals（不推荐）。
- by using `ni.data` and having a new unique namespace (`table`) - 通过使用`ni.data`和拥有一个新的唯一命名空间（`table`）。

让我们使用第二种方法，在`Rotations/Data/Data_Example.lua`中添加以下内容。
Lets use the second way and add the following to `Rotations/Data/Data_Example.lua`.

```lua
ni.data.example = {
 ishelloprinted = false
}
```

!> 以下加载数据文件或GUI的方法已被废弃，应避免使用。请看例子[这里](https://github.com/initvs/ni-profiles/blob/main/Generic/GUIExample.lua)
!> The following method for loading the data file or GUI is deprecated and should be avoided. Please look at the examples [here](https://github.com/initvs/ni-profiles/blob/main/Generic/GUIExample.lua)


#### 7. 加载数据文件可以通过创建一个包含文件名字符串的表格，并将该表格作为第四个参数传递给`ni.bootstrap.profile`来完成。
#### 7. Loading Data files can be done by creating a table which contains strings of file names and passing that table as 4th argument to `ni.bootstrap.profile`.

```lua
local data = {
	"Data_Example.lua"
}

local queue = {
	"Print Hello"
}

local queue2 = {
	"Print Hello World"
}

local abilities = {
	["Print Hello"] = function()
		ni.data.example.ishelloprinted = true
		ni.debug.log("Hello")
	end,
	["Print Hello World"] = function()
		ni.data.example.ishelloprinted = false
		ni.debug.log("Hello World")
	end
}

local dynamicqueue = function()
	if ni.data.example.ishelloprinted then
		return queue
	end

	return queue2
end

ni.bootstrap.profile("Warlock_Example", dynamicqueue, abilities, data)
```
以上是**ni**运行一个配置文件基本指南，希望你有所收获。
