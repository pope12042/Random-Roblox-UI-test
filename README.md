# Random-Roblox-UI-test
Just a test




Booting the library:
local Random = loadstring(game:HttpGet("https://raw.githubusercontent.com/pope12042/Random-Roblox-UI-test/refs/heads/main/Boot.txt"))() 


fixed link : https://raw.githubusercontent.com/pope12042/Random-Roblox-UI-test/refs/heads/main/Boot.txt      


creating a window:
    --!strict
    --[[
        UI_Library_Consolidated.lua

        FINAL VERSION: This is the complete, consolidated UI library.
        Copy this ENTIRE code block into a NEW ModuleScript in Roblox Studio.
        Then, right-click that ModuleScript and "Save to Roblox..." to get its Asset ID.
    --]]

    -- Services (defined once for the entire consolidated library)
    local TweenService = game:GetService("TweenService")
    local Players = game:GetService("Players")
    local RunService = game:GetService("RunService")

    -- === BaseComponent Module ===
    local BaseComponent = {}
    BaseComponent.__index = BaseComponent

    local DEFAULT_BASE_PROPERTIES = {
        Position = UDim2.new(0, 0, 0, 0),
        Size = UDim2.new(0, 100, 0, 50),
        BackgroundColor3 = Color3.fromRGB(60, 60, 60),
        BackgroundTransparency = 0,
        BorderSizePixel = 0,
        Visible = true,
        ZIndex = 1,
        AnchorPoint = Vector2.new(0, 0),
        ClipsDescendants = true,

        Text = "",
        TextColor3 = Color3.fromRGB(255, 255, 255),
        Font = Enum.Font.SourceSansBold,
        TextSize = 16,
        TextScaled = false,
        TextXAlignment = Enum.TextXAlignment.Center,
        TextYAlignment = Enum.TextYAlignment.Center,
        TextWrapped = false,
        TextTransparency = 0,

        CornerRadius = UDim.new(0, 0),
        Padding = nil,

        StrokeColor = nil,
        StrokeTransparency = 0,
        StrokeThickness = 1,
        StrokeLineJoinMode = Enum.LineJoinMode.Miter,

        GradientColor = nil,
        GradientRotation = 0,
        GradientOffset = Vector2.new(0, 0),

        Draggable = false,
    }

    function BaseComponent.new(instanceType: Enum.StudioPluginSetting, parent: GuiObject, props: { [string]: any }): (any)
        local self = setmetatable({}, BaseComponent)
        local instance = Instance.new(instanceType.Name)
        instance.Parent = parent
        self._instance = instance

        local finalProps = {}
        for propName, defaultValue in pairs(DEFAULT_BASE_PROPERTIES) do
            finalProps[propName] = defaultValue
        end
        for propName, value in pairs(props) do
            finalProps[propName] = value
        end

        for propName, value in pairs(finalProps) do
            if propName == "Padding" and value ~= nil then
                local padding = Instance.new("UIPadding")
                if typeof(value) == "table" then
                    padding.PaddingLeft = value.Left or UDim.new(0,0)
                    padding.PaddingRight = value.Right or UDim.new(0,0)
                    padding.PaddingTop = value.Top or UDim.new(0,0)
                    padding.PaddingBottom = value.Bottom or UDim.new(0,0)
                else
                    padding.PaddingLeft = value
                    padding.PaddingRight = value
                    padding.PaddingTop = value
                    padding.PaddingBottom = value
                end
                padding.Parent = instance
            elseif propName == "CornerRadius" and (value.Scale ~= 0 or value.Offset ~= 0) then
                local uiCorner = Instance.new("UICorner")
                uiCorner.CornerRadius = value
                uiCorner.Parent = instance
            elseif propName == "StrokeColor" and value ~= nil then
                local uiStroke = Instance.new("UIStroke")
                uiStroke.Color = value
                uiStroke.Transparency = finalProps.StrokeTransparency
                uiStroke.Thickness = finalProps.StrokeThickness
                uiStroke.LineJoinMode = finalProps.StrokeLineJoinMode
                uiStroke.Parent = instance
            elseif propName == "GradientColor" and value ~= nil then
                local uiGradient = Instance.new("UIGradient")
                uiGradient.Color = value
                uiGradient.Rotation = finalProps.GradientRotation
                uiGradient.Offset = finalProps.GradientOffset
                uiGradient.Parent = instance
            elseif instance[propName] ~= nil then
                if propName == "Font" and typeof(value) == "EnumItem" and value.ClassName == "EnumItem" then
                    instance.Font = value
                else
                    instance[propName] = value
                end
            end
        end

        self._draggable = finalProps.Draggable
        return self
    end

    function BaseComponent:GetInstance(): GuiObject
        return self._instance
    end

    function BaseComponent:SetProperty(propName: string, value: any)
        if self._instance[propName] ~= nil then
            self._instance[propName] = value
        else
            warn("SetProperty: Property '" .. propName .. "' does not exist on " .. self._instance.ClassName)
        end
    end

    function BaseComponent:SetPosition(pos: UDim2) self._instance.Position = pos end
    function BaseComponent:SetSize(size: UDim2) self._instance.Size = size end
    function BaseComponent:SetBackgroundColor(color: Color3) self._instance.BackgroundColor3 = color end
    function BaseComponent:SetBackgroundTransparency(transparency: number) self._instance.BackgroundTransparency = transparency end
    function BaseComponent:SetText(text: string)
        if self._instance:IsA("TextLabel") or self._instance:IsA("TextButton") or self._instance:IsA("TextBox") then
            self._instance.Text = text
        else warn("SetText called on a non-text component:", self._instance.ClassName) end
    end
    function BaseComponent:SetTextColor(color: Color3)
        if self._instance:IsA("TextLabel") or self._instance:IsA("TextButton") or self._instance:IsA("TextBox") then
            self._instance.TextColor3 = color
        else warn("SetTextColor called on a non-text component:", self._instance.ClassName) end
    end
    function BaseComponent:SetFont(font: Enum.Font)
        if self._instance:IsA("TextLabel") or self._instance:IsA("TextButton") or self._instance:IsA("TextBox") then
            self._instance.Font = font
        else warn("SetFont called on a non-text component:", self._instance.ClassName) end
    end
    function BaseComponent:SetTextSize(size: number)
        if self._instance:IsA("TextLabel") or self._instance:IsA("TextButton") or self._instance:IsA("TextBox") then
            self._instance.TextSize = size
        else warn("SetTextSize called on a non-text component:", self._instance.ClassName) end
    end
    function BaseComponent:SetVisible(visible: boolean) self._instance.Visible = visible end

    function BaseComponent:Destroy()
        self._instance:Destroy()
        for i, v in pairs(self) do self[i] = nil end
        setmetatable(self, nil)
    end
    -- === End BaseComponent Module ===


    -- === Button Module ===
    local Button = {}
    Button.__index = Button
    setmetatable(Button, BaseComponent)

    local DEFAULT_BUTTON_PROPERTIES = {
        Size = UDim2.new(0, 120, 0, 40),
        BackgroundColor3 = Color3.fromRGB(75, 150, 255),
        TextColor3 = Color3.fromRGB(255, 255, 255),
        Text = "Button",
        CornerRadius = UDim.new(0, 6),
        AutoButtonColor = false,
        StrokeColor = Color3.fromRGB(40, 40, 40),
        StrokeThickness = 1,
    }

    function Button.new(parent: GuiObject, props: { [string]: any }): (any)
        local mergedProps = {}
        for k, v in pairs(DEFAULT_BUTTON_PROPERTIES) do mergedProps[k] = v end
        for k, v in pairs(props) do mergedProps[k] = v end

        local self = BaseComponent.new(Enum.Class.TextButton, parent, mergedProps)
        setmetatable(self, Button)

        self._instance.MouseButton1Click:Connect(function()
            if self.onClick then
                self.onClick()
            end
        end)

        return self
    end

    function Button:OnClick(handler: () -> ())
        self.onClick = handler
    end
    -- === End Button Module ===


    -- === Label Module ===
    local Label = {}
    Label.__index = Label
    setmetatable(Label, BaseComponent)

    local DEFAULT_LABEL_PROPERTIES = {
        Size = UDim2.new(0, 200, 0, 30),
        BackgroundColor3 = Color3.fromRGB(255, 255, 255),
        BackgroundTransparency = 1,
        TextColor3 = Color3.fromRGB(255, 255, 255),
        Text = "New Label",
        TextXAlignment = Enum.TextXAlignment.Center,
        TextYAlignment = Enum.TextYAlignment.Center,
        TextSize = 16,
        Font = Enum.Font.SourceSansBold,
    }

    function Label.new(parent: GuiObject, props: { [string]: any }): (any)
        local mergedProps = {}
        for k, v in pairs(DEFAULT_LABEL_PROPERTIES) do mergedProps[k] = v end
        for k, v in pairs(props) do mergedProps[k] = v end

        local self = BaseComponent.new(Enum.Class.TextLabel, parent, mergedProps)
        setmetatable(self, Label)

        return self
    end
    -- === End Label Module ===


    -- === Toggle Module ===
    local Toggle = {}
    Toggle.__index = Toggle
    setmetatable(Toggle, BaseComponent)

    local DEFAULT_TOGGLE_PROPERTIES = {
        Size = UDim2.new(0, 50, 0, 25),
        BackgroundColor3 = Color3.fromRGB(80, 80, 80),
        OnColor = Color3.fromRGB(75, 150, 255),
        InitialState = false,
        CornerRadius = UDim.new(0.5, 0),
        StrokeColor = Color3.fromRGB(40, 40, 40),
        StrokeThickness = 1,
        Text = "Toggle",
        TextColor3 = Color3.fromRGB(255, 255, 255),
        Font = Enum.Font.SourceSans,
        TextSize = 14,
        TextXAlignment = Enum.TextXAlignment.Left,
        TextYAlignment = Enum.TextYAlignment.Center,
        Padding = UDim.new(0, 5),
    }

    function Toggle.new(parent: GuiObject, props: { [string]: any }): (any)
        local mergedProps = {}
        for k, v in pairs(DEFAULT_TOGGLE_PROPERTIES) do mergedProps[k] = v end
        for k, v in pairs(props) do mergedProps[k] = v end

        local self = BaseComponent.new(Enum.Class.TextButton, parent, mergedProps)
        setmetatable(self, Toggle)

        local toggleButton = self._instance
        toggleButton.Text = ""
        toggleButton.AutoButtonColor = false

        local track = Instance.new("Frame")
        track.Name = "ToggleTrack"
        track.Size = UDim2.new(1, 0, 1, 0)
        track.Position = UDim2.new(0,0,0,0)
        track.BackgroundColor3 = mergedProps.BackgroundColor3
        track.BackgroundTransparency = 0
        track.Parent = toggleButton

        local trackCorner = Instance.new("UICorner")
        trackCorner.CornerRadius = mergedProps.CornerRadius
        trackCorner.Parent = track

        local handle = Instance.new("Frame")
        handle.Name = "ToggleHandle"
        handle.Size = UDim2.new(0.4, 0, 0.8, 0)
        handle.Position = UDim2.new(0, 0, 0.1, 0)
        handle.BackgroundColor3 = Color3.fromRGB(255, 255, 255)
        handle.Parent = track

        local handleCorner = Instance.new("UICorner")
        handleCorner.CornerRadius = UDim.new(0.5, 0)
        handleCorner.Parent = handle

        local label = Label.new(toggleButton, {
            Text = mergedProps.Text,
            Size = UDim2.new(1, -mergedProps.Size.X.Offset * 0.6, 1, 0),
            Position = UDim2.new(0.6, 0, 0, 0),
            TextColor3 = mergedProps.TextColor3,
            Font = mergedProps.Font,
            TextSize = mergedProps.TextSize,
            TextXAlignment = Enum.TextXAlignment.Left,
            BackgroundTransparency = 1,
        })
        label:GetInstance().ZIndex = toggleButton.ZIndex + 1

        self._state = mergedProps.InitialState
        self._track = track
        self._handle = handle
        self._onColor = mergedProps.OnColor
        self._offColor = mergedProps.BackgroundColor3

        local function updateVisualState()
            local targetXPosition = self._state and (1 - handle.Size.X.Scale) or 0
            local targetColor = self._state and self._onColor or self._offColor

            TweenService:Create(handle, TweenInfo.new(0.15, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
                Position = UDim2.new(targetXPosition, 0, 0.1, 0)
            }):Play()

            TweenService:Create(track, TweenInfo.new(0.15, Enum.EasingStyle.Quad, Enum.EasingDirection.Out), {
                BackgroundColor3 = targetColor
            }):Play()
        end

        updateVisualState()

        toggleButton.MouseButton1Click:Connect(function()
            self._state = not self._state
            updateVisualState()
            if self.onChanged then
                self.onChanged(self._state)
            end
        end)

        return self
    end

    function Toggle:GetState(): boolean
        return self._state
    end

    function Toggle:SetState(newState: boolean)
        if self._state ~= newState then
            self._state = newState
            local func = self.onChanged
            if func then
                self.onChanged = nil
                func(self._state)
                self.onChanged = func
            end
            local track = self._instance:FindFirstChild("ToggleTrack")
            local handle = track:FindFirstChild("ToggleHandle")
            local targetXPosition = self._state and (1 - handle.Size.X.Scale) or 0
            local targetColor = self._state and self._onColor or self._offColor
            
            handle.Position = UDim2.new(targetXPosition, 0, 0.1, 0)
            track.BackgroundColor3 = targetColor
        end
    end

    function Toggle:OnChanged(handler: (boolean) -> ())
        self.onChanged = handler
    end
    -- === End Toggle Module ===


    -- === TextBox Module ===
    local TextBox = {}
    TextBox.__index = TextBox
    setmetatable(TextBox, BaseComponent)

    local DEFAULT_TEXTBOX_PROPERTIES = {
        Size = UDim2.new(0, 200, 0, 30),
        BackgroundColor3 = Color3.fromRGB(50, 50, 50),
        TextColor3 = Color3.fromRGB(255, 255, 255),
        Text = "Enter text...",
        Font = Enum.Font.SourceSans,
        TextSize = 16,
        TextXAlignment = Enum.TextXAlignment.Left,
        TextYAlignment = Enum.TextYAlignment.Center,
        ClearTextOnFocus = true,
        CornerRadius = UDim.new(0, 6),
        StrokeColor = Color3.fromRGB(40, 40, 40),
        StrokeThickness = 1,
        Padding = UDim.new(0, 5),
    }

    function TextBox.new(parent: GuiObject, props: { [string]: any }): (any)
        local mergedProps = {}
        for k, v in pairs(DEFAULT_TEXTBOX_PROPERTIES) do mergedProps[k] = v end
        for k, v in pairs(props) do mergedProps[k] = v end

        local self = BaseComponent.new(Enum.Class.TextBox, parent, mergedProps)
        setmetatable(self, TextBox)

        local textBoxInstance = self._instance

        textBoxInstance.Changed:Connect(function(property)
            if property == "Text" and self.onTextChanged then
                self.onTextChanged(textBoxInstance.Text)
            end
        end)

        textBoxInstance.FocusLost:Connect(function(enterPressed)
            if self.onFocusLost then
                self.onFocusLost(enterPressed)
            end
            if enterPressed and self.onEnterPressed then
                self.onEnterPressed(textBoxInstance.Text)
            end
        end)

        textBoxInstance.Focused:Connect(function()
            if self.onFocused then
                self.onFocused()
            end
        end)

        return self
    end

    function TextBox:GetText(): string
        return self._instance.Text
    end

    function TextBox:OnTextChanged(handler: (text: string) -> ())
        self.onTextChanged = handler
    end

    function TextBox:OnFocusLost(handler: (enterPressed: boolean) -> ())
        self.onFocusLost = handler
    end

    function TextBox:OnEnterPressed(handler: (text: string) -> ())
        self.onEnterPressed = handler
    end

    function TextBox:OnFocused(handler: () -> ())
        self.onFocused = handler
    end
    -- === End TextBox Module ===


    -- === TabController Module ===
    local TabController = {}
    TabController.__index = TabController
    setmetatable(TabController, BaseComponent)

    local DEFAULT_TAB_CONTROLLER_PROPERTIES = {
        Size = UDim2.new(1, 0, 1, 0),
        TabButtonHeight = 35,
        TabButtonSpacing = 5,
        TabButtonBackgroundColor3 = Color3.fromRGB(50, 50, 50),
        TabButtonActiveColor3 = Color3.fromRGB(75, 150, 255),
        TabButtonTextColor3 = Color3.fromRGB(200, 200, 200),
        TabButtonActiveTextColor3 = Color3.fromRGB(255, 255, 255),
        BackgroundColor3 = Color3.fromRGB(30, 30, 30),
        CornerRadius = UDim.new(0, 8),
        StrokeColor = Color3.fromRGB(20, 20, 20),
        StrokeThickness = 1,
        Padding = UDim.new(0, 10),
        TextSize = 16,
        Font = Enum.Font.SourceSansBold,
        IconSize = UDim2.new(0, 20, 0, 20),
        IconPadding = 5,
    }

    function TabController.new(parent: GuiObject, props: { [string]: any }): (any)
        local mergedProps = {}
        for k, v in pairs(DEFAULT_TAB_CONTROLLER_PROPERTIES) do mergedProps[k] = v end
        for k, v in pairs(props) do mergedProps[k] = v end

        local self = BaseComponent.new(Enum.Class.Frame, parent, mergedProps)
        setmetatable(self, TabController)

        local mainFrame = self._instance
        mainFrame.ClipsDescendants = true

        local tabButtonsFrameProps = {
            Size = UDim2.new(1, 0, 0, mergedProps.TabButtonHeight),
            Position = UDim2.new(0, 0, 0, 0),
            BackgroundColor3 = Color3.fromRGB(25, 25, 25),
            BackgroundTransparency = 0,
            BorderSizePixel = 0,
            ZIndex = mainFrame.ZIndex + 1,
            CornerRadius = mergedProps.CornerRadius,
            StrokeColor = mergedProps.StrokeColor,
            StrokeThickness = mergedProps.StrokeThickness,
        }
        local tabButtonsFrame = BaseComponent.new(Enum.Class.Frame, mainFrame, tabButtonsFrameProps)
        self._tabButtonsFrame = tabButtonsFrame:GetInstance()

        local tabButtonLayout = Instance.new("UIListLayout")
        tabButtonLayout.FillDirection = Enum.FillDirection.Horizontal
        tabButtonLayout.HorizontalAlignment = Enum.HorizontalAlignment.Left
        tabButtonLayout.VerticalAlignment = Enum.VerticalAlignment.Center
        tabButtonLayout.Padding = UDim.new(0, mergedProps.TabButtonSpacing)
        tabButtonLayout.Parent = self._tabButtonsFrame

        local buttonFramePadding = Instance.new("UIPadding")
        buttonFramePadding.PaddingLeft = UDim.new(0, mergedProps.TabButtonSpacing)
        buttonFramePadding.PaddingRight = UDim.new(0, mergedProps.TabButtonSpacing)
        buttonFramePadding.Parent = self._tabButtonsFrame

        local contentFrameProps = {
            Size = UDim2.new(1, 0, 1, -mergedProps.TabButtonHeight),
            Position = UDim2.new(0, 0, 0, mergedProps.TabButtonHeight),
            BackgroundColor3 = mergedProps.BackgroundColor3,
            BackgroundTransparency = 0,
            BorderSizePixel = 0,
            ZIndex = mainFrame.ZIndex,
            Padding = mergedProps.Padding,
        }
        local contentFrame = BaseComponent.new(Enum.Class.Frame, mainFrame, contentFrameProps)
        self._contentFrame = contentFrame:GetInstance()

        self._tabs = {}
        self._activeTabName = nil
        self._mergedProps = mergedProps

        return self
    end

    function TabController:AddTab(tabName: string, iconAssetId: number | nil): GuiObject
        if self._tabs[tabName] then
            warn("Tab with name '" .. tabName .. "' already exists!")
            return self._tabs[tabName].content
        end

        local tabButton = Instance.new("TextButton")
        tabButton.Name = tabName .. "TabButton"
        tabButton.Size = UDim2.new(0, 0, 1, 0)
        tabButton.BackgroundColor3 = self._mergedProps.TabButtonBackgroundColor3
        tabButton.TextColor3 = self._mergedProps.TabButtonTextColor3
        tabButton.BackgroundTransparency = 0
        tabButton.BorderSizePixel = 0
        tabButton.AutoBu            



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
