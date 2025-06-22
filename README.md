# Random-Roblox-UI-test
Just a test




Booting the library:
local Random = loadstring(game:HttpGet("https://raw.githubusercontent.com/pope12042/Random-Roblox-UI-test/refs/heads/main/Boot.txt"))() 


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



creating a tab:
--!strict
--[[
    Library_Booter.lua

    This LocalScript is the "library booter."
    It loads the entire UI library from a single Roblox Asset ID.

    How to use:
    1. Copy the content from "UI_Library_Consolidated.lua" (above) into a new ModuleScript in Roblox Studio.
    2. Right-click that ModuleScript in Explorer and select "Save to Roblox...".
    3. Note down the Asset ID (e.g., 1234567890) that Roblox provides.
    4. Replace the 'YOUR_UI_LIBRARY_ASSET_ID' placeholder below with your actual Asset ID.
    5. Place this 'Library_Booter.lua' LocalScript into 'StarterPlayerScripts'.
    6. Run your game!
--]]

local Players = game:GetService("Players")
local PlayerGui = Players.LocalPlayer:WaitForChild("PlayerGui")

-- === CONFIGURATION ===
local UI_LIBRARY_ASSET_ID = 0 -- <<< IMPORTANT: REPLACE WITH YOUR UI LIBRARY ASSET ID!
-------------------------

if UI_LIBRARY_ASSET_ID == 0 then
    error("UI_LIBRARY_ASSET_ID is not set! Please replace '0' with your uploaded ModuleScript's Asset ID.")
end

-- Load the entire UI library from the Asset ID
local UI_Library: { [string]: any } = nil
pcall(function()
    UI_Library = require(UI_LIBRARY_ASSET_ID)
end)

if not UI_Library then
    error("Failed to load UI Library from Asset ID: " .. UI_LIBRARY_ASSET_ID .. ". Make sure the Asset ID is correct and it's a valid ModuleScript.")
end

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
UI_Library.init(screenGui)

-- --- Now you can use the UI library! ---
-- Example: Create a Draggable Window
local myWindow = UI_Library.new("Window", nil, {
    Title = "Loaded UI Library",
    Size = UDim2.new(0, 350, 0, 300),
    Position = UDim2.new(0.5, -175, 0.5, -150),
    BackgroundColor3 = Color3.fromRGB(30, 30, 30),
    CornerRadius = UDim.new(0, 10),
    StrokeColor = Color3.fromRGB(15, 15, 15),
    StrokeThickness = 2,
    Padding = UDim.new(0, 15),
})

local windowContentParent = myWindow:GetInstance()

local mainLabel = UI_Library.new("Label", windowContentParent, {
    Text = "Library loaded via Asset ID!",
    Size = UDim2.new(1, 0, 0, 30),
    Position = UDim2.new(0, 0, 0, myWindow.TitleBar.Size.Y.Offset + 10),
    TextColor3 = Color3.fromRGB(200, 200, 200),
    TextSize = 20,
    Font = Enum.Font.FredokaOne,
    BackgroundTransparency = 1,
})

local myButton = UI_Library.new("Button", windowContentParent, {
    Text = "Test Button",
    Size = UDim2.new(0.8, 0, 0, 40),
    Position = UDim2.new(0.5, -((350 * 0.8) / 2), 0, myWindow.TitleBar.Size.Y.Offset + 60),
    BackgroundColor3 = Color3.fromRGB(75, 150, 255),
    CornerRadius = UDim.new(0, 8),
    StrokeColor = Color3.fromRGB(40, 40, 40),
    StrokeThickness = 1,
})

myButton:OnClick(function()
    warn("Button clicked from loaded library!")
    myButton:SetText("Clicked!")
    task.wait(0.5)
    myButton:SetText("Test Button")
end)

local myToggle = UI_Library.new("Toggle", windowContentParent, {
    Text = "Dynamic Feature",
    InitialState = false,
    Size = UDim2.new(0.8, 0, 0, 25),
    Position = UDim2.new(0.5, -((350 * 0.8) / 2), 0, myWindow.TitleBar.Size.Y.Offset + 120),
    OnColor = Color3.fromRGB(0, 200, 100),
})

myToggle:OnChanged(function(newState)
    print("Toggle for dynamic feature state:", newState)
end)

local myTextBox = UI_Library.new("TextBox", windowContentParent, {
    Text = "Enter command...",
    Size = UDim2.new(0.8, 0, 0, 30),
    Position = UDim2.new(0.5, -((350 * 0.8) / 2), 0, myWindow.TitleBar.Size.Y.Offset + 160),
    BackgroundColor3 = Color3.fromRGB(50, 50, 50),
    TextColor3 = Color3.fromRGB(255, 255, 255),
    ClearTextOnFocus = true,
    Font = Enum.Font.SourceSans,
    TextSize = 15,
    CornerRadius = UDim.new(0, 6),
    StrokeColor = Color3.fromRGB(40, 40, 40),
    StrokeThickness = 1,
    Padding = UDim.new(0, 5),
})

myTextBox:OnEnterPressed(function(text)
    print("Command entered:", text)
    myTextBox:SetText("Executing: " .. text)
    task.wait(1)
    myTextBox:SetText("Enter command...")
end)

print("Library Booter loaded UI from Asset ID and created example UI!")                              




creating a button:
local Plr, UI = game:GetService("Players"), require(game.StarterPlayer.StarterPlayerScripts.UI.UI)
local SG = Plr.LocalPlayer:WaitForChild("PlayerGui"):FindFirstChild("MyCustomUIScreen") or Instance.new("ScreenGui")
if not SG.Parent then SG.Name="MyCustomUIScreen"; SG.DisplayOrder=10; SG.ResetOnSpawn=false; SG.Parent=Plr.LocalPlayer.PlayerGui end UI.init(SG)
local myWindow = UI.new("Window", nil, {Title="Button Tab Demo", Size=UDim2.new(0,250,0,150), Position=UDim2.new(0.5,-125,0.5,-75)})
local btnTabC = myWindow:CreateTab("Button", 3899201970); local btn = UI.new("Button", btnTabC, {Text="Click Me!", Size=UDim2.new(0.6,0,0,40), Position=UDim2.new(0.5,-75,0.5,-20)}); btn:OnClick(function()
--the function when you press the button.
end)
