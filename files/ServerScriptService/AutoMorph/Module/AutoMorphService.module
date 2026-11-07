--[[--
	Created by ProbabilityGuy (Roblox) / uncleansable (Discord).
	I am the sole creator and holder of this script.
	All rights reserved. Do not redistribute, resell or claim as your own.
--]]--

local Players = game:GetService("Players")
local ServerStorage = game:GetService("ServerStorage")
local MorphFolder = ServerStorage:WaitForChild("Morphs")

local AutoMorphService = {}

local given = setmetatable({}, { __mode = "k" })

local function track(character: Model, item: Instance)
	local list = given[character]
	if not list then
		list = {}
		given[character] = list
	end
	table.insert(list, item)
end

local function clearGiven(character: Model)
	local list = given[character]
	if not list then return end

	for _, item in list do
		item:Destroy()
	end

	given[character] = nil
end

local function applyClothing(character: Model, className: string, property: string, value: string)
	local existing = character:FindFirstChildOfClass(className)
	if existing then
		existing[property] = value
		return
	end

	local item = Instance.new(className)
	item[property] = value
	item.Parent = character
end

function AutoMorphService:ApplyTeamClothes(character: Model, team: Team?)
	if not character then return end

	local player = Players:GetPlayerFromCharacter(character)
	local humanoid = character:FindFirstChildOfClass("Humanoid")
	local backpack = player and player:FindFirstChildOfClass("Backpack")

	clearGiven(character)

	if not team then return end

	local teamFolder = MorphFolder:FindFirstChild(team.Name)
	if not teamFolder then return end

	local clothesFolder = teamFolder:FindFirstChild("Clothes")
	if clothesFolder then
		local shirtObj = clothesFolder:FindFirstChildOfClass("Shirt")
		local pantsObj = clothesFolder:FindFirstChildOfClass("Pants")

		if shirtObj then
			applyClothing(character, "Shirt", "ShirtTemplate", shirtObj.ShirtTemplate)
		end

		if pantsObj then
			applyClothing(character, "Pants", "PantsTemplate", pantsObj.PantsTemplate)
		end
	end

	local accFolder = teamFolder:FindFirstChild("Accesories")
	if accFolder and humanoid then
		for _, item in accFolder:GetChildren() do
			if item:IsA("Accessory") then
				local clone = item:Clone()
				humanoid:AddAccessory(clone)
				track(character, clone)
			end
		end
	end

	local toolFolder = teamFolder:FindFirstChild("Tool")
	if toolFolder and backpack then
		for _, item in toolFolder:GetChildren() do
			if item:IsA("Tool") then
				local clone = item:Clone()
				clone.Parent = backpack
				track(character, clone)
			end
		end
	end
end

return AutoMorphService
