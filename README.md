print("no source stealer please, this script genv by 992 deobf discord.gg/9ybcc9bM63 No remove watermark please!!! DEOBF BY 073!!!")
print("no source stealer please, this script genv by 992 deobf discord.gg/9ybcc9bM63 No remove watermark please!!! DEOBF BY 073!!!")
print("no source stealer please, this script genv by 992 deobf discord.gg/9ybcc9bM63 No remove watermark please!!! DEOBF BY 073!!!")
print("no source stealer please, this script genv by 992 deobf discord.gg/9ybcc9bM63 No remove watermark please!!! DEOBF BY 073!!!")
print("no source stealer please, this script genv by 992 deobf discord.gg/9ybcc9bM63 No remove watermark please!!! DEOBF BY 073!!!")
print("no source stealer please, this script genv by 992 deobf discord.gg/9ybcc9bM63 No remove watermark please!!! DEOBF BY 073!!!")
print("no source stealer please, this script genv by 992 deobf discord.gg/9ybcc9bM63 No remove watermark please!!! DEOBF BY 073!!!")
print("no source stealer please, this script genv by 992 deobf discord.gg/9ybcc9bM63 No remove watermark please!!! DEOBF BY 073!!!")
print("no source stealer please, this script genv by 992 deobf discord.gg/9ybcc9bM63 No remove watermark please!!! DEOBF BY 073!!!")
print("no source stealer please, this script genv by 992 deobf discord.gg/9ybcc9bM63 No remove watermark please!!! DEOBF BY 073!!!")
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local UserInputService = game:GetService("UserInputService")
local TweenService = game:GetService("TweenService")
local CoreGui = game:GetService("CoreGui")
local Workspace = game:GetService("Workspace")
local Lighting = game:GetService("Lighting")
local ReplicatedStorage = game:GetService("ReplicatedStorage")

local LP = Players.LocalPlayer
local playerGui = LP:WaitForChild("PlayerGui")

-- ============================================================
-- RUNTIME
-- ============================================================
local environment = (getgenv and getgenv()) or _G
local RUNTIME_KEY = "__WEEKLY_CODE_SNIPER_RUNTIME"
local previous = environment[RUNTIME_KEY]
if type(previous) == "table" and type(previous.destroy) == "function" then
    pcall(previous.destroy)
end

local runtime = {
    alive = true,
    enabled = false,
    snipeKey = Enum.KeyCode.Z,
    listeningKey = false,
    connections = {},
    spamCount = 20,
    submitAfter = 1,
    autoSubmit = false,
    spamRedeem = true,
    antiRagdoll = false,
    autoBuy = false,
    antiLag = false,
    removeAccessories = false,
    gui = nil,
    settingsGui = nil,
    headDisplay = nil,
    lastCode = nil,
    spamLoopActive = false,
    notifConn = nil,
    notifyRemote = nil,
    seen = {},
    capturedParts = {},
}
environment[RUNTIME_KEY] = runtime

-- ============================================================
-- HELPERS
-- ============================================================
local function disconnect(conn)
    if conn then pcall(function() conn:Disconnect() end) end
end

local function connect(signal, callback)
    local c = signal:Connect(callback)
    table.insert(runtime.connections, c)
    return c
end

local function new(className, props, parent)
    local obj = Instance.new(className)
    for k, v in pairs(props or {}) do
        obj[k] = v
    end
    if parent then obj.Parent = parent end
    return obj
end

local function corner(parent, radius)
    return new("UICorner", {
        CornerRadius = typeof(radius) == "UDim" and radius or UDim.new(0, radius),
    }, parent)
end

local function stroke(parent, color, transparency, thickness)
    return new("UIStroke", {
        ApplyStrokeMode = Enum.ApplyStrokeMode.Border,
        Color = color,
        Transparency = transparency or 0,
        Thickness = thickness or 1,
    }, parent)
end

-- ============================================================
-- BACKGROUND PADRÃO
-- ============================================================
local BG_IMAGE = "rbxassetid://70418952815837"
local BG_BASE_COLOR = Color3.fromRGB(15, 10, 25)

local function applyBackground(parent, cornerRadius)
    local bgFrame = new("Frame", {
        Name = parent.Name .. "Background",
        Size = UDim2.new(1, 0, 1, 0),
        Position = UDim2.new(0, 0, 0, 0),
        BackgroundColor3 = BG_BASE_COLOR,
        BorderSizePixel = 0,
        ZIndex = 0,
    }, parent)
    corner(bgFrame, cornerRadius)

    new("UIGradient", {
        Color = ColorSequence.new({
            ColorSequenceKeypoint.new(0.00, Color3.fromRGB(25, 10, 40)),
            ColorSequenceKeypoint.new(0.50, Color3.fromRGB(15, 8, 25)),
            ColorSequenceKeypoint.new(1.00, Color3.fromRGB(30, 12, 45)),
        }),
        Rotation = 45,
    }, bgFrame)

    local bgImage = new("ImageLabel", {
        Name = parent.Name .. "BackgroundImage",
        Size = UDim2.new(1, 0, 1, 0),
        Position = UDim2.new(0, 0, 0, 0),
        BackgroundTransparency = 1,
        Image = BG_IMAGE,
        ImageTransparency = 0.3,
        ScaleType = Enum.ScaleType.Stretch,
        ZIndex = 1,
    }, bgFrame)
    corner(bgImage, cornerRadius)

    return bgFrame, bgImage
end

-- ============================================================
-- HEAD DISPLAY
-- ============================================================
local function createHeadDisplay()
    local char = LP.Character
    if not char then return end
    local head = char:FindFirstChild("Head")
    if not head then return end

    local old = head:FindFirstChild("WeeklyCodeSniperHeadDisplay")
    if old then old:Destroy() end

    local billboard = new("BillboardGui", {
        Name = "WeeklyCodeSniperHeadDisplay",
        Adornee = head,
        Size = UDim2.new(0, 200, 0, 30),
        StudsOffset = Vector3.new(0, 2.5, 0),
        MaxDistance = 100,
        AlwaysOnTop = true,
    }, head)

    new("TextLabel", {
        Size = UDim2.new(1, 0, 1, 0),
        BackgroundTransparency = 1,
        Text = "this code sniper deobf full by 073 992 discord.gg/9ybcc9bM63",
        TextColor3 = Color3.new(1, 1, 1),
        TextSize = 18,
        Font = Enum.Font.GothamBold,
        TextScaled = true,
        TextXAlignment = Enum.TextXAlignment.Center,
        TextYAlignment = Enum.TextYAlignment.Center,
    }, billboard)

    runtime.headDisplay = billboard
end

-- ============================================================
-- GUI DETECTION
-- ============================================================
local function isOurGui(instance)
    local p = instance
    for _ = 1, 10 do
        if not p then break end
        if p.Name == "WeeklyCodeSniperUI" or p.Name == "WeeklySettingsUI" then return true end
        p = p.Parent
    end
    return false
end

local function isVisibleChain(inst)
    local current = inst
    while current do
        if current:IsA("GuiObject") and not current.Visible then return false end
        if current:IsA("ScreenGui") then return current.Enabled end
        current = current.Parent
    end
    return true
end

local function findAllTextBoxes(pg)
    local boxes = {}
    for _, gui in ipairs(pg:GetChildren()) do
        if gui:IsA("ScreenGui") and gui.Enabled and not isOurGui(gui) then
            for _, d in ipairs(gui:GetDescendants()) do
                if d:IsA("TextBox") and not isOurGui(d) then
                    boxes[#boxes+1] = d
                end
            end
        end
    end
    return boxes
end

local function findCodeBox()
    local pg = playerGui
    if not pg then return nil end
    local allBoxes = findAllTextBoxes(pg)
    for _, box in ipairs(allBoxes) do
        if isVisibleChain(box) then
            local n  = box.Name:lower()
            local pn = (box.Parent and box.Parent.Name or ""):lower()
            if n:find("code") or pn:find("code") or n:find("redeem") or pn:find("redeem") or n:find("input") or n:find("enter") then
                return box
            end
        end
    end
    for _, box in ipairs(allBoxes) do
        if isVisibleChain(box) then return box end
    end
    return nil
end

local function findSubmitButton(box)
    local pg = playerGui
    if not pg then return nil end

    local searchNames = {"submit","redeem","claim","confirm","enter","send","apply","ok","use","go","check"}

    if box then
        local p = box.Parent
        for _ = 1, 6 do
            if not p then break end
            for _, d in ipairs(p:GetDescendants()) do
                if (d:IsA("TextButton") or d:IsA("ImageButton")) and not isOurGui(d) and d ~= box then
                    local n = d.Name:lower()
                    local txt = ""
                    pcall(function() txt = d.Text:lower() end)
                    for _, sn in ipairs(searchNames) do
                        if (n:find(sn) or txt:find(sn)) and isVisibleChain(d) then
                            return d
                        end
                    end
                end
            end
            p = p.Parent
        end
    end

    local btns = {}
    for _, gui in ipairs(pg:GetChildren()) do
        if gui:IsA("ScreenGui") and gui.Enabled and not isOurGui(gui) then
            for _, d in ipairs(gui:GetDescendants()) do
                if (d:IsA("TextButton") or d:IsA("ImageButton")) and not isOurGui(d) then
                    local n = d.Name:lower()
                    local txt = ""
                    pcall(function() txt = d.Text:lower() end)
                    for _, sn in ipairs(searchNames) do
                        if (n:find(sn) or txt:find(sn)) and isVisibleChain(d) then
                            table.insert(btns, d)
                            break
                        end
                    end
                end
            end
        end
    end
    return btns[1]
end

local getupvalues = (debug and debug.getupvalues) or getupvalues
local getconns    = getconnections or (debug and debug.getconnections)
local setupv      = (debug and debug.setupvalue) or setupvalue

local function clickButton(btn)
    if not btn then return false end
    local anyOk = false
    local methods = {
        function() btn.MouseButton1Click:Fire() end,
        function() btn.Activated:Fire() end,
    }
    if typeof(firesignal) == "function" then
        table.insert(methods, function() firesignal(btn.MouseButton1Click) end)
        table.insert(methods, function() firesignal(btn.Activated) end)
    end
    if typeof(getconns) == "function" then
        table.insert(methods, function()
            local ok, cs = pcall(getconns, btn.MouseButton1Click)
            if ok and type(cs) == "table" then
                for _, c in ipairs(cs) do pcall(function() c:Fire() end) end
            end
            local ok2, cs2 = pcall(getconns, btn.Activated)
            if ok2 and type(cs2) == "table" then
                for _, c in ipairs(cs2) do pcall(function() c:Fire() end) end
            end
        end)
    end
    if typeof(fireclick) == "function" then
        table.insert(methods, function() fireclick(btn) end)
    end
    for _, fn in ipairs(methods) do
        local ok = pcall(fn)
        anyOk = anyOk or ok
    end
    return anyOk
end

local function fireBoxFocusLost(box)
    if not box then return false end
    local anyFired = false
    if typeof(firesignal) == "function" then
        anyFired = anyFired or pcall(firesignal, box.FocusLost, true)
    end
    if typeof(getconns) == "function" then
        local ok, cs = pcall(getconns, box.FocusLost)
        if ok and type(cs) == "table" then
            for _, c in ipairs(cs) do
                local fn
                pcall(function() fn = c.Function end)
                if fn and typeof(getupvalues) == "function" and typeof(setupv) == "function" then
                    local uOk, ups = pcall(getupvalues, fn)
                    if uOk and type(ups) == "table" then
                        for i, v in pairs(ups) do
                            if type(v) == "boolean" and v == true then
                                pcall(setupv, fn, i, false)
                            end
                        end
                    end
                end
                local fOk = pcall(function()
                    if c.Enabled ~= false then c:Fire(true) end
                end)
                anyFired = anyFired or fOk
            end
        end
    end
    return anyFired
end

-- ============================================================
-- REDEEM
-- ============================================================
local function redeemCode(code)
    if not code or code == "" then return false, "no code" end

    local box = findCodeBox()
    if not box then return false, "no code box" end

    pcall(function() box.Text = code end)

    local submitBtn = findSubmitButton(box)
    if not submitBtn then
        fireBoxFocusLost(box)
        return false, "no submit button"
    end

    if runtime.spamRedeem then
        local n = math.clamp(runtime.spamCount, 1, 100)
        for i = 1, n do
            if not runtime.alive then break end
            clickButton(submitBtn)
            task.wait(0.0001)
        end
    else
        clickButton(submitBtn)
    end

    fireBoxFocusLost(box)
    return true, "submitted"
end

local function startSpamRedeem(code)
    if runtime.spamLoopActive then return end
    runtime.spamLoopActive = true

    local count = math.clamp(runtime.spamCount, 1, 100)
    local success = 0

    consoleLog(string.format(
        "<font color='rgb(105,190,132)'>Spamming %d vezes...</font>",
        count
    ))

    task.spawn(function()
        for i = 1, count do
            if not runtime.alive or not runtime.spamLoopActive then break end
            local ok = redeemCode(code)
            if ok then success = success + 1 end
            if i % 5 == 0 or i == count then
                consoleLog(string.format(
                    "<font color='rgb(105,190,132)'>Spam %d/%d</font> <font color='rgb(150,150,150)'>(%d ok)</font>",
                    i, count, success
                ))
            end
            task.wait(0.0001)
        end
        consoleLog(string.format(
            "<font color='rgb(105,190,132)'>Spam finalizado: %d/%d</font>",
            success, count
        ))
        runtime.spamLoopActive = false
    end)
end

-- ============================================================
-- NOTIFICATION LISTENER (Submit After funcional)
-- ============================================================
local function resolveNotifyRemote()
    if runtime.notifyRemote and runtime.notifyRemote.Parent then
        return runtime.notifyRemote
    end
    local candidates = {}
    pcall(function()
        for _, d in ipairs(ReplicatedStorage:GetDescendants()) do
            if d:IsA("RemoteEvent") then
                local n = d.Name:lower()
                if n:find("notif") or n:find("announce") or n:find("broadcast")
                or n:find("global") or n:find("message") or n:find("chat") then
                    table.insert(candidates, d)
                end
            end
        end
    end)
    if #candidates > 0 then
        runtime.notifyRemote = candidates[1]
        return candidates[1]
    end
    return nil
end

local function stripRich(text)
    if type(text) ~= "string" then return tostring(text) end
    return (text:gsub("<[^>]->", ""))
end

local function tokenize(text)
    local words = {}
    for word in text:gmatch("[%w_]+") do
        words[#words + 1] = word
    end
    return words
end

local function onAnnouncement(...)
    if not runtime.enabled then return end

    local text = stripRich(tostring((...) or ""))
    text = text:match("^%s*(.-)%s*$") or ""
    if text == "" then return end
    if text:find("%s") then return end

    if runtime.seen[text] then return end
    runtime.seen[text] = true
    task.delay(1.25, function() runtime.seen[text] = nil end)

    for _, word in ipairs(tokenize(text)) do
        table.insert(runtime.capturedParts, word)
    end

    local capturedCount = #runtime.capturedParts
    local joined = table.concat(runtime.capturedParts)

    consoleLog(string.format(
        "<font color='rgb(105,190,132)'>Captured %d/%d</font> <font color='rgb(150,150,150)'>[%s]</font>",
        capturedCount,
        runtime.submitAfter,
        joined
    ))

    if capturedCount >= runtime.submitAfter then
        runtime.lastCode = joined
        runtime.capturedParts = {}

        consoleLog("<font color='rgb(105,190,132)'>Submitting: " .. joined .. "</font>")

        if runtime.autoSubmit then
            task.spawn(function()
                local ok, err = redeemCode(joined)
                if ok then
                    consoleLog("<font color='rgb(105,190,132)'>Redeemed: " .. joined .. "</font>")
                else
                    consoleLog("<font color='rgb(150,150,150)'>Failed: " .. tostring(err) .. "</font>")
                end
            end)
        end
    end
end

local function startNotifListener()
    if runtime.notifConn then return end
    local remote = resolveNotifyRemote()
    if remote then
        runtime.notifConn = remote.OnClientEvent:Connect(function(...)
            pcall(onAnnouncement, ...)
        end)
    end
end

local function stopNotifListener()
    if runtime.notifConn then
        disconnect(runtime.notifConn)
        runtime.notifConn = nil
    end
end

-- ============================================================
-- MAIN UI
-- ============================================================
local parentGui = (gethui and gethui()) or CoreGui
local oldGui = parentGui:FindFirstChild("WeeklyCodeSniperUI")
if oldGui then oldGui:Destroy() end

local ScreenGui = new("ScreenGui", {
    Name = "WeeklyCodeSniperUI",
    ResetOnSpawn = false,
    IgnoreGuiInset = true,
    DisplayOrder = 999,
}, parentGui)
runtime.gui = ScreenGui

local Window = new("Frame", {
    Name = "Window",
    Size = UDim2.new(0, 270, 0, 400),
    AnchorPoint = Vector2.new(1, 0),
    Position = UDim2.new(1, -8, 0, 8),
    BackgroundColor3 = BG_BASE_COLOR,
    BorderSizePixel = 0,
    ClipsDescendants = true,
    Active = true,
}, ScreenGui)
corner(Window, 14)
new("UIScale", { Name = "InterfaceScale", Scale = 0.92 }, Window)
applyBackground(Window, 14)

-- Header
local Header = new("Frame", {
    Name = "Header",
    Size = UDim2.new(1, 0, 0, 56),
    BackgroundTransparency = 1,
    Active = true,
    ZIndex = 3,
}, Window)

local Avatar = new("ImageLabel", {
    Name = "AvatarImage",
    Size = UDim2.new(0, 32, 0, 32),
    Position = UDim2.new(0, 8, 0, 12),
    BackgroundTransparency = 1,
    ZIndex = 3,
}, Header)
corner(Avatar, 16)

new("TextLabel", {
    Name = "Title",
    Size = UDim2.new(0, 140, 0, 30),
    Position = UDim2.new(0, 46, 0, 8),
    BackgroundTransparency = 1,
    Text = "Weekly Code Sniper",
    TextSize = 16,
    TextColor3 = Color3.fromRGB(180, 180, 190),
    TextXAlignment = Enum.TextXAlignment.Left,
    Font = Enum.Font.GothamBold,
    ZIndex = 3,
}, Header)

local KeybindsBtn = new("TextButton", {
    Name = "KeybindsButton",
    Size = UDim2.new(0, 62, 0, 24),
    Position = UDim2.new(1, -95, 0, 16),
    BackgroundColor3 = Color3.fromRGB(26, 26, 32),
    BackgroundTransparency = 0.15,
    BorderSizePixel = 0,
    AutoButtonColor = false,
    Text = "Keybinds",
    TextSize = 11,
    TextColor3 = Color3.fromRGB(200, 200, 210),
    Font = Enum.Font.GothamBold,
    ZIndex = 5,
}, Header)
corner(KeybindsBtn, 6)
stroke(KeybindsBtn, Color3.fromRGB(85, 85, 95), 0.4)

local MinimizeBtn = new("TextButton", {
    Name = "MinimizeBtn",
    Size = UDim2.new(0, 20, 0, 24),
    Position = UDim2.new(1, -26, 0, 16),
    BackgroundColor3 = Color3.fromRGB(26, 26, 32),
    BackgroundTransparency = 0.15,
    BorderSizePixel = 0,
    AutoButtonColor = false,
    Text = "-",
    TextSize = 16,
    TextColor3 = Color3.new(1, 1, 1),
    Font = Enum.Font.GothamBold,
    ZIndex = 5,
}, Header)
corner(MinimizeBtn, 6)
stroke(MinimizeBtn, Color3.fromRGB(85, 85, 95), 0.4)

-- Console
local Console = new("ScrollingFrame", {
    Name = "Cons
