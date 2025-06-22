# Random-Roblox-UI-test
Just a test




Booting the library:
loadstring(game:HttpGet("https://raw.githubusercontent.com/pope12042/Random-Roblox-UI-test/refs/heads/main/Boot.txt"))()


fixed link : https://raw.githubusercontent.com/pope12042/Random-Roblox-UI-test/refs/heads/main/Boot.txt


creating a window:
--!strict
--[[
    UI_Library_Example.lua

    UPDATED: Removed the TextBox demonstration, keeping only the
    basic "Developer Docs" style window frame.
    Place this script directly inside StarterPlayerScripts.
--]]

local Players = game:GetService("Players")
local PlayerGui = Players.LocalPlayer:WaitForChild("PlayerGui")
local TweenService = game:GetService("TweenService") -- Still useful if you add animations later

-- Require the core UI library module
-- If using the consolidated booter, this would be:
-- local UI = require(YOUR_UI_LIBRARY_ASSET_ID)
local UI = require(game.StarterPlayer.StarterPlayerScripts.UI.UI)


-- Create a ScreenGui for our UI elements if it doesn't exist.
local screenGui = PlayerGui:FindFirstChild("MyCustomUIScreen")
if not screenGui then
    screenGui = Instance.new("ScreenGui")
    screenGui.Name = "MyCustomUIScreen"
    screenGui.DisplayOrder = 10
    screenGui.ResetOnSpawn = false
    screenGui.Parent = PlayerGui
end

-- Initialize the UI library with the ScreenGui
UI.init(screenGui)

-- --- Create a Draggable Window (Developer Docs style) ---
local devDocsWindow = UI.new("Window", nil, {
    Title = "Developer Docs",
    Size = UDim2.new(0, 400, 0, 350),
    Position = UDim2.new(0.5, -200, 0.5, -175), -- Centered
    BackgroundColor3 = Color3.fromRGB(35, 35, 35),
    CornerRadius = UDim.new(0, 10),
    StrokeColor = Color3.fromRGB(20, 20, 20),
    StrokeThickness = 1,
    Padding = UDim.new(0, 0), -- No padding on main window
})

-- You can add other content here using UI.new(...)
-- For now, this window will be mostly empty except for its title bar.
local windowContentParent = devDocsWindow:GetInstance()

-- Example: Add a simple label to confirm the window is there
local infoLabel = UI.new("Label", windowContentParent, {
    Text = "This is a custom window!",
    Size = UDim2.new(1, -20, 0, 30),
    Position = UDim2.new(0, 10, 0, devDocsWindow.TitleBar.Size.Y.Offset + 15),
    TextXAlignment = Enum.TextXAlignment.Center,
    TextColor3 = Color3.fromRGB(255, 255, 255),
    Font = Enum.Font.SourceSansBold,
    TextSize = 20,
    BackgroundTransparency = 1,
})

print("UI Library Example Loaded with a simple window!")
