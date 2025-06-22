# Random-Roblox-UI-test
Just a test




Booting the library:
loadstring(game:HttpGet("https://raw.githubusercontent.com/pope12042/Random-Roblox-UI-test/refs/heads/main/Boot.txt"))()


fixed link : https://raw.githubusercontent.com/pope12042/Random-Roblox-UI-test/refs/heads/main/Boot.txt


creating a window:
--!strict
--[[
    UI_Library_Example.lua

    UPDATED: Now demonstrates creating a window similar to the "Developer Docs"
    screenshot, alongside the existing TabController example.
    Place this script directly inside StarterPlayerScripts.
--]]

local Players = game:GetService("Players")
local PlayerGui = Players.LocalPlayer:WaitForChild("PlayerGui")
local TweenService = game:GetService("TweenService")

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

-- --- Create a Draggable Window similar to the "Developer Docs" screenshot ---
local devDocsWindow = UI.new("Window", nil, {
    Title = "Developer Docs", -- Title from screenshot
    Size = UDim2.new(0, 450, 0, 450), -- Larger size to accommodate content
    Position = UDim2.new(0.5, -460, 0.5, -225), -- Positioned to the left of center
    BackgroundColor3 = Color3.fromRGB(35, 35, 35), -- Dark background matching screenshot
    CornerRadius = UDim.new(0, 10), -- Rounded corners
    StrokeColor = Color3.fromRGB(20, 20, 20), -- Subtle border for definition
    StrokeThickness = 1,
    Padding = UDim.new(0, 0), -- No padding on main window, content frames will handle it
})

local devDocsContentParent = devDocsWindow:GetInstance() -- Get the main frame for content

-- Sub-heading: "Creating a window"
local subHeadingLabel = UI.new("Label", devDocsContentParent, {
    Text = "Creating a window",
    Size = UDim2.new(1, -20, 0, 30),
    Position = UDim2.new(0, 10, 0, devDocsWindow.TitleBar.Size.Y.Offset + 15), -- Below title bar + padding
    TextXAlignment = Enum.TextXAlignment.Left,
    TextYAlignment = Enum.TextYAlignment.Center,
    TextColor3 = Color3.fromRGB(255, 255, 255),
    Font = Enum.Font.SourceSansBold, -- Use SourceSansBold for headings
    TextSize = 22,
    BackgroundTransparency = 1,
})

-- Code Block Simulation (using a TextBox for editable code-like text)
local codeTextBox = UI.new("TextBox", devDocsContentParent, {
    Text = [[
local Window = Random:CreateWindow({
    Name = "Random Example Window",
    Icon = 0, -- Icon in Topbar. Can also be a URL.
    LoadingTitle = "Random UI",
    LoadingSubtitle = "by Pope12042",
    ShowText = "Test",
    Theme = "Default",
    ToggleUIKeybind = "K",
    DisableRayfieldPrompts = false,
    DisableBuildWarnings = false,
    ConfigurationSaving = {
        Enabled = true,
        FolderName = nil,
        FileName = "Big Hub"
    },
    Discord = {
        Enabled = false,
        Invite = "noinvitelink",
        RememberJoins = true
    },
    KeySystem = false,
})
    ]], -- Example code
    MultiLine = true, -- Allow multiple lines of text
    TextXAlignment = Enum.TextXAlignment.Left,
    TextYAlignment = Enum.TextYAlignment.Top, -- Align text to top
    Size = UDim2.new(1, -20, 1, -(devDocsWindow.TitleBar.Size.Y.Offset + 15 + subHeadingLabel.Size.Y.Offset + 20)), -- Fill remaining space
    Position = UDim2.new(0, 10, 0, subHeadingLabel:GetInstance().Position.Y.Offset + subHeadingLabel:GetInstance().Size.Y.Offset + 10),
    BackgroundColor3 = Color3.fromRGB(25, 25, 25), -- Darker background for code area
    TextColor3 = Color3.fromRGB(200, 200, 200), -- Light grey text for code
    Font = Enum.Font.Inconsolata, -- Monospace font for code (if available, fallback to SourceSansMono)
    TextSize = 14,
    ClearTextOnFocus = false, -- Don't clear on focus for a code viewer
    CornerRadius = UDim.new(0, 5),
    StrokeColor = Color3.fromRGB(15, 15, 15),
    StrokeThickness = 1,
    Padding = UDim.new(0, 8), -- Padding inside the code box
    TextWrapped = true, -- Allow text wrapping if it goes beyond bounds
})
-- Fallback for font if Inconsolata isn't strictly available (Roblox sometimes uses nearest match)
codeTextBox:SetFont(Enum.Font.Inconsolata or Enum.Font.SourceSansMono)


-- --- Existing Tabbed Window (from previous response) ---
local myWindow = UI.new("Window", nil, {
    Title = "Tabbed Interface Demo",
    Size = UDim2.new(0, 400, 0, 350),
    Position = UDim2.new(0.5, 60, 0.5, -175), -- Positioned to the right of the dev docs window
    BackgroundColor3 = Color3.fromRGB(30, 30, 30),
    CornerRadius = UDim.new(0, 10),
    StrokeColor = Color3.fromRGB(15, 15, 15),
    StrokeThickness = 2,
    Padding = UDim.new(0, 0),
})

local windowContentParent = myWindow:GetInstance()

local tabController = UI.new("TabController", windowContentParent, {
    Size = UDim2.new(1, 0, 1, -myWindow.TitleBar.Size.Y.Offset),
    Position = UDim2.new(0, 0, 0, myWindow.TitleBar.Size.Y.Offset),
    BackgroundColor3 = Color3.fromRGB(40, 40, 40),
    CornerRadius = UDim.new(0, 8),
    StrokeColor = Color3.fromRGB(20, 20, 20),
    StrokeThickness = 1,
    Padding = UDim.new(0, 10),
    TabButtonHeight = 35,
    TabButtonSpacing = 5,
    TabButtonBackgroundColor3 = Color3.fromRGB(50, 50, 50),
    TabButtonActiveColor3 = Color3.fromRGB(75, 150, 255),
})

tabController:AddTab("General", function(parentFrame)
    local tab1Content = UI.new("Frame", parentFrame, {
        BackgroundColor3 = Color3.fromRGB(40, 40, 40),
        BackgroundTransparency = 0,
        CornerRadius = UDim.new(0, 5),
        Padding = UDim.new(0, 10),
        ClipsDescendants = true,
    })

    local tab1Parent = tab1Content:GetInstance()

    local settingLabel = UI.new("Label", tab1Parent, {
        Text = "Toggle a Feature:",
        Size = UDim2.new(1, 0, 0, 30),
        Position = UDim2.new(0, 0, 0, 0),
        TextXAlignment = Enum.TextXAlignment.Left,
        TextColor3 = Color3.fromRGB(220, 220, 220),
        BackgroundTransparency = 1,
    })

    local myToggle = UI.new("Toggle", tab1Parent, {
        Text = "Enable Feature X",
        InitialState = false,
        Size = UDim2.new(0.9, 0, 0, 25),
        Position = UDim2.new(0, 0, 0, 35),
        OnColor = Color3.fromRGB(0, 200, 100),
    })

    myToggle:OnChanged(function(newState)
        print("Feature X state:", newState)
        settingLabel:SetText("Feature X: " .. (newState and "Enabled" or "Disabled"))
    end)

    local inputLabel = UI.new("Label", tab1Parent, {
        Text = "Your Name:",
        Size = UDim2.new(1, 0, 0, 75),
        Position = UDim2.new(0, 0, 0, 75),
        TextXAlignment = Enum.TextXAlignment.Left,
        TextColor3 = Color3.fromRGB(220, 220, 220),
        BackgroundTransparency = 1,
    })

    local nameTextBox = UI.new("TextBox", tab1Parent, {
        Text = "Type your name...",
        Size = UDim2.new(0.9, 0, 0, 30),
        Position = UDim2.new(0, 0, 0, 105),
        BackgroundColor3 = Color3.fromRGB(50, 50, 50),
        TextColor3 = Color3.fromRGB(255, 255, 255),
        ClearTextOnFocus = true,
    })

    nameTextBox:OnEnterPressed(function(text)
        print("Name entered:", text)
        inputLabel:SetText("Hello, " .. text .. "!")
        task.wait(1)
        inputLabel:SetText("Your Name:")
    end)

    return tab1Content
end)

tabController:AddTab("Statistics", function(parentFrame)
    local tab2Content = UI.new("Frame", parentFrame, {
        BackgroundColor3 = Color3.fromRGB(40, 40, 40),
        BackgroundTransparency = 0,
        CornerRadius = UDim.new(0, 5),
        Padding = UDim.new(0, 10),
        ClipsDescendants = true,
    })

    local tab2Parent = tab2Content:GetInstance()

    local statLabel = UI.new("Label", tab2Parent, {
        Text = "Player Stats:",
        Size = UDim2.new(1, 0, 0, 30),
        Position = UDim2.new(0, 0, 0, 0),
        TextXAlignment = Enum.TextXAlignment.Left,
        TextColor3 = Color3.fromRGB(220, 220, 220),
        BackgroundTransparency = 1,
    })

    local xpLabel = UI.new("Label", tab2Parent, {
        Text = "XP: 1234",
        Size = UDim2.new(1, 0, 0, 25),
        Position = UDim2.new(0, 0, 0, 35),
        TextXAlignment = Enum.TextXAlignment.Left,
        TextColor3 = Color3.fromRGB(150, 255, 150),
        BackgroundTransparency = 1,
    })

    local levelLabel = UI.new("Label", tab2Parent, {
        Text = "Level: 42",
        Size = UDim2.new(1, 0, 0, 25),
        Position = UDim2.new(0, 0, 0, 60),
        TextXAlignment = Enum.TextXAlignment.Left,
        TextColor3 = Color3.fromRGB(150, 150, 255),
        BackgroundTransparency = 1,
    })

    local refreshButton = UI.new("Button", tab2Parent, {
        Text = "Refresh Stats",
        Size = UDim2.new(0.6, 0, 0, 40),
        Position = UDim2.new(0.5, -((tab2Parent.AbsoluteSize.X * 0.6) / 2), 0, 100),
        BackgroundColor3 = Color3.fromRGB(255, 150, 75),
        CornerRadius = UDim.new(0, 8),
    })

    refreshButton:OnClick(function()
        local randomXP = math.random(1000, 9999)
        local randomLevel = math.random(10, 100)
        xpLabel:SetText("XP: " .. randomXP)
        levelLabel:SetText("Level: " .. randomLevel)
        print("Stats refreshed!")
    end)

    return tab2Content
end)

print("UI Library Example Loaded with Developer Docs style window and Tabs!")
