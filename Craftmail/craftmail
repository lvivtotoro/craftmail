-- ORIGINAL PROGRAM BY MIDNIGHTAS --

os.loadAPI("disk/json")

w, h = term.getSize()

-- limitRead: Source by remiX on the ComputerCraft forums.
function limitRead(nLimit, specialChars, replaceChar)
  term.setCursorBlink(true)
  local cX, cY = term.getCursorPos()
  local rString = ""
  if replaceChar == "" then replaceChar = nil end
  repeat
    local event, p1 = os.pullEvent()
    if event == "char" then
      -- Character event
      if #rString + 1 <= nLimit then
      if string.match(p1, "%W") and specialChars == false then
      p1 = ""
    end
        rString = rString .. p1
    write(replaceChar or p1)
      end
    elseif event == "key" and p1 == keys.backspace and #rString >= 1 then
      -- Backspace
      rString = string.sub(rString, 1, #rString-1)
      xPos, yPos = term.getCursorPos()
    if xPos > 1 then
        term.setCursorPos(xPos - 1, yPos)
        write(" ")
        term.setCursorPos(xPos - 1, yPos)
    else
      if #rString ~= 0 then
        term.setCursorPos(w, yPos - 1)
      write(" ")
          term.setCursorPos(w, yPos - 1)
    end
    end
    end
  until event == "key" and p1 == keys.enter and #rString > 0
  term.setCursorBlink(false)
  print() -- Skip to the next line after clicking enter.
  return rString
end

function printCenter(text, y, offset)
  offset = offset or 0
  local x = math.floor(w - string.len(text)) / 2 + offset
  term.setCursorPos(x, y)
  print(text)
  return x
end

function drawLogo()
  printCenter("    _____            __ _                    _ _ ", 3)
  printCenter("   / ____|          / _| |                  (_| |", 4)
  printCenter("  | |     _ __ __ _| |_| |_    __ ___   __ _ _| |", 5)
  printCenter("  | |    | '__/ _` |  _| __|  /_ ` _ \\ / _` | | |", 6)
  printCenter("  | |____| | | (_| | | | |_   | | | | | (_| | | |", 7)
  printCenter("    \\_____|_|  \\__,_|_| \\_ _|  |_| |_|  \\__,_|_|_|", 8)
end

function drawBackground()
  term.setBackgroundColor(colors.orange)
  term.setTextColor(colors.black)
  term.clear()
end

username = nil
password = nil

-- States: -1 = register; 0 = menu; 1 = login; 2 = send; 3 = read page; 4 = read message;
state = 0
menuOption = 0

readPage = 1
mails = nil
selectedMail = nil

function drawMenu()
  drawLogo()
  term.setBackgroundColor(colors.orange)
  term.setTextColor(colors.black)
  if password ~= nil then
  if menuOption == 0 then
    term.setBackgroundColor(colors.lightGray)
  end
  btnSendX = printCenter("[ SEND ]", h / 2 + 3)
  term.setBackgroundColor(colors.orange)
  if menuOption == 1 then
    term.setBackgroundColor(colors.lightGray)
  end
  btnReadX = printCenter("[ READ ]", h / 2 + 4)
  term.setBackgroundColor(colors.orange)
  if menuOption == 2 then
    term.setBackgroundColor(colors.lightGray)
  end
  btnExitX = printCenter("[ EXIT ]", h / 2 + 5)
  term.setBackgroundColor(colors.orange)
  else
  if menuOption == 0 then
    term.setBackgroundColor(colors.lightGray)
  end
  btnSendX = printCenter("[ SIGN IN ]", h / 2 + 3)
  term.setBackgroundColor(colors.orange)
  if menuOption == 1 then
      term.setBackgroundColor(colors.lightGray)
  end
  btnReadX = printCenter("[ SIGN UP ]", h / 2 + 4)
  term.setBackgroundColor(colors.orange)
  if menuOption == 2 then
      term.setBackgroundColor(colors.lightGray)
  end
  btnReadX = printCenter("[  EXIT   ]", h / 2 + 5)
  term.setBackgroundColor(colors.orange)
  end
end

function drawLabel()
  term.setBackgroundColor(colors.lightGray)
  term.setCursorPos(1, 1)
  term.clearLine()
  printCenter("CRAFTMAIL CLIENT", 1)
  term.setBackgroundColor(colors.red)
  term.setTextColor(colors.black)
  term.setCursorPos(w,1)
  write("X")
end

function drawState(id)
  if id == 2 then
    term.setBackgroundColor(colors.orange)
  term.setTextColor(colors.black)
  term.setCursorPos(w / 2 - 5, 6)
  write("RECIPIENT:")
  paintutils.drawFilledBox(w / 2 - 5, 7, w / 2 + 5, 7, colors.lightGray)
  term.setCursorPos(w / 2 - 5, 8)
    term.setBackgroundColor(colors.orange)
  write("SUBJECT:")
  paintutils.drawFilledBox(w / 2 - 10, 9, w / 2 + 10, 9, colors.lightGray)
  term.setCursorPos(1, 10)
    term.setBackgroundColor(colors.orange)
  write("MESSAGE (MULTILINE NOT SUPPORTED):")
  term.setCursorPos(1, 11)
  elseif id == 1 then
    drawLogo()
  paintutils.drawFilledBox(w / 2 - 5, h - 6, w / 2 + 5, h - 6, colors.lightGray)
  elseif id == -1 then
    drawLogo()
  elseif id == 3 then
  line = 3
  term.setBackgroundColor(colors.orange)
    term.setTextColor(colors.black)
  term.setCursorPos(1, 1)
  term.clearLine()
  write("BACK - BACKSPACE | SCROLL = LEFT & RIGHT ARROWS")
  term.setBackgroundColor(colors.lightGray)
  for i = readPage * 8 - 8, readPage * 8, 1 do
    local v = mails[i + 1]
      if v ~= nil then
    term.setCursorPos(1, line)
    term.clearLine()
    print(v[2] .. " - " .. string.sub(v[5], 0, w / 2))
    line = line + 1
    end
  end
  term.setBackgroundColor(colors.orange)
  term.setCursorPos(1, h)
  write(readPage .. "/" .. math.floor(table.getn(mails) / 8) + 1)
  elseif id == 4 then
    local v = mails[selectedMail + 1]
    term.setCursorPos(1, 1)
  term.setBackgroundColor(colors.orange)
  term.setTextColor(colors.black)
  print("Press BACKSPACE to leave.");
  print("Mail ID: " .. v[1])
  print("From: " .. v[2])
  print()
  print(v[4])
  print(string.sub("----------------------------------------------------------------------------", 0, w))
  print(v[5])
  end
end

function draw()
  drawBackground()
  if state == 0 then
    drawMenu()
  else
    drawState(state)
  end
end

drawBackground()
drawMenu()

while true do
  local event, param1, param2, param3 = os.pullEvent()
  if event == "key" then
    if state == 0 then
    if param1 == keys.down then
      if menuOption == 0 then
      menuOption = 1
    elseif menuOption == 1 then
      menuOption = 2
    end
    drawMenu()
      elseif param1 == keys.up then
      if menuOption == 2 then
      menuOption = 1
    elseif menuOption == 1 then
      menuOption = 0
    end
    drawMenu()
      elseif param1 == keys.enter then
      if menuOption == 2 then
      break
    end
      if password ~= nil then
          if menuOption == 0 then
        state = 2
        draw()
      term.setCursorPos(w / 2 - 5, 7)
      term.setBackgroundColor(colors.lightGray)
      local recipient = limitRead(10, false)
      term.setCursorPos(w / 2 - 10, 9)
      local subject = limitRead(20, true)
      term.setCursorPos(1, 11)
      term.setBackgroundColor(colors.orange)
      local message = limitRead((h - 11) * w, true)
      term.setBackgroundColor(colors.orange)
      term.clear()
      printCenter("Accessing remote database", h / 2)
      local page = http.get("http://midnightasapi.vapr.cc/?request=mail&username=" .. username .. "&password=" .. textutils.urlEncode(password) .. "&recipient=" .. recipient .. "&subject=" .. textutils.urlEncode(subject) .. "&message=" .. textutils.urlEncode(message))
      local pageContent = page.readAll()
      if string.find(pageContent, "1") then
        printCenter("Mail sent", h / 2 + 1)
      elseif string.find(pageContent, "0") then
        printCenter("Error found", h / 2 + 1)
      end
      printCenter("Press any key to continue", h / 2 + 2)
          os.pullEvent("key")
      state = 0
      draw()
      elseif menuOption == 1 then
      term.setBackgroundColor(colors.orange)
      term.clear()
      printCenter("Accessing remote database", h / 2)
      local page = http.get("http://midnightasapi.vapr.cc/?request=read&username=" .. username .. "&password=" .. textutils.urlEncode(password))
      mails = {}
      local obj = json.decode(page.readAll())
      for k in pairs(obj) do
        local v = obj[k]
        table.insert(mails, obj[k])
      end
      printCenter("Request successful", h / 2 + 1)
      printCenter("Press any key to continue", h / 2 + 2)
          os.pullEvent("key")
      state = 3
      draw()
      end
    else
      if menuOption == 0 then
      state = 1
      draw()
      term.setCursorPos(w / 2 - 5, h - 7)
      term.setBackgroundColor(colors.orange)
      term.setTextColor(colors.black)
      write("USERNAME:")
      term.setBackgroundColor(colors.lightGray)
      term.setCursorPos(w / 2 - 5, h - 6)
      local newUsername = limitRead(10, false)
      term.setBackgroundColor(colors.orange)
      term.setCursorPos(w / 2 - 5, h - 5)
      write("PASSWORD:")
      paintutils.drawFilledBox(w / 2 - 5, h - 4, w / 2 + 5, h - 4, colors.lightGray)
      term.setBackgroundColor(colors.lightGray)
      term.setCursorPos(w / 2 - 5, h - 4)
      newPassword = limitRead(10, true, "*")
      term.setBackgroundColor(colors.orange)
      term.clear()
      printCenter("Accessing remote database", h / 2)
      local page = http.get("http://midnightasapi.vapr.cc/?request=login&username=" .. newUsername .. "&password=" .. textutils.urlEncode(newPassword))
      local pageContent = page.readAll()
      if string.find(pageContent, "1") then
        printCenter("Request successful", h / 2 + 1)
        username = newUsername
        password = newPassword
      elseif string.find(pageContent, "0") then
        printCenter("Invalid credentials", h / 2 + 1)
      end
      printCenter("Press any key to continue", h / 2 + 2)
          os.pullEvent("key")
      state = 0
      draw()
      elseif menuOption == 1 then
        state = -1
      draw()
      paintutils.drawFilledBox(w / 2 - 5, h - 6, w / 2 + 5, h - 6, colors.lightGray)
      term.setBackgroundColor(colors.orange)
      term.setTextColor(colors.black)
      term.setCursorPos(w / 2 - 5, h - 7)
      write("USERNAME:")
      term.setBackgroundColor(colors.lightGray)
      term.setCursorPos(w / 2 - 5, h - 6)
      local newUsername = limitRead(10, false)
      term.setBackgroundColor(colors.orange)
      term.setCursorPos(w / 2 - 5, h - 5)
      write("PASSWORD:")
      paintutils.drawFilledBox(w / 2 - 5, h - 4, w / 2 + 5, h - 4, colors.lightGray)
      term.setBackgroundColor(colors.lightGray)
      term.setCursorPos(w / 2 - 5, h - 4)
      local newPassword = limitRead(10, true, "*")
      term.setBackgroundColor(colors.orange)
      term.clear()
      printCenter("Accessing remote database", h / 2)
      local page = http.get("http://midnightasapi.vapr.cc/?request=register&username=" .. newUsername .. "&password=" .. textutils.urlEncode(newPassword))
      local pageContent = page.readAll()
      if string.find(pageContent, "1") then
        printCenter("Registration successful", h / 2 + 1)
      elseif string.find(pageContent, "0") then
        printCenter("Username already is registered", h / 2 + 1)
      elseif string.find(pageContent, "2") then
        printCenter("Using hacked client, cancelling registration.", h / 2 + 1)
      end
      printCenter("Press any key to continue", h / 2 + 2)
          os.pullEvent("key")
      state = 0
      draw()
      end
    end
  end
  elseif state == 3 then
    if param1 == keys.backspace then
      state = 0
    draw()
    elseif param1 == keys.left then
      if readPage > 1 then
      readPage = readPage - 1
    end
    elseif param1 == keys.right then
      if readPage < math.floor(table.getn(mails) / 8) + 1 then
      readPage = readPage + 1
    end
    end
  elseif state == 4 then
    if param1 == keys.backspace then
    state = 3
    draw()
    elseif param1 == keys.delete then
    term.setBackgroundColor(colors.orange)
    term.clear()
    printCenter("Accessing remote database", h / 2)
    local page = http.get("http://midnightasapi.vapr.cc/?request=deletemail&username=" .. username .. "&password=" .. textutils.urlEncode(password) .. "&id=" .. mails[selectedMail + 1][1])
    local pageContent = page.readAll()
    if string.find(pageContent, "1") then
      printCenter("Mail deleted", h / 2 + 1)
    elseif string.find(pageContent, "0") then
        printCenter("Failed to delete mail.", h / 2 + 1)
    end
    printCenter("Press any key to continue", h / 2 + 2)
      os.pullEvent("key")
    state = 0
    draw()
    end
  end
  elseif event == "mouse_click" then
    if state == 3 then
    if param3 >= 3 and param3 <= 11 then
    selectedMail = (8 * readPage) - 8 + param3 - 3
    if mails[selectedMail + 1] ~= nil then
      state = 4
        draw()
    end
    end
  end
  end
end
term.setBackgroundColor(colors.black)
term.setTextColor(colors.yellow)
term.clear()
term.setCursorPos(1, 1)
print("Thank you for using CraftMail service.")
print("Project by Midnightas")