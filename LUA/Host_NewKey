local tArgs = {...}
local ModemSide = "left";
local SCREEN = {550,1024};
local MAX_COLOR = 16777215;
local COLOR_SCHEME = {"4C58FF","1E257F","0011FF","000DCC","00087F"};
local SIDE = "right";
local LOG_ACTIV = true;
local HELP = false;
local POSX = 5;
local POSY = 5;
if fs.exists("logs") then
    fs.delete("logs");
    fs.makeDir("logs");
end
--[Pre Init 
local modem = peripheral.wrap(ModemSide);
--local isWireless = modem.isWireless();
--]Pre Init
--]Color fUNCTIONS  
function color_h(HEX_VALUE)
	local int_value = tonumber(HEX_VALUE,16);
	return int_value;
end
local org_type = type;
function type(var)
    if org_type(var) == "table" then
        if var.__type ~= nil then
            return var.__type;
        else
           return org_type(var);
        end
    else
      return org_type(var);
    end
end
--]Color fUNCTIONS 
--[VECTOR

function Vector(Int_x,Int_y)
    local self=
    {
       x,
       y,
       __type 
    }
    local op = 
    {
     __add,   
     __index,
     __sub,
    }
    self.x = Int_x or 0;
    self.y = Int_y or 0;
    self.__type = "vector";
	function self.test()
        return "test";
    end
    
    
    --[Vector Operators
    op.__add = function(lhs,rhs)
        if type(lhs) == "vector" then
            if type(rhs) == "vector" then
                return Vector(lhs.x+rhs.x,lhs.y+rhs.y);
            elseif type(rhs) == "string" then
                return lhs.x..":x "..lhs.y..":y "..rhs;
            elseif type(rhs) == "number" then 
                return Vector(lhs.x+rhs,lhs.y+rhs);  
            end
        elseif type(lhs) == "string" then
                if type(rhs) == "vector" then
                    return rhs.x..":x "..rhs.y..":y "..lhs;
                end        
        elseif type(lhs) == "number" then 
                if type(rhs) == "vector" then
                    return Vector(rhs.x+lhs,rhs.y+lhs);
                end
        end
    end
    op.__sub = function(lhs,rhs)
        if type(lhs) == "vector" then
            if type(rhs) == "vector" then
                return Vector(lhs.x-rhs.x,lhs.y-rhs.y);
            elseif type(rhs) == "string" then
                return lhs.x..":x "..lhs.y..":y "..rhs;
            elseif type(rhs) == "number" then 
                return Vector(lhs.x-rhs,lhs.y-rhs);  
            end
        elseif type(lhs) == "string" then
                if type(rhs) == "vector" then
                    return rhs.x..":x "..rhs.y..":y "..lhs;
                end        
        elseif type(lhs) == "number" then 
                if type(rhs) == "vector" then
                    return Vector(rhs.x-lhs,rhs.y-lhs);
                end
        end
    end
    op.__index = function(t,k)
        if k == 1 then
            return self.x;
        elseif k == 2 then
            return self.y;
        elseif k == 3 then
            return self.y.." "..self.x
        end
     end
 --]Vector Operators
     setmetatable(self,op);
    --]private values
	function self.equal(vec)
        if type(vec) == "vector" then
            if self.y == vec.y and self.x == vec.x then
                return true;
            else
               return false;
            end
        else
            return false;
        end
    end
    
    return self;
end
--]VECTOR

--[FILE WRAPPER -> UTIL
function file()
	local self=
	{
		locale,arraylines,isAktiv,text,amount
	}
	self.text = "";
	self.arraylines = {};
	self.amount = 0;
	--[private values
	local tmp ="";
	local file_ptr;
	local modus = 0;
	local size = 0;
	local openinWrite = false;
	local openinBin = false;
	local reading = false;
	local path = "";
	--]private values
	function self.new(Path_string,mode_int)
		local mode;
		self.amount = self.amount + 1;
		path = Path_string;
		modus = mode_int;
		if mode_int == 1 then
			mode = "r";
			if fs.exists(Path_string) then  
			  file_ptr = fs.open(Path_string,mode);
			end
		elseif mode_int == 2 then
			mode = "w";
			openinWrite = true;
		elseif mode_int == 3 then
			mode = "a";
			openinWrite = true;
		elseif mode_int == 4 then
		   mode =  "wb";
		   openinBin = true;
		elseif mode_int == 5 then 
			mode = "rb";
			reading = true;
			openinBin = true;
			if fs.exists(Path_string) then  
			  file_ptr = fs.open(Path_string,mode);
			end
		end 
		if not (mode == "r" or mode == "rb") then
			file_ptr = fs.open(Path_string,mode);
		end 
	end

	function self.open()
	  self.isAktiv = true;
	  if modus == 1 then
		local read = true;
		local iter = 0;
		self.arraylines = {};
			while read do
			   self.arraylines[iter] = file_ptr.readLine()
			   if self.arraylines[iter] == nil  then
				   table.remove(self.arraylines,iter);
				   size = iter-1;
				   read = false;
			   end
			   iter = iter + 1;
			end
			for i=0,size,1 do
				self.text = self.text .. self.arraylines[i];
			end
		elseif modus == 2 then
		  self.isAktiv = true; 
		elseif modus == 3 then
		   self.isAktiv = true; 
		elseif modus == 4 then
			self.isAktiv = true;
		elseif modus == 5 then
			self.isAktiv = true;
			openinBin = true;
			reading = true
			 local chars = "";
			 local iter = 0; 
			 local ireading = true;
				 while ireading do 
					local red = file_ptr.read();
					if red ~= nil then 
					self.text = self.text .. red;
					self.arraylines[iter] = red;
					iter = iter + 1;
					size = iter-1;
					else 
						ireading = false;
					end 
				 end 
		end
	end
	function self.get(Line_int)
	   if not openinWrite then
			return self.arraylines[Line_int]
	   end
	end
	function self.write(Text_String)
		if openinWrite then
		   file_ptr.write(Text_String); 
		end
	end
	function self.print(Text_String)
		if openinWrite then
		   file_ptr.write(Text_String.."\n"); 
		elseif openinBin then
			self.binWrite(Text_String)
		end
	end
	
	function self.binw(Binary_Int64)--MAX VAL= 9.223.372.036.854.775.807
	   if  openinBin then
		   file_ptr.write(Binary_Int64);
	   end
	end
	
	function self.binr()
	  if openinBin then
		  if reading then
			  self.new(path,5)
			  return file_ptr.read();
		  end
	  end  
	end
	function self.bintotext()
	   if openinBin then
		if reading then
			local string_text = "";
			for i=0,size,1 do
			 --print(self.arraylines[i]);
			  string_text = string_text .. string.char(self.arraylines[i]);
			end
		  return string_text;
		end 
	   end 
	end
	function self.flush()
		file_ptr.flush();
	end
	--function error(Error_String)
	--   print("Error:"..Error_String);
	-- end
	function self.close()
	   arraylines = {};
	   isAktiv = false;
	   text = "";
	   modus = 0;
	   file_ptr.close();
	end

	return self;
end
--]FILE WRAPPER  
--]LOGGER
function logger(max,maxlogi,id_string,isAktiv)
	local self=
	{
	   size,maxsize,maxlog,id,f
	}
	self.maxsize = max or 90;
	self.size = 0;
	self.maxlog = maxlogi or 5;
	self.id = id_string or "std"
	--[private values
	local loglines = 0;
	local iter_getlog = -1;
	local logs = {}
	local isEnabled = isAktiv or false;
	local time = self.id;--os.time();
	self.f = file();
	if LOG_ACTIV == true then
		if not fs.exists("logs") then 
			fs.makeDir("logs");  
		end
	end
	--]private values
	function self.getlog()
		iter_getlog = iter_getlog + 1;
		getlogsfromFile()
		return logs[iter_getlog]; 
	end
	
	function self.decrap()
	  self.f.new("/logs/logger_"..time..".ptl",2);
	  self.f.write("[CLEARD]\n");
	  self.f.close();
	end
	
	function self.log(Text_String)
		if self.size == self.maxsize then 
			self.decrap()
		end
		self.enable();
		if isEnabled == true and LOG_ACTIV then    
		   self.f.write("[LOG]:["..self.id.."]:["..textutils.formatTime(os.time(),true).."]:"..Text_String.."\n");
		   self.size = self.size  + 1;
		   self.disable(); 
		end
	end
	
	function self.krit(Text_String)
		if self.size == self.maxsize then 
			self.decrap()
		end
		self.enable();
		if isEnabled == true and LOG_ACTIV then 
			self.f.write("[LOG]:[FAILURE]:".."["..self.id.."]:".."["..textutils.formatTime(os.time(),true).."]: "..Text_String.."\n");
			self.size = self.size  + 1; 
			self.disable();
		end
	end
	function self.enable()
		if LOG_ACTIV then     
			isEnabled = true;
			self.f.new("/logs/logger_"..time..".ptl",3);
		end
	end
	
	function self.disable()
	   if isEnabled == true and LOG_ACTIV then
		   isEnabled = false;
		   self.f.flush();
		   self.f.close();
	   end
	end
	
	function getlogsfromFile()
	   self.f.new("/logs/logger_"..time..".ptl",1);
	   self.f.open();
	   logs = f.arraylines;
	end
	
	return self;
end
--]LOGGER
function args(argmuent_strings)
	local self=
	{
		argument
	}
	self.argmuent = {};
	for i,v in ipairs(argmuent_strings) do
		self.argmuent[i] = v;
	end
	
	function self.is(stirng_arg)
		for i=0,#self.argmuent,1 do
			if self.argmuent[i] == stirng_arg then
				return true;
			end
		end
		return false;
	end
	
	function self.add(argmuent)
	   self.argmuent[#self.argmuent+1] = argmuent;
	end
	function self.toString()
		local string = "";
		for i=1,#self.argmuent do
		   string = string .. " " .. self.argmuent[i]; 
		end
		return string;
	end
	return self;
end
function ComParser()
	
	local self=
	{
		arguments,
		lenght,
		Slot_startX,
		Slot_startY,
		Slot_end,
		margin,
		show_log,
		opacity_val,
		color,
		console
	}
	self.console = logger(900,50,"ComParser",LOG_ACTIV);
	LOG_ACTIV = true;
	self.console.log("TEST");
	self.Slot_startX = 5;
	self.Slot_startY = 5;
	self.opacity_val = 9.0;
	self.color = color_h(COLOR_SCHEME[1]);
	self.arguments = tArgs;
	self.lenght = #tArgs;
	self.margin = 5;
	local num;
	
	local logg = args({"-l","-L","--log","--Log"});
	local displaylog = args({"-dl","-Dl","--displaylog","--DisplayLog"});
	local slot = args({"--slot","--Slot","-s","-S"});
	local margin_args = args({"-m","-M","--margin","--Margin"});
	local opacity = args({"-o","-O","--opacity","--Opacity"});
	local help = args({"-h","-H","--help","--Help"});
	local color = args({"-c","-C","-color","-Color"})
	self.console.log("Loadet Argument array");
	
	for i=1,#tArgs,1 do
	   if help.is(tArgs[i]) then
		HELP = true;
		term.clear();
		term.setTextColor(colors.brown);
		print("---------------------------------------------------")
		term.setTextColor(colors.lime);
		print("Valid args Are:")
		term.setTextColor(colors.green);
		print(opacity.toString().."<0.1 - 0.9>");
		term.setTextColor(colors.white);
		print("Sets the Opacity of the ItemGrid");
		term.setTextColor(colors.green);
		print(logg.toString());
		term.setTextColor(colors.white);
		print("Disables Logging");
		term.setTextColor(colors.green);
		print(displaylog.toString());
		term.setTextColor(colors.white);
		print("Shows log not in LogDisplay")
		term.setTextColor(colors.green);
		print(slot.toString().." <posX,posY 'number'>")
		term.setTextColor(colors.white);
		print("Sets the ItemGrid Position")
		term.setTextColor(colors.green);
		print(margin_args.toString().."<margin 'number'>")
		term.setTextColor(colors.white);
		print("Sets the ItemGrid Margins")
		term.setTextColor(colors.green);
		print(color.toString().."<color 'HEX number'>")
		term.setTextColor(colors.white);
		print("Sets the Color of the ItemGrid");
		term.setTextColor(colors.brown);
		print("---------------------------------------------------")
		term.setTextColor(colors.white)
	   elseif opacity.is(tArgs[i]) then
		local op = args({"0.0","0.1","0.2","0.3","0.4","0.5","0.6","0.7","0.8","0.9","1.0"})
		   if op.is(tArgs[i+1]) then
			 self.opacity_val = tArgs[i+1];
			 self.console.log("loadet Opacity:"..self.opacity_val);
			 i = i + 1;
		   end
	   elseif color.is(tArgs[i]) then
			 self.color = color_h(tArgs[i+1])
			 self.console.log("Color Setted to: "..self.color);
	   elseif logg.is(tArgs[i]) then
		   LOG_ACTIV = false;
	   elseif displaylog.is(tArgs[i]) then
		self.console.log("Log Will Not Be Displayed");
		   show_log = false;
	   elseif margin_args.is(tArgs[i]) then
		   num = tonumber(tArgs[i+1]);
		   if type(num) == "number" then
				self.margin = tArgs[i+1];
				self.console.log("Margin Setted to: "..self.margin);
		   end
	   elseif slot.is(tArgs[i]) then
		  self.console.log("Argument Slot Parsing");
		  num = tonumber(tArgs[i+1]);
			if type(num) == "number" then
				self.console.log("Slot ParsingX:"..tArgs[i+1]);
				self.Slot_startX = tArgs[i+1];
			end
		  num = tonumber(tArgs[i+2]);
			if type(num) == "number" then
				self.console.log("Slot ParsingY:"..tArgs[i+2]);
				self.Slot_startY = tArgs[i+2];
			end
		  i = i+2;
	   
	   end
	end

	return self;
end
function item_slot(x,y,x_size,y_size,color,Item_ID,Glass_Handle_ptr,back_opacity)
	local self=
	{
		--Global_Values
		xpos,ypos,xsize,ysize,id,opacity,color
	}
	--Size of one Item ~15
	self.id = Item_ID or "minecraft:dirt"; 
	self.xsize = x_size or 20;
	self.ysize = y_size or 20;
	self.xpos = x or 5;
	self.ypos = y or 5;
	self.opacity = back_opacity or 0.8;
	self.color = color_h(color or COLOR_SCHEME[1]) or color_h(COLOR_SCHEME[1]);
	--[private values
	local Glass_Handle = Glass_Handle_ptr or peripheral.wrap(SIDE);
	local Item_size = 8;   
	local enable = false;
	local ItemOffsetX = (self.xsize / 2) - Item_size;
	local ItemOffsetY = (self.ysize / 2) - Item_size;
	--]private values
	function self.enable()
		enable = true;
	end
	
	function self.disable()
	   enable = false;
	end
	
	function self.render()
	 ItemOffsetX = (self.xsize / 2) - Item_size;
	 ItemOffsetY = (self.ysize / 2) - Item_size;
	 self.enable();
		Glass_Handle.addBox(self.xpos,self.ypos,self.xsize,self.ysize,self.color,self.opacity);
		Glass_Handle.addIcon(self.xpos+ItemOffsetX,self.ypos+ItemOffsetY,self.id);
	 self.disable(); 
	end
	return self;
end
--[Slot Array Function
function Slot_x16()
	local self=
	{
		slot_array,
		sizeX,
		sizeY,
		marginS,
		marginB,
		posX,
		posY,
		color,
		BASE_posX,
		Glass_Handle,
        
		--idList,DEPRECED
		--opacityList,DEPRECED
		--xlist,DEPRECED
		--ylist,DEPRECED
	}
    local console = logger(90,30,"Slot_x16",LOG_ACTIV);
	self.Glass_Handle = peripheral.wrap(SIDE)
	self.Glass_Handle.clear();
	self.Glass_Handle.sync();
	self.slot_array = {}
	self.color = color_h(COLOR_SCHEME[1]);
	self.marginS = 2;
	self.marginB = 2;
	self.sizeX = 20;
	self.sizeY = 20;
	self.posX = POSX;
	self.posY = POSY;
	self.BASE_posX = POSX;
    console.log("Making BaseInit");
	--OPTION LISTS
	--OPTION LISTS
	local setSlotinUSE = false;
	for i=1,16,1 do 
		self.slot_array[i] = item_slot();
	end
    console.log("Succsesfull Loadet Slot Array");
  
	function self.init() 
		local iter = 1;
		for o=1,4,1 do
		   for z=1,4,1 do
			   if self.slot_array[iter].xpos == 5 then
				 self.slot_array[iter].xpos = self.posX;
			   end
			   if self.slot_array[iter].ypos == 5 then
				 self.slot_array[iter].ypos = self.posY;
			   end
			   self.posX = (self.posX + self.sizeX) + self.marginS;
			   iter = iter + 1; 
		   end
		  self.posY = self.posY + self.sizeY + self.marginB;
		  self.posX = self.BASE_posX;
		end
        self.posY = POSY;
        self.posX = POSX;
        console.log("Initialised Positions");
	end
	
	--[SINGEL SLOT FUNCTIONS 
	function self.setSlot_Pos(Slot_int,x,y)
		console.log("Setting Slot"..Slot_int.."to Position"..x.." "..y);
        if Slot_int > #self.slot_array then
		  Slot_int = 1;
		elseif Slot_int == 0 then
		  Slot_int = 1;
		end
		tx = x or 5;
		ty = y or 5;
		self.slot_array[Slot_int].xpos = tx;
		self.slot_array[SLot_int].ypos = ty;
	end
	
	function self.setSlot_Item(Slot_int,id_string)
        console.log("Setting Slot"..Slot_int.."id to"..id_string);
		if Slot_int > #self.slot_array then
		  Slot_int = 1;
		elseif Slot_int == 0 then
		  Slot_int = 1;
		end
		self.slot_array[Slot_int].id = id_string;
	end
	 
	function self.setSlot_Opacity(Slot_int,opacity_int)
		console.log("Setting Slot"..Slot_int.."opacity to"..opacity_int);
        if Slot_int > #self.slot_array then
		  Slot_int = 1;
		elseif Slot_int == 0 then
		  Slot_int = 1;
		end
		self.slot_array[Slot_int].opacity = opacity_int;
	end
	
	function self.setSlot_Color(Slot_int,color)
        console.log("Setting Slot"..Slot_int.."color to"..color);
		if Slot_int > #self.slot_array then
		  Slot_int = 1;
		elseif Slot_int == 0 then
		  Slot_int = 1;
		end 
		self.slot_array[Slot_int].color = color;
	end
	
	function self.setSlot_Size(Slot_int,sx,sy)
		console.log("Setting Slot"..Slot_int.."Size X/Y to"..sx.." "..sy);
        if Slot_int > #self.slot_array then
		  Slot_int = 1;
		elseif Slot_int == 0 then
		  Slot_int = 1;
		end
		self.slot_array[Slot_int].xsize = tsx;
		self.slot_array[Slot_int].ysize = tsy;
	end
	
	
	function self.setSlot(Slot_int,id_string,color,opacity_lvl,x,y,sx,sy)
	  console.log("Setting Slot"..Slot_int.."All");
      if Slot_int > #self.slot_array then
		  Slot_int = 1;
	  elseif Slot_int == 0 then
		  Slot_int = 1;
	  end
	  tcolor = color_h(color or COLOR_SCHEME[1]);
	  tx = x or 5;
	  ty = y or 5;
	  tid = id_string or "minecraft:stone";
	  tsx = sx or 20;
	  tsy = sy or 20;
	  topacity = opacity_lvl or 0.9;
	  
	  self.slot_array[Slot_int].xpos = tx;
	  self.slot_array[Slot_int].ypos = ty;
	  self.slot_array[Slot_int].xsize = tsx;
	  self.slot_array[Slot_int].ysize = tsy;
	  self.slot_array[Slot_int].color = tcolor;
	  self.slot_array[Slot_int].opacity = topacity;
	  self.slot_array[Slot_int].id = tid;
	  
	 end 
	--]SINGEL SLOT FUNCTIONS  
	
	--[MULTI SLOT FUNCTIONS
	function self.setSlotA_Opacity(opacity_int)
        console.log("Setting Slot opacity to"..opacity_int);
		for i=1,16 do
			self.setSlot_Opacity(i,opacity_int);
		end
	end
	
	function self.setSlotA_Color(color)
        console.log("Setting Slot color to"..color);
		for i=1,16 do
			self.setSlot_Color(i,color);
		end
	end
	
	function self.setSlotA_Size(xs,ys)
        console.log("Setting Slot Size X/Y to"..xs.." "..ys);
		for i=1,16 do
			self.setSlot_Size(i,xs,ys);
		end
	end
	
	function self.setSlotA_Item(Item_ID)
        console.log("Setting Slot id to"..Item_ID);
		for i=1,16 do
			self.setSlot_Item(i,Item_ID);
		end
	end
	
	function self.setSlotA_Pos(x,y)
        console.log("Setting Slot to Position"..x.." "..y);
		for i=1,16 do
			self.setSlot_Pos(i,x,y);
		end
	end
	
	--]MULTI SLOT FUNCTIONS
	function self.render()
       console.log("Start Renderer");
	   self.init();  
	   self.Glass_Handle.clear();
	   for i=1,16,1 do
		   self.slot_array[i].render();
	   end
	   self.Glass_Handle.sync();
       console.log("Stoped Renderer");
	end
    
    function self.getVector(Slot_int)
       console.log("Get Vector from "..Slot_int);
          if Slot_int > #self.slot_array then
		       Slot_int = 1;
	      elseif Slot_int == 0 then
		    Slot_int = 1;
	  end 
        local ivec = Vector(self.slot_array[Slot_int].xpos,self.slot_array[Slot_int].ypos); 
        console.log("The Vector form "..Slot_int.." is: "..ivec.x.." "..ivec.y); 
        return ivec;
    end
    
	return self;
end
--]Slot Array Function

--[Command Line Parsing
local log = logger(900,30,"std",LOG_ACTIV);
local parse = ComParser();
--]Command Line Parsing

--[Create New Slots
local Slots = Slot_x16();
log.log("Initialised Item Slots");
--]Create NewSlots 
--[Localization functions + Vars
function Localization(Short_Lang_String_arg)
	local self=
	{
		--Global_Values
	}
	--[private values
	local lang = Short_Lang_String_arg or "EN";
	local size = 2;
	local Possible_langs = {};
	local langTable = {};
	local lang_iter = 0;
	--Will Replaced by Lang Files But Actually not!
	
	--]private values
	
	function self.addLang(Short_Lang_String,...)
	   if arg["n"] == size then     
			for i,v in ipairs(arg) do
				langTable[Short_Lang_String] = {};
				langTable[Short_Lang_String][i] = tostring(v);
				print(langTable[Short_Lang_String][i+1]);
			end
			addShortName(Short_Lang_String);  
	   else 
			error("Not Enought Args given!");
	   end
	end
	
	function addShortName(Short_Lang_String)
		if string.len( Short_Lang_String) > 1 and string.len( Short_Lang_String) < 3 then
		  Possible_langs[#Possible_langs+1] = Short_Lang_String;  
		else
			error("Your Language String is to Big or to Small");
		end
	end
	
	function self.addSlot(...)
	   if arg["n"] == #Possible_langs then 
		 local langf = {};
		 local iter = 1;
			for i,v in ipairs(arg) do
				langf[iter] = tostring(v);
				iter = iter + 1; 
			end
			for x=1,#Possible_langs do
					langTable[Possible_langs[x]][#langTable[Possible_langs[x]]+1] = langf[x];
			end
			size = size + 1;
		end
	end
	function addSlot_L(Short_Lang_String,text,Iterator)
		if Iterator == 1 then 
			langTable[Short_Lang_String] = {};
		end    
			langTable[Short_Lang_String][Iterator] = text;
	end
	
	function Possible_string()
		local pslang = "";
		for i=1,#Possible_langs do
			pslang = pslang .. Possible_langs[i] .. " ";
		end
		return pslang;
	end
	
	function Possible_lang(Lang_String)
	  for i=1,#Possible_langs,1 do
		 if Lang_String == Possible_langs[i] then
			return true
		 end
	  end
	  return false;
	end
	
	function self.getLang()
	   term.clear();
	   term.setCursorPos(1,1);
	   print("Possible Languages are: "..Possible_string().."\n\n");
	   local lng = "";
	   while true do 
		  term.setCursorPos(1,2);
		  lng = read();
		  lng = string.upper( lng );
		  if Possible_lang(lng) == true then
			lang = lng; 
			break;
		  end
	   end
	end
	
	function getLangfromFile(Short_Lang_String)
	  local filename = "lang_"..Short_Lang_String;           
	  if fs.exists("/lang/"..filename..".ptal") then
		local file = fs.open("/lang/"..filename..".ptal","r");
		local iterv = 0;
		--langTable[Short_Lang_String] = {};
		while true do
		   iterv = iterv + 1;
		   local line = file.readLine();
		   if line == nil then
			 file.close();
			 break;    
		   end 
			addSlot_L(Short_Lang_String,line,iterv);--Iterator  
		end
		if iterv > size then
		   size = iterv;
		end 
		addShortName(Short_Lang_String);
	  else
		error("Can not Load from Nil");
	  end     
	end
	
	function self.initBase()
	   getLangfromFile("EN");
	   getLangfromFile("DE");
	   getLangfromFile("FR");
	end
	
	function self.print(Lang_Number)
		   Lang_Number = Lang_Number or lang_iter;
		   if Lang_Number < 0 then
				Lang_Number = 0;
		   end
		   lang_iter = lang_iter + 1;
		   if Lang_Number <= size then     
				Lang_Number = Lang_Number + 1;
				if type(langTable[lang]) == "table" then
				   if langTable[lang][Lang_Number]  ~= "nil" and langTable[lang][Lang_Number] ~= "" and type(langTable[lang][Lang_Number]) == "string" then
					   print(langTable[lang][Lang_Number]);
				   else
					   if type(langTable["EN"][Lang_Number]) ~= "nil" then                        
							print("Need Language Parameter:".."\n"..langTable["EN"][Lang_Number]);
					   else
							print("Out of Localization Strings!");
					   end
				   end  
				else 
					print("Need Language Parameter:");
					print("\n"..langTable["EN"][Lang_Number]);
				end
		   else
			error("try to get text out of bounds");
		   end 
	end
	
	return self;
end
--]Localization functions + Vars
if not HELP then
	--[Base Init
	local lang = Localization("EN");
	lang.initBase();
	 --lang.getLang();
	--]Base Init
	log.log("Loadet Base Init");
	if isWireless then
	lang.print(0);
	log.log("Modem is Wireless");   
	else
	lang.print(1);
	log.krit("Modem Not Wireless");
	end
	lang.print(3);
    Slots.init();
    Slots.render();
    local DVec = Slots.getVector(2);
else
  log.log("Lodet Help Manual")
end 


