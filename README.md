-- // The Streets Anti-Exploit bypass (kind of) rewritten by John.
-- // Synapse only.
local metatable = getrawmetatable(game)
setreadonly(metatable, false)
local index, newindex, namecall = metatable.__index, metatable.__newindex, metatable.__namecall
local walking_speed = 16
local running_speed = 33
local crouching_speed = 8 -- you can change these three to your liking
local default_index_values = { -- what the script gives the anticheat when the anticheat asks for it (recommend you do not change)
	["WalkSpeed"] = 16,
	["JumpPower"] = 38,
	["HipHeight"] = 0,
	["Gravity"] = 196.1
}
metatable.__index = function(t,k)
	if k == "IlIl" then
		return
	end
	if default_index_values[k] ~= nil then
		if not checkcaller() then
			return default_index_values[k]
		end
	end
	return index(t,k)
end
metatable.__newindex = function(t,k,v)
	if k == "WalkSpeed" then
		if not checkcaller() then
			if game:service("UserInputService"):IsKeyDown(Enum.KeyCode.LeftShift) and not game:service("UserInputService"):IsKeyDown(Enum.KeyCode.S) then
				return newindex(t,k,running_speed)
			end
			if game:service("UserInputService"):IsKeyDown(Enum.KeyCode.LeftControl) then
				return newindex(t,k,crouching_speed)
			end
			return newindex(t,k,walking_speed)
		end
	end
	if k == "CFrame" then
		if t == game.Players.LocalPlayer.Character.HumanoidRootPart then
			if not checkcaller() then
				return	
			end
		end
	end
	if k == "Gravity" or k == "Health" then
		if not checkcaller() then
			return	
		end
	end
	return newindex(t,k,v)
end
metatable.__namecall = function(t, ...)
	local oof = {...}
	if oof[#oof] == "SetStateEnabled" or oof[#oof] == "BreakJoints" or oof[#oof] == "Kick" then
		if not checkcaller() then
			return
		end
	end
	if oof[#oof] == "Destroy" then
		if t:IsA("HopperBin") then
			if not checkcaller() then
				return
			end
		end
	end
	return namecall(t, ...)
end
