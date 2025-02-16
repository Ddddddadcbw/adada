print("A")
local ArrayField = loadstring(game:HttpGet('https://raw.githubusercontent.com/UI-Interface/ArrayField/main/Source.lua'))()
getgenv().SecureMode = true
local Rayfield = loadstring(game:HttpGet(('https://raw.githubusercontent.com/Ddddddadcbw/ddad/refs/heads/main/README.md')))()
local Window = Rayfield:CreateWindow({
    Name = "Whale Hub",
    LoadingTitle = "Whale",
    LoadingSubtitle = "by Whale Hub Devloper team",
    ConfigurationSaving = {
        Enabled = false,
        FolderName = nil,  -- 기본 폴더 사용
        FileName = "IWV Hub"
    },
    KeySystem = false, -- 기본 키 시스템 비활성화
    KeySettings = {
        Title = "IWV",
        Subtitle = "Key System",
        Note = "IWV Script 2024-08-31\nACS Engine & CE Engine 부대테러",
        FileName = "Key",
        SaveKey = false,
        GrabKeyFromSite = false,
        Key = {[1] = "12345678"} -- 이 필드는 비워두고 직접 검증 로직 구현
    }
})
---------------------- 기본기능 함수 모음 -----------------------
local Players = game:GetService("Players")
local UserInputService = game:GetService("UserInputService")
local Player = Players.LocalPlayer
local ReplicatedStorage = game:GetService("ReplicatedStorage")
local RunService = game:GetService("RunService")
local camera = workspace.CurrentCamera

-- 밥밥부대 탈옥감지 무력화
local lineObject = game:GetService("ReplicatedStorage"):FindFirstChild("line")

if lineObject then
    lineObject:Destroy() -- line 객체를 제거합니다.
end

-- 플래그 및 상태 변수
local isKeyValid = false
local dragging = false
local dragInput, mousePos, framePos
local teleporting = false
local isFlying = false
local spinSpeed = 2
local flySpeed = 2
local spinning = false
local noclip = false
local userName = ""
local espEnabled = false
local espObjects = {}

-- 기능 플래그
local CurrentValue = false
local flyEnabled = false
local noclipEnabled = false
local spinEnabled = false
local KillAuraEnabled = false
local AimBotEnabled = false
local StaticCuffEnabled = false
local TpKillEnabled = false
local swordAttackEnabled = false -- swordattack 기능 상태
local increaseSize = false -- 작대기 크기 증가 상태
local swordEquipEnabled = false  -- 검 꺼내기 상태
local swordSizeIncreaseEnabled = false  -- 검 크기 키우기 상태

-- Aimbot 관련 변수
local aimAssistEnabled = false
local currentTarget = nil
local renderSteppedConnection
local frame = nil
local uiStroke = nil
local colors = {
    Color3.fromRGB(255, 0, 0),
    Color3.fromRGB(255, 165, 0),
    Color3.fromRGB(255, 255, 0),
    Color3.fromRGB(0, 255, 0),
    Color3.fromRGB(0, 0, 255),
    Color3.fromRGB(128, 0, 128),
    Color3.fromRGB(0, 0, 0)
}
local baseSize = 100
local sizeIncrement = 50
local currentColorIndex = 1

local ToggleFly, ToggleNoclip, ToggleESP, ToggleSpin, ToggleKillAura, ToggleAimBot, ToggleStaticCuff, ToggleTpKill
local ToggleWeaponSize, ToggleSwordAttack

------------------------ 키 인증 부분 -------------------------------
-- Pastebin 링크
local dataUrl = 'https://pastebin.com/raw/HFHDpMQR'

-- 데이터를 가져오는 코드
local success, response = pcall(function()
    local jsonData = game:HttpGet(dataUrl)
    return HttpService:JSONDecode(jsonData)
end)

if success then
    keysData = response
else
    warn("Failed to fetch JSON data: " .. tostring(response))
end
------------------------ 키 인증 부분 -------------------------------
local Players = game:GetService("Players")

-- 함수: 유저 ID나 유저 닉네임의 일부를 통해 플레이어의 디스플레이 닉네임을 찾습니다.
local function findPlayerDisplayName(identifier)
    for _, player in ipairs(Players:GetPlayers()) do
        -- 유저 닉네임의 일부로 검색 (identifier가 문자열일 때)
        if string.find(string.lower(player.Name), string.lower(identifier)) then
            return player.Name
        end
    end
end

--------------------------------------- 은신 기능 ----------------------------------------------------
-- 서비스와 플레이어 참조
local players = game:GetService("Players")
local runService = game:GetService("RunService")
local userInputService = game:GetService("UserInputService")
local localPlayer = players.LocalPlayer
local camera = workspace.CurrentCamera

function Transparency_toggle_bt(value)
    --Settings:
    local ScriptStarted = false
    local Keybind = "H" --Set to whatever you want, has to be the name of a KeyCode Enum.
    local Transparency = true --Will make you slightly transparent when you are invisible. No reason to disable.
    local NoClip = false --Will make your fake character no clip.

    local Player = game:GetService("Players").LocalPlayer
    local RealCharacter = Player.Character or Player.CharacterAdded:Wait()

    local IsInvisible = false

    RealCharacter.Archivable = true
    local FakeCharacter = RealCharacter:Clone()
    local Part
    Part = Instance.new("Part", workspace)
    Part.Anchored = true
    Part.Size = Vector3.new(200, 1, 200)
    Part.CFrame = CFrame.new(0, -500, 0) --Set this to whatever you want, just far away from the map.
    Part.CanCollide = true
    FakeCharacter.Parent = workspace
    FakeCharacter.HumanoidRootPart.CFrame = Part.CFrame * CFrame.new(0, 5, 0)

    for i, v in pairs(RealCharacter:GetChildren()) do
    if v:IsA("LocalScript") then
        local clone = v:Clone()
        clone.Disabled = true
        clone.Parent = FakeCharacter
    end
    end
    if Transparency then
    for i, v in pairs(FakeCharacter:GetDescendants()) do
        if v:IsA("BasePart") then
            v.Transparency = 0.7
        end
    end
    end
    local CanInvis = true
    function RealCharacterDied()
    CanInvis = false
    RealCharacter:Destroy()
    RealCharacter = Player.Character
    CanInvis = true
    isinvisible = false
    FakeCharacter:Destroy()
    workspace.CurrentCamera.CameraSubject = RealCharacter.Humanoid

    RealCharacter.Archivable = true
    FakeCharacter = RealCharacter:Clone()
    Part:Destroy()
    Part = Instance.new("Part", workspace)
    Part.Anchored = true
    Part.Size = Vector3.new(200, 1, 200)
    Part.CFrame = CFrame.new(9999, 9999, 9999) --Set this to whatever you want, just far away from the map.
    Part.CanCollide = true
    FakeCharacter.Parent = workspace
    FakeCharacter.HumanoidRootPart.CFrame = Part.CFrame * CFrame.new(0, 5, 0)

    for i, v in pairs(RealCharacter:GetChildren()) do
        if v:IsA("LocalScript") then
            local clone = v:Clone()
            clone.Disabled = true
            clone.Parent = FakeCharacter
        end
    end
    if Transparency then
        for i, v in pairs(FakeCharacter:GetDescendants()) do
            if v:IsA("BasePart") then
                v.Transparency = 0.7
            end
        end
    end
    RealCharacter.Humanoid.Died:Connect(function()
    RealCharacter:Destroy()
    FakeCharacter:Destroy()
    end)
    Player.CharacterAppearanceLoaded:Connect(RealCharacterDied)
    end
    RealCharacter.Humanoid.Died:Connect(function()
    RealCharacter:Destroy()
    FakeCharacter:Destroy()
    end)
    Player.CharacterAppearanceLoaded:Connect(RealCharacterDied)
    local PseudoAnchor
    game:GetService "RunService".RenderStepped:Connect(
    function()
        if PseudoAnchor ~= nil then
            PseudoAnchor.CFrame = Part.CFrame * CFrame.new(0, 5, 0)
        end
        if NoClip then
        FakeCharacter.Humanoid:ChangeState(11)
        end
    end
    )

    PseudoAnchor = FakeCharacter.HumanoidRootPart
    local function Invisible()
    if IsInvisible == false then
        local StoredCF = RealCharacter.HumanoidRootPart.CFrame
        RealCharacter.HumanoidRootPart.CFrame = FakeCharacter.HumanoidRootPart.CFrame
        FakeCharacter.HumanoidRootPart.CFrame = StoredCF
        RealCharacter.Humanoid:UnequipTools()
        Player.Character = FakeCharacter
        workspace.CurrentCamera.CameraSubject = FakeCharacter.Humanoid
        PseudoAnchor = RealCharacter.HumanoidRootPart
        for i, v in pairs(FakeCharacter:GetChildren()) do
            if v:IsA("LocalScript") then
                v.Disabled = false
            end
        end

        IsInvisible = true
    else
        local StoredCF = FakeCharacter.HumanoidRootPart.CFrame
        FakeCharacter.HumanoidRootPart.CFrame = RealCharacter.HumanoidRootPart.CFrame
        
        RealCharacter.HumanoidRootPart.CFrame = StoredCF
        
        FakeCharacter.Humanoid:UnequipTools()
        Player.Character = RealCharacter
        workspace.CurrentCamera.CameraSubject = RealCharacter.Humanoid
        PseudoAnchor = FakeCharacter.HumanoidRootPart
        for i, v in pairs(FakeCharacter:GetChildren()) do
            if v:IsA("LocalScript") then
                v.Disabled = true
            end
        end
        IsInvisible = false
    end
    end

    game:GetService("UserInputService").InputBegan:Connect(
    function(key, gamep)
        if gamep then
            return
        end
        if key.KeyCode.Name:lower() == Keybind:lower() and CanInvis and RealCharacter and FakeCharacter then
            if RealCharacter:FindFirstChild("HumanoidRootPart") and FakeCharacter:FindFirstChild("HumanoidRootPart") then
                Invisible()
            end
        end
    end
    )
    local Sound = Instance.new("Sound",game:GetService("SoundService"))
    Sound.SoundId = "rbxassetid://232127604"
    Sound:Play()
    game:GetService("StarterGui"):SetCore("SendNotification",{["Title"] = "Invisible Toggle Loaded",["Text"] = "Press "..Keybind.." to become change visibility.",["Duration"] = 20,["Button1"] = "Okay."})
end
--------------------------------------- 은신 기능 ----------------------------------------------------
--kill용 은신
function Transparency_toggle_bt_kill()
    --Settings:
    local ScriptStarted = false
    local Keybind = "H" --Set to whatever you want, has to be the name of a KeyCode Enum.
    local Transparency = true --Will make you slightly transparent when you are invisible. No reason to disable.
    local NoClip = false --Will make your fake character no clip.

    local Player = game:GetService("Players").LocalPlayer
    local RealCharacter = Player.Character or Player.CharacterAdded:Wait()

    local IsInvisible = false

    RealCharacter.Archivable = true
    local FakeCharacter = RealCharacter:Clone()
    local Part
    Part = Instance.new("Part", workspace)
    Part.Anchored = true
    Part.Size = Vector3.new(200, 1, 200)
    Part.CFrame = CFrame.new(0, -1000, 0) --Set this to whatever you want, just far away from the map.
    Part.CanCollide = true
    FakeCharacter.Parent = workspace
    FakeCharacter.HumanoidRootPart.CFrame = Part.CFrame * CFrame.new(0, 5, 0)

    for i, v in pairs(RealCharacter:GetChildren()) do
    if v:IsA("LocalScript") then
        local clone = v:Clone()
        clone.Disabled = true
        clone.Parent = FakeCharacter
    end
    end
    if Transparency then
    for i, v in pairs(FakeCharacter:GetDescendants()) do
        if v:IsA("BasePart") then
            v.Transparency = 0.7
        end
    end
    end
    local CanInvis = true
    function RealCharacterDied()
    CanInvis = false
    RealCharacter:Destroy()
    RealCharacter = Player.Character
    CanInvis = true
    isinvisible = false
    FakeCharacter:Destroy()
    workspace.CurrentCamera.CameraSubject = RealCharacter.Humanoid

    RealCharacter.Archivable = true
    FakeCharacter = RealCharacter:Clone()
    Part:Destroy()
    Part = Instance.new("Part", workspace)
    Part.Anchored = true
    Part.Size = Vector3.new(200, 1, 200)
    Part.CFrame = CFrame.new(9999, 9999, 9999) --Set this to whatever you want, just far away from the map.
    Part.CanCollide = true
    FakeCharacter.Parent = workspace
    FakeCharacter.HumanoidRootPart.CFrame = Part.CFrame * CFrame.new(0, 5, 0)

    for i, v in pairs(RealCharacter:GetChildren()) do
        if v:IsA("LocalScript") then
            local clone = v:Clone()
            clone.Disabled = true
            clone.Parent = FakeCharacter
        end
    end
    if Transparency then
        for i, v in pairs(FakeCharacter:GetDescendants()) do
            if v:IsA("BasePart") then
                v.Transparency = 0.7
            end
        end
    end
    RealCharacter.Humanoid.Died:Connect(function()
    RealCharacter:Destroy()
    FakeCharacter:Destroy()
    end)
    Player.CharacterAppearanceLoaded:Connect(RealCharacterDied)
    end
    RealCharacter.Humanoid.Died:Connect(function()
    RealCharacter:Destroy()
    FakeCharacter:Destroy()
    end)
    Player.CharacterAppearanceLoaded:Connect(RealCharacterDied)
    local PseudoAnchor
    game:GetService "RunService".RenderStepped:Connect(
    function()
        if PseudoAnchor ~= nil then
            PseudoAnchor.CFrame = Part.CFrame * CFrame.new(0, 5, 0)
        end
        if NoClip then
        FakeCharacter.Humanoid:ChangeState(11)
        end
    end
    )

    PseudoAnchor = FakeCharacter.HumanoidRootPart
    local function Invisible()
    if IsInvisible == false then
        local StoredCF = RealCharacter.HumanoidRootPart.CFrame
        RealCharacter.HumanoidRootPart.CFrame = FakeCharacter.HumanoidRootPart.CFrame
        FakeCharacter.HumanoidRootPart.CFrame = StoredCF
        RealCharacter.Humanoid:UnequipTools()
        Player.Character = FakeCharacter
        workspace.CurrentCamera.CameraSubject = FakeCharacter.Humanoid
        PseudoAnchor = RealCharacter.HumanoidRootPart
        for i, v in pairs(FakeCharacter:GetChildren()) do
            if v:IsA("LocalScript") then
                v.Disabled = false
            end
        end

        IsInvisible = true
    else
        local StoredCF = FakeCharacter.HumanoidRootPart.CFrame
        FakeCharacter.HumanoidRootPart.CFrame = RealCharacter.HumanoidRootPart.CFrame
        
        RealCharacter.HumanoidRootPart.CFrame = StoredCF
        
        FakeCharacter.Humanoid:UnequipTools()
        Player.Character = RealCharacter
        workspace.CurrentCamera.CameraSubject = RealCharacter.Humanoid
        PseudoAnchor = FakeCharacter.HumanoidRootPart
        for i, v in pairs(FakeCharacter:GetChildren()) do
            if v:IsA("LocalScript") then
                v.Disabled = true
            end
        end
        IsInvisible = false
    end
    end

    game:GetService("UserInputService").InputBegan:Connect(
    function(key, gamep)
        if gamep then
            return
        end
        if key.KeyCode.Name:lower() == Keybind:lower() and CanInvis and RealCharacter and FakeCharacter then
            if RealCharacter:FindFirstChild("HumanoidRootPart") and FakeCharacter:FindFirstChild("HumanoidRootPart") then
                Invisible()
            end
        end
    end
    )
    local Sound = Instance.new("Sound",game:GetService("SoundService"))
    Sound.SoundId = "rbxassetid://232127604"
    Sound:Play()
    game:GetService("StarterGui"):SetCore("SendNotification",{["Title"] = "Invisible Toggle Loaded",["Text"] = "Press "..Keybind.." to become change visibility.",["Duration"] = 20,["Button1"] = "Okay."})
end
---------------------- 킬용 은신 ------------------------
---------------------- 특정 플레이어 날리기 --------------------
-- 메인 함수 정의
function Select_Fling_Player(username)
    local Players = game:GetService("Players")
    local Player = Players.LocalPlayer

    local AllBool = false

    local function Message(_Title, _Text, Time)
        game:GetService("StarterGui"):SetCore("SendNotification", {Title = _Title, Text = _Text, Duration = Time})
    end

    local function GetPlayer(Name)
        Name = Name:lower()
        if Name == "all" or Name == "others" then
            AllBool = true
            return nil
        elseif Name == "random" then
            local GetPlayers = Players:GetPlayers()
            if table.find(GetPlayers, Player) then table.remove(GetPlayers, table.find(GetPlayers, Player)) end
            return GetPlayers[math.random(#GetPlayers)]
        else
            for _, x in next, Players:GetPlayers() do
                if x ~= Player then
                    if x.Name:lower():match("^" .. Name) or x.DisplayName:lower():match("^" .. Name) then
                        return x
                    end
                end
            end
        end
        return nil
    end

    local function SkidFling(TargetPlayer)
        local Character = Player.Character
        local Humanoid = Character and Character:FindFirstChildOfClass("Humanoid")
        local RootPart = Humanoid and Humanoid.RootPart

        local TCharacter = TargetPlayer.Character
        local THumanoid
        local TRootPart
        local THead
        local Accessory
        local Handle

        if TCharacter and TCharacter:FindFirstChildOfClass("Humanoid") then
            THumanoid = TCharacter:FindFirstChildOfClass("Humanoid")
        end
        if THumanoid and THumanoid.RootPart then
            TRootPart = THumanoid.RootPart
        end
        if TCharacter and TCharacter:FindFirstChild("Head") then
            THead = TCharacter.Head
        end
        if TCharacter and TCharacter:FindFirstChildOfClass("Accessory") then
            Accessory = TCharacter:FindFirstChildOfClass("Accessory")
        end
        if Accessory and Accessory:FindFirstChild("Handle") then
            Handle = Accessory.Handle
        end

        if Character and Humanoid and RootPart then
            if RootPart.Velocity.Magnitude < 50 then
                getgenv().OldPos = RootPart.CFrame
            end
            if THumanoid and THumanoid.Sit and not AllBool then
                return Message("Error Occurred", "Targeting is sitting", 5)
            end
            if THead then
                workspace.CurrentCamera.CameraSubject = THead
            elseif not THead and Handle then
                workspace.CurrentCamera.CameraSubject = Handle
            elseif THumanoid and TRootPart then
                workspace.CurrentCamera.CameraSubject = THumanoid
            end
            if not TCharacter:FindFirstChildWhichIsA("BasePart") then
                return
            end

            local function FPos(BasePart, Pos, Ang)
                RootPart.CFrame = CFrame.new(BasePart.Position) * Pos * Ang
                Character:SetPrimaryPartCFrame(CFrame.new(BasePart.Position) * Pos * Ang)
                RootPart.Velocity = Vector3.new(9e7, 9e7 * 10, 9e7)
                RootPart.RotVelocity = Vector3.new(9e8, 9e8, 9e8)
            end

            local function SFBasePart(BasePart)
                local TimeToWait = 2
                local Time = tick()
                local Angle = 0

                repeat
                    if RootPart and THumanoid then
                        if BasePart.Velocity.Magnitude < 50 then
                            Angle = Angle + 100

                            FPos(BasePart, CFrame.new(0, 1.5, 0) + THumanoid.MoveDirection * BasePart.Velocity.Magnitude / 1.25, CFrame.Angles(math.rad(Angle), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, 0) + THumanoid.MoveDirection * BasePart.Velocity.Magnitude / 1.25, CFrame.Angles(math.rad(Angle), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(2.25, 1.5, -2.25) + THumanoid.MoveDirection * BasePart.Velocity.Magnitude / 1.25, CFrame.Angles(math.rad(Angle), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(-2.25, -1.5, 2.25) + THumanoid.MoveDirection * BasePart.Velocity.Magnitude / 1.25, CFrame.Angles(math.rad(Angle), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, 1.5, 0) + THumanoid.MoveDirection, CFrame.Angles(math.rad(Angle), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, 0) + THumanoid.MoveDirection, CFrame.Angles(math.rad(Angle), 0, 0))
                            task.wait()
                        else
                            FPos(BasePart, CFrame.new(0, 1.5, THumanoid.WalkSpeed), CFrame.Angles(math.rad(90), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, -THumanoid.WalkSpeed), CFrame.Angles(0, 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, 1.5, THumanoid.WalkSpeed), CFrame.Angles(math.rad(90), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, 1.5, TRootPart.Velocity.Magnitude / 1.25), CFrame.Angles(math.rad(90), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, -TRootPart.Velocity.Magnitude / 1.25), CFrame.Angles(0, 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, 1.5, TRootPart.Velocity.Magnitude / 1.25), CFrame.Angles(math.rad(90), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, 0), CFrame.Angles(math.rad(90), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, 0), CFrame.Angles(0, 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, 0), CFrame.Angles(math.rad(-90), 0, 0))
                            task.wait()

                            FPos(BasePart, CFrame.new(0, -1.5, 0), CFrame.Angles(0, 0, 0))
                            task.wait()
                        end
                    else
                        break
                    end
                until BasePart.Velocity.Magnitude > 500 or BasePart.Parent ~= TargetPlayer.Character or TargetPlayer.Parent ~= Players or not TargetPlayer.Character == TCharacter or THumanoid.Sit or Humanoid.Health <= 0 or tick() > Time + TimeToWait
            end

            workspace.FallenPartsDestroyHeight = 0 / 0

            local BV = Instance.new("BodyVelocity")
            BV.Name = "EpixVel"
            BV.Parent = RootPart
            BV.Velocity = Vector3.new(9e8, 9e8, 9e8)
            BV.MaxForce = Vector3.new(1 / 0, 1 / 0, 1 / 0)

            Humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, false)

            if TRootPart and THead then
                if (TRootPart.CFrame.p - THead.CFrame.p).Magnitude > 5 then
                    SFBasePart(THead)
                else
                    SFBasePart(TRootPart)
                end
            elseif TRootPart and not THead then
                SFBasePart(TRootPart)
            elseif not TRootPart and THead then
                SFBasePart(THead)
            elseif not TRootPart and not THead and Accessory and Handle then
                SFBasePart(Handle)
            else
                return Message("Error Occurred", "Target is missing everything", 5)
            end

            BV:Destroy()
            Humanoid:SetStateEnabled(Enum.HumanoidStateType.Seated, true)
            workspace.CurrentCamera.CameraSubject = Humanoid

            repeat
                RootPart.CFrame = getgenv().OldPos * CFrame.new(0, .5, 0)
                Character:SetPrimaryPartCFrame(getgenv().OldPos * CFrame.new(0, .5, 0))
                Humanoid:ChangeState("GettingUp")
                table.foreach(Character:GetChildren(), function(_, x)
                    if x:IsA("BasePart") then
                        x.Velocity, x.RotVelocity = Vector3.new(), Vector3.new()
                    end
                end)
                task.wait()
            until (RootPart.Position - getgenv().OldPos.p).Magnitude < 25
            workspace.FallenPartsDestroyHeight = getgenv().FPDH
        else
            return Message("Error Occurred", "Random error", 5)
        end
    end

    -- Main logic to select player and fling
    local targetPlayer = GetPlayer(username)
    if targetPlayer and targetPlayer ~= Player then
        SkidFling(targetPlayer)
    else
        Message("Error Occurred", "Username Invalid or not found", 5)
    end
end
---------------------- ESP 기능 ------------------------
-- 서비스 및 변수 설정
local Players = game:GetService("Players")
local RunService = game:GetService("RunService")
local CoreGui = game:GetService("CoreGui")

local Player = Players.LocalPlayer
local espEnabled = false
local espTransparency = 0.3
local espObjects = {}

-- 캐릭터의 루트 파트를 가져오는 함수
local function getRoot(character)
    return character:FindFirstChild("HumanoidRootPart") or character:FindFirstChild("Head")
end

-- 숫자를 반올림하는 함수
local function round(num, numDecimalPlaces)
    local mult = 10^(numDecimalPlaces or 0)
    return math.floor(num * mult + 0.5) / mult
end

-- 플레이어에 대한 ESP를 생성하는 함수
local function createESP(plr)
    if plr == Player then return end -- 자기 자신에 대한 ESP는 생성하지 않음

    local function setupESP()
        local character = plr.Character
        if not character or not character:FindFirstChild("Humanoid") then return end

        local rootPart = getRoot(character)
        if not rootPart then return end

        local existingESP = espObjects[plr.UserId]
        if existingESP then
            existingESP:Destroy()
            espObjects[plr.UserId] = nil
        end

        local ESPholder = Instance.new("Folder")
        ESPholder.Name = plr.Name..'_ESP'
        ESPholder.Parent = CoreGui
        espObjects[plr.UserId] = ESPholder

        -- 캐릭터의 각 파트에 대해 ESP 박스 생성
        for _, part in pairs(character:GetChildren()) do
            if part:IsA("BasePart") then
                local adornment = Instance.new("BoxHandleAdornment")
                adornment.Name = plr.Name
                adornment.Parent = ESPholder
                adornment.Adornee = part
                adornment.AlwaysOnTop = true
                adornment.ZIndex = 10
                adornment.Size = part.Size
                adornment.Transparency = espTransparency
                adornment.Color3 = plr.TeamColor.Color
            end
        end

        -- 캐릭터의 머리 위에 텍스트 GUI 생성
        local head = character:FindFirstChild("Head")
        if head then
            local BillboardGui = Instance.new("BillboardGui")
            local TextLabel = Instance.new("TextLabel")
            BillboardGui.Adornee = head
            BillboardGui.Name = plr.Name
            BillboardGui.Parent = ESPholder
            BillboardGui.Size = UDim2.new(0, 100, 0, 150)
            BillboardGui.StudsOffset = Vector3.new(0, 2, 0)
            BillboardGui.AlwaysOnTop = true
            TextLabel.Parent = BillboardGui
            TextLabel.BackgroundTransparency = 1
            TextLabel.Position = UDim2.new(0, 0, 0, -50)
            TextLabel.Size = UDim2.new(0, 100, 0, 100)
            TextLabel.Font = Enum.Font.SourceSansSemibold
            TextLabel.TextSize = 20
            TextLabel.TextColor3 = Color3.new(1, 1, 1)
            TextLabel.TextStrokeTransparency = 0
            TextLabel.TextYAlignment = Enum.TextYAlignment.Bottom
            TextLabel.Text = 'Name: '..plr.Name

            -- ESP 업데이트 함수
            local function updateESP()
                if not ESPholder.Parent then return end
                if character and rootPart and character:FindFirstChildOfClass("Humanoid") then
                    local playerRootPart = getRoot(Player.Character)
                    if playerRootPart and rootPart then
                        local distance = round((rootPart.Position - playerRootPart.Position).Magnitude, 1)
                        local health = round(character:FindFirstChildOfClass('Humanoid').Health, 1)
                        TextLabel.Text = 'Name: '..plr.Name..' | Health: '..health..' | Distance: '..distance
                    end
                end
            end

            -- RenderStepped에서 ESP 업데이트 루프 실행
            local espLoopFunc = RunService.RenderStepped:Connect(updateESP)

            -- 캐릭터가 제거될 때 ESP 및 업데이트 루프 제거
            local function onCharacterRemoving()
                espLoopFunc:Disconnect()
                if ESPholder.Parent then
                    ESPholder:Destroy()
                end
            end

            -- 새 캐릭터가 추가될 때 ESP 재설정
            local function onCharacterAdded(newCharacter)
                setupESP() -- 새로운 캐릭터가 추가되면 ESP 재설정
            end

            -- 이벤트 연결
            plr.CharacterRemoving:Connect(onCharacterRemoving)
            plr.CharacterAdded:Connect(onCharacterAdded)

            -- 팀 색상 변경 시 ESP 색상 업데이트
            local function onTeamChanged()
                for _, adornment in pairs(ESPholder:GetChildren()) do
                    if adornment:IsA("BoxHandleAdornment") then
                        adornment.Color3 = plr.TeamColor.Color
                    end
                end
            end
            plr:GetPropertyChangedSignal("TeamColor"):Connect(onTeamChanged)
        end
    end

    -- 캐릭터가 존재할 경우 ESP 설정
    if plr.Character then
        setupESP()
    end

    -- 새로 추가된 캐릭터에 대해서도 ESP 설정
    plr.CharacterAdded:Connect(setupESP)
end

-- 플레이어가 떠날 때 ESP 제거
local function removeESPForPlayer(plr)
    local esp = espObjects[plr.UserId]
    if esp then
        esp:Destroy()
        espObjects[plr.UserId] = nil
    end
end

-- 모든 플레이어에 대한 ESP 설정
local function applyESP()
    for _, plr in pairs(Players:GetPlayers()) do
        createESP(plr)
    end
end

-- 모든 플레이어에 대한 ESP 제거
local function removeESP()
    for _, esp in pairs(espObjects) do
        if esp then
            esp:Destroy()
        end
    end
    espObjects = {}
end

-- 자신의 캐릭터가 추가될 때 모든 플레이어에게 ESP 적용
Player.CharacterAdded:Connect(function()
    if espEnabled then
        applyESP()
    end
end)

-- 새로 추가된 플레이어에게 ESP 적용
Players.PlayerAdded:Connect(function(plr)
    if espEnabled then
        createESP(plr)
    end
end)

-- 플레이어가 떠날 때 ESP 제거
Players.PlayerRemoving:Connect(function(plr)
    removeESPForPlayer(plr)
end)

-- 플레이어의 캐릭터가 사망하거나 재생성될 때 ESP 업데이트
Players.PlayerAdded:Connect(function(plr)
    plr.CharacterAdded:Connect(function(character)
        if espEnabled then
            createESP(plr)
        end
    end)
end)

-- 게임 시작 시 이미 존재하는 플레이어들에게 ESP 적용
if espEnabled then
    applyESP()
end
---------------------- ESP 기능 --------------------------
---------------------- FLY 기능 --------------------------
local function startFlying()
    if isFlying then return end
    isFlying = true
    local T = Player.Character.HumanoidRootPart
    local CONTROL = {F = 0, B = 0, L = 0, R = 0, Q = 0, E = 0}
    local lCONTROL = {F = 0, B = 0, L = 0, R = 0, Q = 0, E = 0}
    local SPEED = 0

    local function FLY()
        local BG = Instance.new('BodyGyro')
        local BV = Instance.new('BodyVelocity')
        BG.P = 9e4
        BG.Parent = T
        BV.Parent = T
        BG.maxTorque = Vector3.new(9e9, 9e9, 9e9)
        BG.cframe = T.CFrame
        BV.velocity = Vector3.new(0, 0, 0)
        BV.maxForce = Vector3.new(9e9, 9e9, 9e9)

        task.spawn(function()
            repeat wait()
                if CONTROL.L + CONTROL.R ~= 0 or CONTROL.F + CONTROL.B ~= 0 or CONTROL.Q + CONTROL.E ~= 0 then
                    SPEED = flySpeed * 40
                elseif SPEED ~= 0 then
                    SPEED = 0
                end
                if (CONTROL.L + CONTROL.R) ~= 0 or (CONTROL.F + CONTROL.B) ~= 0 or (CONTROL.Q + CONTROL.E) ~= 0 then
                    BV.velocity = ((workspace.CurrentCamera.CoordinateFrame.lookVector * (CONTROL.F + CONTROL.B)) + ((workspace.CurrentCamera.CoordinateFrame * CFrame.new(CONTROL.L + CONTROL.R, (CONTROL.F + CONTROL.B + CONTROL.Q + CONTROL.E) * 0.2, 0).p) - workspace.CurrentCamera.CoordinateFrame.p)) * SPEED
                    lCONTROL = {F = CONTROL.F, B = CONTROL.B, L = CONTROL.L, R = CONTROL.R}
                elseif (CONTROL.L + CONTROL.R) == 0 and (CONTROL.F + CONTROL.B) == 0 and (CONTROL.Q + CONTROL.E) == 0 and SPEED ~= 0 then
                    BV.velocity = ((workspace.CurrentCamera.CoordinateFrame.lookVector * (lCONTROL.F + lCONTROL.B)) + ((workspace.CurrentCamera.CoordinateFrame * CFrame.new(lCONTROL.L + lCONTROL.R, (lCONTROL.F + lCONTROL.B + CONTROL.Q + CONTROL.E) * 0.2, 0).p) - workspace.CurrentCamera.CoordinateFrame.p)) * SPEED
                else
                    BV.velocity = Vector3.new(0, 0, 0)
                end
                BG.cframe = workspace.CurrentCamera.CoordinateFrame
            until not isFlying
            CONTROL = {F = 0, B = 0, L = 0, R = 0, Q = 0, E = 0}
            lCONTROL = {F = 0, B = 0, L = 0, R = 0, Q = 0, E = 0}
            SPEED = 0
            BG:Destroy()
            BV:Destroy()
            if Player.Character:FindFirstChildOfClass('Humanoid') then
                Player.Character:FindFirstChildOfClass('Humanoid').PlatformStand = false
            end
        end)
    end

    local function onKeyPress(KEY)
        if KEY:lower() == 'w' then
            CONTROL.F = 1
        elseif KEY:lower() == 's' then
            CONTROL.B = -1
        elseif KEY:lower() == 'a' then
            CONTROL.L = -1
        elseif KEY:lower() == 'd' then
            CONTROL.R = 1
        elseif KEY:lower() == 'e' then
            CONTROL.Q = 1
        elseif KEY:lower() == 'q' then
            CONTROL.E = -1
        end
    end

    local function onKeyRelease(KEY)
        if KEY:lower() == 'w' then
            CONTROL.F = 0
        elseif KEY:lower() == 's' then
            CONTROL.B = 0
        elseif KEY:lower() == 'a' then
            CONTROL.L = 0
        elseif KEY:lower() == 'd' then
            CONTROL.R = 0
        elseif KEY:lower() == 'e' then
            CONTROL.Q = 0
        elseif KEY:lower() == 'q' then
            CONTROL.E = 0
        end
    end

    UserInputService.InputBegan:Connect(function(input, processed)
        if not processed and input.UserInputType == Enum.UserInputType.Keyboard then
            onKeyPress(input.KeyCode.Name)
        end
    end)

    UserInputService.InputEnded:Connect(function(input)
        if input.UserInputType == Enum.UserInputType.Keyboard then
            onKeyRelease(input.KeyCode.Name)
        end
    end)

    FLY()
end

local function stopFlying()
    if not isFlying then return end
    isFlying = false
    if Player.Character:FindFirstChildOfClass('Humanoid') then
        Player.Character:FindFirstChildOfClass('Humanoid').PlatformStand = false
    end
end

local function startSpinning()
    local character = Player.Character or Player.CharacterAdded:Wait()
    local humanoidRootPart = character:WaitForChild("HumanoidRootPart")
    
    for _, v in pairs(humanoidRootPart:GetChildren()) do
        if v:IsA("BodyAngularVelocity") then
            v:Destroy()
        end
    end
    
    local bodyAngularVelocity = Instance.new("BodyAngularVelocity")
    bodyAngularVelocity.Name = "Spinning"
    bodyAngularVelocity.Parent = humanoidRootPart
    bodyAngularVelocity.MaxTorque = Vector3.new(0, math.huge, 0)
    
    local angularVelocityMagnitude = math.rad(spinSpeed) * 300
    bodyAngularVelocity.AngularVelocity = Vector3.new(0, angularVelocityMagnitude, 0)
end

local function stopSpinning()
    local character = Player.Character
    if not character then return end
    
    for _, v in pairs(character.HumanoidRootPart:GetChildren()) do
        if v:IsA("BodyAngularVelocity") then
            v:Destroy()
        end
    end
end
---------------------- FLY 기능 ------------------------------
---------------------- Noclip 기능 ------------------------------
local function NoclipLoop()
    if noclip and Player.Character then
        for _, child in pairs(Player.Character:GetDescendants()) do
            if child:IsA("BasePart") and child.CanCollide then
                child.CanCollide = false
            end
        end
    end
end

local function setNoclip(value)
    noclip = value
    if noclip then
        while noclip do
            NoclipLoop()
            task.wait(0.1)
        end
    else
        for _, child in pairs(Player.Character:GetDescendants()) do
            if child:IsA("BasePart") then
                child.CanCollide = true
            end
        end
    end
end
---------------------- 부대게임, 샤크부대 ------------------------------
-- 메시지를 띄우기 위한 함수
local function showMessage(mass)
    Rayfield:Notify({
        Title = "알림",  -- 알림 창의 제목
        Content = tostring(mass) .. "이(가) 실행 되었습니다!",  -- 알림 창에 표시될 내용
        Duration = 5,  -- 알림이 표시될 시간(초 단위)
    })
end
---------------------- 기본기능 모음 탭 1 ------------------------
Rayfield:Notify({
    Title = "🐳 Whale Hub🐳가 정상적으로 실행됨.",
    Content = "감사합니다 :)🐳🐳",
    Duration = 6.5,
    Image = 4483362458,
})

local Tab2 = Window:CreateTab("Standerd Cheat2") -- 'Tab' 변수에 새 탭을 생성

-- 섹션 1: Cheat
local Section1_1 = Tab2:CreateSection("Cheat2")
local Toggle_Fly = Tab2:CreateToggle({
    Name = "Fly",
    CurrentValue = false,
    Flag = "Toggle",
    Callback = function(Value)
        if Value then
             showMessage("Fly :" .. tostring(Value))
             startFlying()-- 비행 활성화 코드
        else
             showMessage("Fly :" .. tostring(Value))
             stopFlying()-- 비행 비활성화 코드
        end
    end,
 })
 
 local Toggle_Noclip = Tab2:CreateToggle({
    Name = "Noclip",
    CurrentValue = false,
    Flag = "Toggle",
    Callback = function(Value)
         showMessage("Noclip : " .. tostring(Value))
         setNoclip(Value)
    end,
 })
 
 local Toggle_Spin = Tab2:CreateToggle({
    Name = "Spin",
    CurrentValue = false,
    Flag = "Toggle",
    Callback = function(Value)
         if Value then
             showMessage("Spin : " .. tostring(Value))
             startSpinning()
         else
             stopSpinning()
         end
    end,
 })
 
 local Toggle_ESP = Tab2:CreateToggle({
    Name = "ESP",
    CurrentValue = false,
    Flag = "Toggle",
    Callback = function(Value)
        if Value then
             showMessage("ESP : " .. tostring(Value))
             espEnabled = true
             while espEnabled do
                 applyESP()-- ESP 활성화 코드
                 wait(1)
             end
        else
             showMessage("ESP : " .. tostring(Value))
             espEnabled = false
             removeESP()-- ESP 비활성화 코드
        end
    end,
 })
 
 local Button_Kick_All = Tab2:CreateButton({
     Name = "모든 플레이어 날리기 (플레이어 끼리 통과되면 안 됨)",
     Callback = function()
         showMessage("Fling")
         loadstring(game:HttpGet("https://pastebin.com/raw/zqyDSUWX"))()
     end
 })
 
 local Button_Kick_Selcet = Tab2:CreateButton({
     Name = "특정 플레이어 날리기 (플레이어 끼리 통과되면 안 됨)",
     Callback = function()
         Select_Fling_Player(FlingUser_Name)
     end
 })
 
 local Input_Fling_User = Tab2:CreateInput({
     Name = "특정 플레이어 이름",
     RemoveTextAfterFocusLost = false,
     PlaceholderText = "Enter UserName here",
     Callback = function(text)
         FlingUser_Name = findPlayerDisplayName(text) -- 입력받는 텍스트 사용
     end
 })

 local Input_FlySpeed = Tab2:CreateSlider({
     Name = "Fly Speed",
     Range = {0, 20},
     Increment = 1,
     Suffix = "%",
     CurrentValue = 5,
     Callback = function(value)
         showMessage("Speed : ".. tostring(value))
         flySpeed = value-- Fly speed 변경 코드
     end
 })
 local KillA123123l123lButton = Tab2:CreateButton({
    Name = "인피니티 야드",
    Callback = function()
        loadstring(game:HttpGet("https://cdn.wearedevs.net/scripts/Infinite%20Yield.txt"))()
        game.StarterGui:SetCore("SendNotification", {
            Title = "Whale Hub",
            Text = "인피니티 야드를 킴.",
            Duration = 5
        })
    end,
 })
 local KillA12312dadad3l123lButton = Tab2:CreateButton({
    Name = "아이템 익스플로잇 ",
    Callback = function()
        loadstring(game:HttpGet("https://raw.githubusercontent.com/yofriendfromschool1/Sky-Hub-Backup/main/gametoolgiver.lua"))()
        game.StarterGui:SetCore("SendNotification", {
            Title = "Whale Hub",
            Text = "아이템 익스플로잇을 성공적으로 킴. ",
            Duration = 5
        })
    end,
 })
print("Whale Hub")
 local Input_SpinSpeed = Tab2:CreateSlider({
     Name = "Spin Speed",
     Range = {0, 50},
     Increment = 1,
     Suffix = "%",
     CurrentValue = 5,
     Callback = function(value)
         -- Spin speed 변경 코드
         spinSpeed = value
     end
 })
 local Section1_3 = Tab2:CreateSection("Tools")
 

 local Toggle_Transparency = Tab2:CreateButton({
     Name = "Transparency (투명 H)",
     Callback = function()
         Transparency_toggle_bt(true)
     end
 })
 local FlingUser_Name = nil
local Tab = Window:CreateTab("Standerd Cheat") -- 'Tab' 변수에 새 탭을 생성

-- 섹션 1: Cheat
local Section1_1 = Tab:CreateSection("Cheat")
-- 자기 자신을 킥하는 버튼 추가
local kickButton = Tab:CreateButton({
    Name = "🐳자기 자신 킥🐳",
    Callback = function()
        local player = game.Players.LocalPlayer
        -- 자기 자신을 킥
        player:Kick("🐳")  -- 퇴장 메시지도 설정할 수 있습니다.
    end,
})
local kick197233Button = Tab:CreateButton({
    Name = "🐳맵 복사🐳",
    Callback = function()
        saveinstance()
    end,
})
-- CE 올킬 버튼
local kick3Button = Tab:CreateButton({
    Name = "🐳CE 올킬🐳",
    Callback = function()
        local ReplicatedStorage = game:GetService("ReplicatedStorage")
        local Players = game:GetService("Players")

        -- Getting the DamageEvent from ACS_Engine
        local ACS_Engine = ReplicatedStorage:WaitForChild("ACS_Engine", 3)
        local DamageEvent = ACS_Engine:WaitForChild("Events"):WaitForChild("Damage")

        -- Iterate over all players and apply damage to them
        for _, plr in pairs(Players:GetPlayers()) do
            if plr ~= Players.LocalPlayer then
                local humanoid = plr.Character and plr.Character:FindFirstChild("Humanoid")
                if humanoid then
                    -- Fire the DamageEvent with proper parameters
                    DamageEvent:FireServer(humanoid, 100000, "Torso", {'nil','Auth','nil','nil'})
                end
            end
        end
    end,
})

-- ACS 블럭 설치 버튼
local kick2Button = Tab:CreateButton({
    Name = "ACS 블럭 설치🐳",
    Callback = function()
        local ReplicatedStorage = game:GetService("ReplicatedStorage")
        local Event = ReplicatedStorage:WaitForChild("ACS_Engine", 3).Events.Breach
        local Players = game:GetService("Players")
        Event:InvokeServer(3, {Fortified = {}, Destroyable = workspace}, CFrame.new(), CFrame.new(), {
            CFrame = Players.LocalPlayer.Character:GetPivot().Position,
            Size = Vector3.new(10, 10, 10)
        })
    end,
})

-- 텔레포트 상태를 제어할 변수
local stopTeleport = false

-- 플레이어 텔레포트 입력 필드
local TPInput = Tab:CreateInput({
    Name = "플레이어 이름 입력(계속tp)🐳",
    PlaceholderText = "플레이어 이름을 입력하세요🐳",  
    RemoveTextAfterFocusLost = true,
    Callback = function(Text)
        local player = game.Players.LocalPlayer
        local targetPlayerName = Text

        -- 텔레포트할 플레이어 찾기
        local targetPlayer = game.Players:FindFirstChild(targetPlayerName)

        -- 텔레포트 실행
        if targetPlayer then
            -- 플레이어가 존재하면 계속해서 텔레포트 되도록
            stopTeleport = false
            spawn(function()
                while not stopTeleport do
                    if targetPlayer.Character and player.Character then
                        -- 나를 원하는 플레이어에게 계속 텔레포트
                        player.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
                    end
                    wait(1)
                end
            end)
            
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 시작🐳🐳🐳",
                Text = "지정된 플레이어에게 계속 텔레포트 됩니다.🐳🐳",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 실패🐳",
                Text = "플레이어를 찾을 수 없습니다.🐳",
                Duration = 5
            })
        end
    end,
})

-- 텔레포트를 멈추는 버튼 추가
local stopButton = Tab:CreateButton({
    Name = "텔레포트 멈추기🐳",
    Callback = function()
        stopTeleport = true
        game.StarterGui:SetCore("SendNotification", {
            Title = "텔레포트 종료🐳",
            Text = "텔레포트가 멈췄습니다.🐳",
            Duration = 5
        })
    end,
})

-- 무한 점프 버튼
local Button = Tab:CreateButton({
    Name = "무한 점프(실행 할려면 눌르세요)🐳",
    Callback = function()
        -- Toggles the infinite jump between on or off on every script run
        _G.infinjump = not _G.infinjump

        if _G.infinJumpStarted == nil then
            -- Ensures this only runs once to save resources
            _G.infinJumpStarted = true
            
            -- Notifies readiness
            game.StarterGui:SetCore("SendNotification", {Title="무한 점프🐳", Text="무한 점프가 가동됨.🐳", Duration=5})
            local plr = game:GetService('Players').LocalPlayer
            local m = plr:GetMouse()
            m.KeyDown:connect(function(k)
                if _G.infinjump then
                    if k:byte() == 32 then
                        local humanoid = game:GetService('Players').LocalPlayer.Character:FindFirstChildOfClass('Humanoid')
                        humanoid:ChangeState('Jumping')
                        wait()
                        humanoid:ChangeState('Seated')
                    end
                end
            end)
        end
    end,
})
local Slider = Tab:CreateSlider({
   Name = "스피드 핵🐳",
   Range = {1, 5000},
   Increment = 1,
   Suffix = "Speed",
   CurrentValue = 16,
   Flag = "sliderws", 
   Callback = function(Value)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Value)
   end,
})
local Input = MainTab:CreateInput({
   Name = "Walkspeed",
   PlaceholderText = "숫자",
   RemoveTextAfterFocusLost = true,
   Callback = function(Text)
        game.Players.LocalPlayer.Character.Humanoid.WalkSpeed = (Text)
   end,
})
local TPInput = MainTab:CreateInput({
    Name = "플레이어 이름 입력",
    PlaceholderText = "플레이어 이름을 입력하세요",  -- 플레이어 이름을 입력할 필드
    RemoveTextAfterFocusLost = true,
    Callback = function(Text)
        local player = game.Players.LocalPlayer
        local targetPlayerName = Text  -- 텍스트로 입력된 플레이어 이름

        -- 텔레포트할 플레이어 찾기
        local targetPlayer = game.Players:FindFirstChild(targetPlayerName)
        if targetPlayer and targetPlayer.Character then
            -- 텔레포트 실행
            player.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 성공",
                Text = "플레이어 " .. targetPlayerName .. " 에게 텔레포트 되었습니다.",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 실패",
                Text = "플레이어를 찾을 수 없습니다.",
                Duration = 5
            })
        end
    end,
})

local KillAllButton = Tab:CreateButton({
    Name = "재설정",
    Callback = function()
        for _, player in ipairs(game.Players:GetPlayers()) do
            -- 캐릭터가 있을 경우
            if player.Character and player.Character:FindFirstChild("Humanoid") then
                local humanoid = player.Character:FindFirstChild("Humanoid")
                humanoid.Health = 0  -- 플레이어의 체력을 0으로 설정하여 죽임
            end
        end
        
        -- 알림 표시
        game.StarterGui:SetCore("SendNotification", {
            Title = "재설정 신호",
            Text = "재설정 완료!",
            Duration = 5
        })
    end,
 })
-- 플레이어 텔레포트 입력 필드
local TPInput = Tab:CreateInput({
    Name = "플레이어 이름 입력",
    PlaceholderText = "플레이어 이름을 입력하세요",  -- 플레이어 이름을 입력할 필드
    RemoveTextAfterFocusLost = true,
    Callback = function(Text)
        local player = game.Players.LocalPlayer
        local targetPlayerName = Text  -- 텍스트로 입력된 플레이어 이름

        -- 텔레포트할 플레이어 찾기
        local targetPlayer = game.Players:FindFirstChild(targetPlayerName)
        if targetPlayer and targetPlayer.Character then
            -- 텔레포트 실행
            player.Character.HumanoidRootPart.CFrame = targetPlayer.Character.HumanoidRootPart.CFrame
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 성공",
                Text = "플레이어 " .. targetPlayerName .. " 에게 텔레포트 되었습니다.",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "텔레포트 실패",
                Text = "플레이어를 찾을 수 없습니다.",
                Duration = 5
            })
        end
    end,
})

-- 위치 입력을 위한 텍스트 박스
local LocationInput = Tab:CreateInput({
    Name = "텔레포트 위치 입력",
    PlaceholderText = "X, Y, Z 좌표 입력 (예: 0, 10, 0)",
    RemoveTextAfterFocusLost = true,
    Callback = function(Text)
        -- 텍스트에서 X, Y, Z 좌표를 파싱
        local x, y, z = Text:match("([%-?%d%.]+),%s*([%-?%d%.]+),%s*([%-?%d%.]+)")
        
        -- 입력된 좌표가 올바른지 확인
        if x and y and z then
            x, y, z = tonumber(x), tonumber(y), tonumber(z) -- 문자열을 숫자로 변환

            local player = game.Players.LocalPlayer
            -- 텔레포트
            player.Character.HumanoidRootPart.CFrame = CFrame.new(x, y, z)
            game.StarterGui:SetCore("SendNotification", {
                Title = "위치 텔레포트 성공",
                Text = "지정된 위치로 텔레포트 되었습니다.",
                Duration = 5
            })
        else
            game.StarterGui:SetCore("SendNotification", {
                Title = "잘못된 좌표 형식",
                Text = "좌표를 올바른 형식으로 입력해주세요. (예: 0, 10, 0)",
                Duration = 5
            })
        end
    end,
})
