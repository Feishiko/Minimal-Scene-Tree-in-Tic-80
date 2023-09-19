# Minimal-Scene-Tree-in-Tic-80
A minimal scene tree

You can use it on your tic-80 or any other places you can use lua

```lua
-- title:   MinimalSceneTree
-- author:  Feishiko, feishiko@foxmail.com
-- desc:    A minimal scene tree
-- site:    feishiko.itch.io
-- license: MIT License
-- version: 0.1
-- script:  lua

CurrentScene = "Init"
FirstScene = "First" -- Change it to any scene you want
Scene = {}

function Scene:New(_scene)
	Scene[_scene] = {}
	self.__index = self
	return setmetatable(Scene[_scene], self)
end

function Scene:Change(...)
	local _scene, _keep = ...
		if _keep == nil then
    		Scene[_scene]:Init()
		end
	 CurrentScene = _scene
end

local initScene = Scene:New("Init")

function initScene:Loop()
	initScene:Change(FirstScene)
end

-- Scene First

-- Must contain an Init and a Loop Function

firstScene = Scene:New("First")

function firstScene:Init() -- Only emit once
    x = 1
end

function firstScene:Loop() -- Emit every frame
    x = x + 1
    cls(2)
    print(x)
    if btn(3) then
 	    firstScene:Change("Second") -- Change to any Scene you want
    end
end

-- Scene Second

secondScene = Scene:New("Second")

function secondScene:Init()

end

function secondScene:Loop()
	cls(3)
    print("Hello World!")
    if btn(2) then
        -- The sceond parameter controls 
        -- whether you want to keep the target 
        -- scene since the last changes.
        -- Default is not to keep
		secondScene:Change("First", true)
    end
end

function TIC()
	Scene[CurrentScene]:Loop()
end
```

Code without the example:

```lua
CurrentScene = "Init"
FirstScene = nil -- Change it to any scene you want
Scene = {}

function Scene:New(_scene)
	Scene[_scene] = {}
	self.__index = self
	return setmetatable(Scene[_scene], self)
end

function Scene:Change(...)
	local _scene, _keep = ...
		if _keep == nil then
    		Scene[_scene]:Init()
		end
	CurrentScene = _scene
end

local initScene = Scene:New("Init")

function initScene:Loop()
	initScene:Change(FirstScene)
end

function TIC()
	Scene[CurrentScene]:Loop()
end
```
