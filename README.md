
-----------------------------------

local Setting = {
	["Auto Farm"] = true,
	["Fast Attack"] = false, -- auto trun if you will truen on AutoFarm
	["Far Attack"] = false, -- auto trun if you will truen on AutoFarm or FastAttack
	---- Stats ----
	["Auto Melee"] = true,
}

------------------------------------




function SettingS()
	local AutoFarmMini = Setting["Auto Farm"]
	_G.Farm = AutoFarmMini
	local AutoFar = Setting["Far Attack"]
	_G.FarAttack = AutoFar
		local AutoClickMini = Setting["Fast Attack"]
	_G.FastAttack = AutoClickMini
	local AutoMelee = Setting["Auto Melee"]
	_G.Melee = AutoMelee
end



SettingS()
local VirtualInputManager = game:GetService("VirtualInputManager")
local RunService = game:GetService("RunService")

function CheckQuest()
    local Lv =  game.Players.LocalPlayer.Data.Level.Value
    if game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == true then
elseif  Lv == 1 or Lv <= 9 then
        Ms = "Bandit [Lv. 5]"
        CQ = CFrame.new(1059.838623046875, 16.516624450683594, 1545.941162109375)
        CM = CFrame.new(1177.2108154296875, 16.43285369873047, 1616.587646484375)
        NQ = "BanditQuest1"
        NM = "Bandit"
        LQ = 1
    elseif Lv == 10 or Lv <= 14 then
        Ms = "Monkey [Lv. 14]"
        NM = "Monkey"
        LQ = 1
        NQ = "JungleQuest"
        CM = CFrame.new(-1443.7662353515625, 61.851966857910156, -47.555946350097656)
        CQ = CFrame.new(-1599.8194580078125, 36.852149963378906, 153.0706024169922)
        elseif Lv == 15 or Lv <= 29999 then
        Ms = "Gorilla [Lv. 20]"
        NM = "Gorilla"
        LQ = 2
        NQ = "JungleQuest"
        CM = CFrame.new(-1257.35938, 6.31437159, -508.771057, -0.920952022, 1.1847475e-08, -0.389675975, -2.06710471e-08, 1, 7.9256921e-08, 0.389675975, 8.1046835e-08, -0.920952022)
        CQ = CFrame.new(-1599.8194580078125, 36.852149963378906, 153.0706024169922) 
     end
 end

function TP(P)
     NoClip = true
     Distance = (P.Position - game.Players.LocalPlayer.Character.HumanoidRootPart.Position).Magnitude
     if Distance < 10 then
         Speed = 1000
     elseif Distance < 170 then
         game.Players.LocalPlayer.Character.HumanoidRootPart.CFrame = P
         Speed = 350
     elseif Distance < 1000 then
         Speed = 350
     elseif Distance >= 1000 then
         Speed = 280
     end
     game:GetService("TweenService"):Create(
         game.Players.LocalPlayer.Character.HumanoidRootPart,
         TweenInfo.new(Distance/Speed, Enum.EasingStyle.Linear),
         {CFrame = P}
     ):Play()
     NoClip = false
 end

 local function StayPart()
	 local part = Instance.new("Part")
	 if not workspace:FindFirstChild("StayPart") then 
	 CheckQuest()
	 part.Name = "StayPart"
	 part.Size = Vector3.new(10, 1.2, 10)
	 part.Transparency = 0.5
	 part.Parent = workspace
	 part.Anchored = true
	 part.CanCollide = true
	 
	 part.CFrame = CM * CFrame.new(0,10,0)
	 else
	workspace:FindFirstChild("StayPart"):Destroy()
	 end
 end
 


 spawn(function()
     while wait() do
         if _G.Farm then
     local stayp = "StayPart"
	 stayp.Size = Vector3.new(10, 1.2, 10)
	 stayp.Transparency = 0.5
	 stayp.Parent = workspace
	 stayp.Anchored = true
	 stayp.CanCollide = true
		 
             CheckQuest()
             if game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == false then
			    game:GetService("Players").LocalPlayer.PlayerGui.Main.Dialogue.Visible = false
                 TP(CQ)
                 wait(0.5)
                 game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("StartQuest",NQ,LQ)
             elseif game:GetService("Players").LocalPlayer.PlayerGui.Main.Quest.Visible == true then
			    game:GetService("Players").LocalPlayer.PlayerGui.Main.Dialogue.Visible = false
                
				
				_G.BringMob = true
				_G.FastAttack = true
				stayp.CFrame = CM * CFrame.new(0,10,0)
				TP(stayp*CFrame.new(0,1,0))
				end
			else
			 _G.BringMob = false
			   _G.FastAttack = false
             end 
         end
     end)





spawn(function()
     while wait() do
         if _G.FastAttack then
_G.FarAttack = true
VirtualInputManager:SendMouseButtonEvent(0,0,0, true, game, 1)
wait(0.1)
else
	_G.FarAttack = false
end
end
end)

spawn(function()
     while wait() do
         if _G.FarAttack then
		 local bigHand = game.Players.LocalPlayer.Character.RightHand
		 bigHand.Size = Vector3.new(20, 20, 20)
		 bigHand.Transparency = 1
		 else
		local smallhand = game.Players.LocalPlayer.Character.RightHand
		 smallhand.Size = Vector3.new( 0.7084817886352539, 1.0482875108718872, 0.8396478891372681)
		 smallhand.Transparency = 0
		 end
	end
 end)

	 spawn(function()
     pcall(function()
         while task.wait() do
             if _G.Farm then
                 if not game.Players.LocalPlayer.Character.HumanoidRootPart:FindFirstChild("BodyClip") then
                     local Noclip = Instance.new("BodyVelocity")
                     Noclip.Name = "BodyClip"
                     Noclip.Parent = game.Players.LocalPlayer.Character.HumanoidRootPart
                     Noclip.MaxForce = Vector3.new(100000,100000,100000)
                     Noclip.Velocity = Vector3.new(0,0,0)
                 end
             else
                 if game.Players.LocalPlayer.Character.HumanoidRootPart:FindFirstChild("BodyClip") then
                     game.Players.LocalPlayer.Character.HumanoidRootPart:FindFirstChild("BodyClip"):Destroy()
                 end
             end
         end
     end)
 end)
 
 
 

 spawn(function()
    while wait() do
        if _G.BringMob then
            pcall(function()
            CheckQuest()
       for i,v in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
for x,y in pairs(game:GetService("Workspace").Enemies:GetChildren()) do
if v.Name == NM then
    if y.Name == NM then
   v.HumanoidRootPart.CFrame = CM
   v.HumanoidRootPart.Size = Vector3.new(30,30,30)
   y.HumanoidRootPart.Size = Vector3.new(30,30,30)
   v.HumanoidRootPart.Transparency = 0.9
   v.HumanoidRootPart.CanCollide = false
   y.HumanoidRootPart.CanCollide = false
   v.Humanoid.WalkSpeed = 0
   y.Humanoid.WalkSpeed = 0
   v.Humanoid.JumpPower = 0
   y.Humanoid.JumpPower = 0


   if sethiddenproperty then
     sethiddenproperty(game.Players.LocalPlayer, "SimulationRadius", math.huge)
end
end
end
end
end
end)
end
end
end)

local DataPoint = 1


spawn(function()
         pcall(function()
         while task.wait() do
             if _G.Melee then
 game:GetService("ReplicatedStorage").Remotes.CommF_:InvokeServer("AddPoint","Melee",DataPoint)
 end
 end
 end)
 end)

local Fluent = loadstring(game:HttpGet("https://github.com/dawid-scripts/Fluent/releases/latest/download/main.lua"))()
local SaveManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/SaveManager.lua"))()
local InterfaceManager = loadstring(game:HttpGet("https://raw.githubusercontent.com/dawid-scripts/Fluent/master/Addons/InterfaceManager.lua"))()

local Window = Fluent:CreateWindow({
    Title = "Aegona Hub " .. Fluent.Version,
    SubTitle = "by Arter",
    TabWidth = 160,
    Size = UDim2.fromOffset(580, 460),
    Acrylic = true, -- The blur may be detectable, setting this to false disables blur entirely
    Theme = "Dark",
    MinimizeKey = Enum.KeyCode.LeftControl -- Used when theres no MinimizeKeybind
})

local Tabs = {
    Main = Window:AddTab({ Title = "Main", Icon = "" }),
	Stats = Window:AddTab({ Title = "Stats", Icon = "" }),
    Settings = Window:AddTab({ Title = "Settings", Icon = "settings" })
}

local Options = Fluent.Options

do
    Fluent:Notify({
        Title = "Notification",
        Content = "This is a notification",
        SubContent = "SubContent", -- Optional
        Duration = 5 -- Set to nil to make the notification not disappear
    })


	local Toggle = Tabs.Main:AddToggle("MyToggle", {Title = "Auto Farm ", Default = _G.Farm })
	local Toggle2 = Tabs.Stats:AddToggle("MyToggle2", {Title = "Auto Melee", Default = _G.Melee })


    Toggle:OnChanged(function()
        _G.Farm = Options.MyToggle.Value
    end)

    Toggle2:OnChanged(function()
        _G.Melee = Options.MyToggle2.Value
    end)

    Options.MyToggle:SetValue(_G.Farm)
    Options.MyToggle2:SetValue(_G.Melee)

	 local Slider = Tabs.Main:AddSlider("Slider", {
        Title = "Point",
        Description = "This is a slider",
        Default = 1,
        Min = 0,
        Max = 100,
        Rounding = 1,
        Callback = function(Value)
            print("Slider was changed:", Value)
        end
    })

    Slider:OnChanged(function(Value)
        DataPoint = Value
    end)

    Slider:SetValue(1)

	end

