local Obj = Instance
local string = {}
local temp = {}
local error = false

while Obj ~= game do
if Obj == nil then
error = true
break
end
table.insert(temp, Obj.Parent == game and Obj.ClassName or tostring(Obj))
Obj = Obj.Parent
end

table.insert(string, "game:GetService(\"" .. temp[#temp] .. "\")")

for i = #temp - 1, 1, -1 do
table.insert(string, HasSpecial(temp[i]) and "[\"" .. temp[i] .. "\"]" or "." .. temp[i])
end

return (error and "nil -- Path contained invalid instance" or table.concat(string, ""))
end

local GetType = function(Instance)
local Types = 
{
EnumItem = function()
return "Enum." .. tostring(Instance.EnumType) .. "." .. tostring(Instance.Name)
end,
Instance = function()
return GetPath(Instance)
end,
CFrame = function()
return "CFrame.new(" .. tostring(Instance) .. ")"
end,
Vector3 = function()
return "Vector3.new(" .. tostring(Instance) .. ")"
end,
BrickColor = function()
return "BrickColor.new(\"" .. tostring(Instance) .. "\")"
end,
Color3 = function()
return "Color3.new(" .. tostring(Instance) .. ")"
end,
string = function()
local S = tostring(Instance)
return "\"" .. (encrypt_string and S:gsub(".", function(c) return "\\" .. c:byte() end) or S) .. "\""
end,
Ray = function()
return "Ray.new(Vector3.new(" .. tostring(Instance.Origin) .. "), Vector3.new(" .. tostring(Instance.Direction) .. "))"
end
}

return Types[typeof(Instance)] ~= nil and Types[typeof(Instance)]() or tostring(Instance)
end

local size_frame = function(frame, UDim)
frame:TweenSize(UDim, "Out", "Quint", 0.3)
end

local pos_frame = function(frame, UDim)
frame:TweenPosition(UDim, "Out", "Quint", 0.3)
end

local size_pos_frame = function(frame, UDim, UDim2)
frame:TweenSizeAndPosition(UDim, UDim2, "Out", "Quint", 0.3)
end

local hide = function()
size_frame(BG, UDim2.new(0, 300, 0, 20))
pos_frame(Title, UDim2.new(0, 0, 0, 0))
pos_frame(Remotes, UDim2.new(0, 10, 0, 100))
pos_frame(Source, UDim2.new(0, 270, 0, 100))
BG.Draggable = true
SetRemotes.Visible = false
SetRemotesTab.Visible = false
Source.Visible = true

return "[]"
end

local show = function()
size_frame(BG, UDim2.new(1, -300, 1, -200))
pos_frame(BG, UDim2.new(0.1, 0, 0.1, 0))
pos_frame(Title, UDim2.new(0.5, -100, 0, 0))
pos_frame(Remotes, UDim2.new(0, 10, 0, 80))
pos_frame(Source, UDim2.new(0, 270, 0, 80))
BG.Draggable = false

return "_"
end

local onclick_hide = function()
Hide.Text = Hide.Text == "_" and hide() or show()
end

local onclick_settings = function()
Source.Visible = not Source.Visible
SetRemotes.Visible = not Source.Visible
SetRemotesTab.Visible = not Source.Visible
end

local onclick_remotespy = function()
spy_enabled = not spy_enabled
EnableSpy.TextColor3 = EnableSpy.TextColor3 == Color3.fromRGB(60, 200, 60) and Color3.fromRGB(200, 60, 60) or Color3.fromRGB(60, 200, 60)
EnableSpy.BorderColor3 = EnableSpy.TextColor3 == Color3.fromRGB(200, 60, 60) and Color3.fromRGB(100, 30, 30) or Color3.fromRGB(30, 100, 30)
end

local onclick_cryptstring = function()
encrypt_string = not encrypt_string
CryptStrings.TextColor3 = CryptStrings.TextColor3 == Color3.fromRGB(60, 200, 60) and Color3.fromRGB(200, 60, 60) or Color3.fromRGB(60, 200, 60)
CryptStrings.BorderColor3 = CryptStrings.TextColor3 == Color3.fromRGB(200, 60, 60) and Color3.fromRGB(100, 30, 30) or Color3.fromRGB(30, 100, 30)
end

local clear_logs = function()
Remotes:ClearAllChildren()
remotes_fired = 0
Total.Text = "0"
end

local filter_events = function()
local n = 0
for i, v in pairs(SetRemotes:GetChildren()) do
v.Visible = not (FilterE.TextColor3 == Color3.fromRGB(60, 200, 60) and v.Icon.Image == "rbxassetid://413369623")
if v.Visible == true then
n = n + 1
v.Position = UDim2.new(0, 10, 0, -20 + n * 30)
else
v.Position = UDim2.new(0, 10, 0, -20 + i * 30)
end
end
end

local filter_functions = function()
local n = 0
for i, v in pairs(SetRemotes:GetChildren()) do
v.Visible = not (FilterF.TextColor3 == Color3.fromRGB(60, 200, 60) and v.Icon.Image == "rbxassetid://413369506")
if v.Visible == true then
n = n + 1
v.Position = UDim2.new(0, 10, 0, -20 + n * 30)
else
v.Position = UDim2.new(0, 10, 0, -20 + i * 30)
end
end
end

local onclick_fevents = function()
FilterE.TextColor3 = FilterE.TextColor3 == Color3.fromRGB(60, 200, 60) and Color3.fromRGB(200, 60, 60) or Color3.fromRGB(60, 200, 60)
FilterE.BorderColor3 = FilterE.TextColor3 == Color3.fromRGB(200, 60, 60) and Color3.fromRGB(100, 30, 30) or Color3.fromRGB(30, 100, 30)
filter_events()
end

local onclick_ffunctions = function()
FilterF.TextColor3 = FilterF.TextColor3 == Color3.fromRGB(60, 200, 60) and Color3.fromRGB(200, 60, 60) or Color3.fromRGB(60, 200, 60)
FilterF.BorderColor3 = FilterF.TextColor3 == Color3.fromRGB(200, 60, 60) and Color3.fromRGB(100, 30, 30) or Color3.fromRGB(30, 100, 30)
filter_functions()
end

local Highlight = function(string, keywords)
   local K = {}
   local S = string
   local Token =
   {
       ["="] = true,
       ["."] = true,
       [","] = true,
       ["("] = true,
       [")"] = true,
       ["["] = true,
       ["]"] = true,
       ["{"] = true,
       ["}"] = true,
       [":"] = true,
       ["*"] = true,
       ["/"] = true,
       ["+"] = true,
       ["-"] = true,
       ["%"] = true,
[";"] = true,
["~"] = true
   }
   for i, v in pairs(keywords) do
       K[v] = true
   end
   S = S:gsub(".", function(c)
       if Token[c] ~= nil then
           return "\32"
       else
           return c
       end
   end)
   S = S:gsub("%S+", function(c)
       if K[c] ~= nil then
           return c
       else
           return (" "):rep(#c)
       end
   end)
 
   return S
end

local Tokens = function(string)
   local Token =
   {
       ["="] = true,
       ["."] = true,
       [","] = true,
       ["("] = true,
       [")"] = true,
       ["["] = true,
       ["]"] = true,
       ["{"] = true,
       ["}"] = true,
       [":"] = true,
       ["*"] = true,
       ["/"] = true,
       ["+"] = true,
       ["-"] = true,
       ["%"] = true,
[";"] = true,
["~"] = true
   }
   local A = ""
   string:gsub(".", function(c)
       if Token[c] ~= nil then
           A = A .. c
       elseif c == "\n" then
           A = A .. "\n"
elseif c == "\t" then
A = A .. "\t"
       else
           A = A .. "\32"
       end
   end)
 
   return A
end

local strings = function(string)
   local highlight = ""
   local quote = false
   string:gsub(".", function(c)
       if quote == false and c == "\"" then
           quote = true
       elseif quote == true and c == "\"" then
           quote = false
       end
       if quote == false and c == "\"" then
           highlight = highlight .. "\""
       elseif c == "\n" then
           highlight = highlight .. "\n"
elseif c == "\t" then
   highlight = highlight .. "\t"
       elseif quote == true then
           highlight = highlight .. c
       elseif quote == false then
           highlight = highlight .. "\32"
       end
   end)
 
   return highlight
end

local comments = function(string)
   local ret = ""
   string:gsub("[^\r\n]+", function(c)
       local comm = false
       local i = 0
       c:gsub(".", function(n)
           i = i + 1
           if c:sub(i, i + 1) == "--" then
               comm = true
           end
           if comm == true then
               ret = ret .. n
           else
               ret = ret .. "\32"
           end
       end)
       ret = ret
   end)
   
   return ret
end

local copy_source = function()
local script = ""
local copy
for i, v in pairs(Source:GetChildren()) do
script = script .. v.SourceText.Text .. "\n"
end
if Clipboard ~= nil then
copy = Clipboard.set
elseif Synapse ~= nil then
copy = function(str)
Synapse:Copy(str)
end
elseif setclipboard ~= nil then 
copy = setclipboard
end
copy(script)
end

local onclick_fullscreen = function()
size_pos_frame(BG, UDim2.new(1, 0, 1, 40), UDim2.new(0, 0, 0, -40))
BG.Draggable = false
end

local filter_remotes = function(type)
local n = 0
if type == "Text" then
for i, v in pairs(SetRemotes:GetChildren()) do
if v.Name:lower():match(Search.Text:lower()) and string ~= "" then
v.Visible = true
n = n + 1
else
v.Visible = false
end
if v.Visible == true then
v.Position = UDim2.new(0, 10, 0, -20 + n * 30)
else
v.Position = UDim2.new(0, 10, 0, -20 + i * 30)
end
end
end
end

local fix = function(string)
if string == "/e fix" then
show()
wait(0.3)
pos_frame(BG, UDim2.new(0.1, 0, 0.1, 0))
end
end

-- FrontEnd-Connections // UI Events

Hide.MouseButton1Down:Connect(onclick_hide)
Settings.MouseButton1Down:Connect(onclick_settings)
ClearList.MouseButton1Down:Connect(clear_logs)
EnableSpy.MouseButton1Down:Connect(onclick_remotespy)
ToClipboard.MouseButton1Down:Connect(copy_source)
CryptStrings.MouseButton1Down:Connect(onclick_cryptstring)
FullScreen.MouseButton1Down:Connect(onclick_fullscreen)
FilterE.MouseButton1Down:Connect(onclick_fevents)
FilterF.MouseButton1Down:Connect(onclick_ffunctions)
Search.Changed:Connect(filter_remotes)
game:GetService("Players").LocalPlayer.Chatted:Connect(fix)

-- Recursive Remotefill // UI-Backend

Table_TS = function(T)
   local M = {}
   for i, v in pairs(T) do
       local I = "\n\t" .. (type(i) == "number" and "[" .. i .. "] = " or "[\"" .. i .. "\"] = ")
       table.insert(M, I .. (type(v) == "table" and Table_TS(v) or GetType(v)))
   end
   
   return "\n{" .. table.concat(M, ", ") .. "\n}"
end

function fill(base)
for i, v in pairs(base:GetChildren()) do
if v.ClassName:match("Remote") and v.Name ~= "CharacterSoundEvent" then
local B = SBTN:Clone()

B.Parent = SetRemotes
B.Icon.Image = (v.ClassName == "RemoteEvent" and "rbxassetid://413369506" or "rbxassetid://413369623")
B.RemoteName.Text = v.Name
B.ID.Text = GetPath(v)
B.Name = v.Name
B.Position = UDim2.new(0, 10, 0, -20 + #SetRemotes:GetChildren() * 30)
B.MouseButton1Down:Connect(function()
B.Enabled.Text = B.Enabled.Text == "Enabled" and "Disabled" or "Enabled"
B.Enabled.TextColor3 = B.Enabled.Text == "Enabled" and Color3.fromRGB(60, 200, 60) or Color3.fromRGB(200, 60, 60)
B.Enabled.BorderColor3 = B.Enabled.Text == "Enabled" and Color3.fromRGB(30, 100, 30) or Color3.fromRGB(100, 30, 30)
end)
end
fill(v)
end
end

fill(game)

-- Backend // Remotespy Backend

local game_meta = getrawmetatable(game)
local game_namecall = game_meta.__namecall
local namecall_dump = {}
local current_rmt = nil
local g_caller = nil
local f_return = nil
local Step = game:GetService("RunService").Stepped

local mwr

if setreadonly ~= nil then
mwr = function()
setreadonly(game_meta, false)
end
elseif make_writeable ~= nil then 
mwr = function()
make_writeable(game_meta)
end
end

mwr()

local namecall_script = function(object, method, ...)
local script = "-- Script generated by R2Sv2\n-- R2Sv2 developed by Luckyxero\n\32\n"
local args = {}
for i, v in pairs{...} do
script = script .. "local A_" .. i .. " = " .. (type(v) == "table" and Table_TS(v) or GetType(v)) .. "\n"
table.insert(args, "A_" .. i)
end
script = script .. "local Event = " .. GetPath(object) .. "\n\n"
script = script .. "Event:" .. method .. "(" .. table.concat(args, ", ") .. ")"

return script
end

local dump_script = function(script)
Source:ClearAllChildren()
local lines = 0
script:gsub("[^\r\n]+", function(c)
lines = lines + 1
local tabs = 0
c:gsub("%\t", function() tabs = tabs + 1 end)
local line = ScriptLine:Clone()
line.Parent = Source
line.SourceText.Text = c 
line.Line.Text = lines
line.RemoteHighlight.Text = Highlight(c, {"FireServer", "InvokeServer", "invokeServer", "fireServer"})
line.Position = UDim2.new(0, tabs * (17 * 2), 0, -17 + #Source:GetChildren() * 17)
line.Globals.Text = Highlight(c, global_env)
line.Line.Position = UDim2.new(0, 0 - tabs * (17 * 2), 0, 0)
line.Strings.Text = strings(c)
line.Keywords.Text = Highlight(c, lua_keywords)
line.Tokens.Text = Tokens(c)
line.Comments.Text = comments(c)
end)
end

local log_remote = function(table)
if SetRemotes[table.object.Name].Enabled.Text == "Disabled" then return end
local B = RBTN:Clone()
g_caller = table.caller
remotes_fired = remotes_fired + 1
Total.Text = remotes_fired

B.Parent = Remotes
B.Position = UDim2.new(0, 10, 0, -20 + #Remotes:GetChildren() * 30)
B.Icon.Image = table.method == "FireServer" and "rbxassetid://413369506" or "rbxassetid://413369623"
B.RemoteName.Text = table.object.Name
B.ID.Text = tostring(remotes_fired)
B.MouseButton1Down:Connect(function()
dump_script(table.script)
g_caller = table.caller
f_return = table.freturn == nil and table.object.Name .. " is not RemoteFunction" or table.freturn
end)
end


local get_namecall_dump = function(script, object, ...)
local Ret = nil
if object.ClassName == "RemoteFunction" then
local freturn = {pcall(object.InvokeServer, object, ...)}
freturn = {select(2, unpack(freturn))}

if #freturn == 0 then
Ret = object.Name .. " is a void type RemoteFunction."
else
Ret = Table_TS(freturn)
end
end
namecall_dump[#namecall_dump + 1] = 
{ 
script = namecall_script(object, object.ClassName == "RemoteEvent" and "FireServer" or "InvokeServer", ...),
caller = script,
object = object,
method = object.ClassName == "RemoteEvent" and "FireServer" or "InvokeServer",
freturn = Ret
}
end

GetReturn.MouseButton1Down:Connect(function()
dump_script(f_return)
end)

Decompile.MouseButton1Down:Connect(function()
local source = decompile(g_caller)

dump_script(type(source) == "boolean" and "Failed to decompile caller script!" or source)
end)

Step:Connect(function()
while #namecall_dump > 0 do
log_remote(table.remove(namecall_dump, 1))
end
end)

local on_namecall = function(object, ...)
local method = getnamecallmethod()
local args = {...}
args[#args] = nil
if object.Name ~= "CharacterSoundEvent" and method:match("Server") and spy_enabled == true then get_namecall_dump(getfenv(2).script, object, unpack(args)) end

return game_namecall(object, ...)
end

game_meta.__namecall = on_namecall
