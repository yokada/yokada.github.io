---
layout: post
title: "lua: Writing a module contains classes"
tags: lua
---

I want to write 2 or more classes into a module by Lua.<br>
One class is private, another one is public, but I don't know how to write that module properly.<br>
So, I have been assumed to make some codes to study that ways.

```lua
--
-- animals.lua
--

local m = {}

--
-- private class Animal
--
local Animal = function(_name)
   local self = setmetatable({
      name = _name
   }, {__index = {}})
   function self:get_name()
      return self.name
   end
   return self
end

--
-- public class Dog extends Animal
--
m.Dog = function(_name)
   local self = Animal(_name)
   function self:cry()
      return "wan-wan"
   end
   return self
end

return m
```

The resulte is here:

```lua
--
-- main.lua
--
local animals = require 'animals'

----[[
local dog1 = animals.Dog("pochi")
print(dog1:get_name()) --=> 'pochi'
print(dog1:cry())  --=> 'wan-wan'
--]]--
```

Unfortunately, I have no skills make sure this module is collect.<br>
These codes could cause some problems relevant to performance and inheritance classes so on.
