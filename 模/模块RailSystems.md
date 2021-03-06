-------------------------------------------------------------------
-- 重要：在保存编辑前，请务必跑完所有测试用例并保证没有错误输出。
-- 测试方法为：在调试控制台输入
--
--   print(p.testCase())
--
-- 如果输出为“所有用例均通过！”，说明可以保存编辑，否则请消除错误。
-------------------------------------------------------------------


local p = {
	DEFAULT_LINE_COLOR = 'cccccc',
	DEFAULT_STATION_SUFFIX = '站',
	DEFAULT_PLAIN_LINK_TEMPLATE = '[[{line_link}|{line_title}]]',
	DEFAULT_RICH_LINK_TEMPLATE = ' <span style="color:#{line_color}"><big>■</big></span> [[{line_link}|{line_title}]]',
	DEFAULT_TRAIN_TIME_TEMPLATE = '往{station_link}方向：{train_time|H:i|次日}',
	DEFAULT_DATE_ELAPSED_TEXT = '[[Category:需要去除时间判断模板的页面|Category:需要去除时间判断模板的页面]]',
	DEFAULT_LINE_DATE_ABSENT_TEXT = '规划中',
}

local function plain_replace(src, substr, repl)
	function literalize(str)
	    return str:gsub("[%(%)%.%%%+%-%*%?%[%]%^%$]", function(c) return "%" .. c end)
	end
	substr = literalize(substr)
	local rstr, _ = src:gsub(substr, repl)
	return rstr
end

local function _loadSystemData(system, raises)
	local system_data = nil
	local state
	if raises == nil then
		raises = true
	end
	if system ~= nil then
		state, system_data = pcall(mw.loadData, "Module:RailSystems/" .. system)
		if not state then
			if raises then
				error(string.format('铁道系统“%s”的数据模块不存在', system))
			else
				system_data = nil
			end
		end
	end
	return system_data
end

local function _safeExpandTemplate(frame, title, args)
	function _caller(frame, t, a)
		return frame:expandTemplate{ ['title'] = t, ['args'] = a }
	end
	
	local state, result = pcall(_caller, frame, title, args)
	if not state then
		return string.format('[[:Template:%s|:Template:%s]]', title)
	else
		return result
	end
end

local function _convBool(val, default)
	if val == nil then
		return default
	elseif type(val) == 'boolean' then
		return val
	elseif mw.text.trim(val) == '' then
		return default
	end
	return (mw.text.trim(val) == '1')
end

function p._internalStationLink(name, style, short, system_data, frame)
	local station_parts, station_data
	local station_link, station_name
	local repr = ''
	if short == nil then short = true end
	
	function p.marshalStation(station_link, station_name, style)
		if not short then
			local link_pos, _ = string.find(station_link, ' %(')
			if link_pos ~= nil then
				station_name = string.sub(station_link, 1, link_pos - 1)
			else
				station_name = station_link
			end
		end
		if style == nil or mw.text.trim(style) == '' then
			return '[['_.._station_link_.._'|' .. station_name .. ']]'
		else
			return '[['_.._station_link_.._'|<span style="' .. style .. '">' .. station_name .. '</span>]]'
		end
	end

	if type(system_data) == 'string' then
		system_data = _loadSystemData(system_data, false)
	end
	if system_data == nil then
		return nil
	end

	if system_data.stationNames[name] ~= nil then
		station_data = system_data.stationNames[name]
		if type(station_data) ~= 'table' then
			station_data = {station_data, }
		end
		for i, datum in ipairs(station_data) do
			if string.find(datum, '|', 1, true) == nil or string.find(datum, '[[', 1, true) ~= nil then
				repr = repr .. datum
			else
				station_parts = mw.text.split(datum, '|')
				station_link = station_parts[1]
				station_name = table.concat(station_parts, '|', 2)
				if frame ~= nil and string.find(station_name, '{', 1, true) ~= nil then
					station_name = frame:preprocess(table.concat(station_parts, '|', 2))
				end
				repr = repr .. p.marshalStation(station_link, station_name, style)
			end
		end
		return repr
	else
		station_link = name .. p.DEFAULT_STATION_SUFFIX
		return p.marshalStation(station_link, name, style)
	end
end

function p._internalLineColor(code, prefix, system)
	local color_prefix = ''
	local system_data = _loadSystemData(system, false)
	if system_data == nil then
		return nil
	end
	
	if prefix then
		color_prefix = '#'
	end
	if system_data.lines[code] == nil then
		return color_prefix .. p.DEFAULT_LINE_COLOR
	elseif system_data.lines[code].color == nil then
		return color_prefix .. p.DEFAULT_LINE_COLOR
	else
		return color_prefix .. system_data.lines[code].color
	end
end

function p._internalLineTitle(line_code, link, system)
	local system_data = _loadSystemData(system, false)
	local tmpl = p.DEFAULT_PLAIN_LINK_TEMPLATE
	local raw_title, link_parts, link_str, _match
	if link == nil then
		link = true
	end
	if system_data == nil then
		return nil
	end
	raw_title = system_data.lines[line_code].title
	link_parts = mw.text.split(raw_title, '|')
	
	if string.find(raw_title, '[[', 1, true) ~= nil then
		return raw_title
	end
	
	if system_data.plain_link_template ~= nil then
		tmpl = system_data.plain_link_template
	end
	if link then
		if table.getn(link_parts) == 1 then
			link_str, _match = string.gsub(string.gsub(tmpl, '\{line_link\}', link_parts[1]), '\{line_title\}', link_parts[1])
		else
			link_str, _match = string.gsub(string.gsub(tmpl, '\{line_link\}', link_parts[1]), '\{line_title\}', link_parts[2])
		end
	else
		link_str = link_parts[1]
	end
	return link_str
end

function p._internalLineRichTitle(line_code, auto_degrade, system)
	local system_data = _loadSystemData(system)
	local raw_title = system_data.lines[line_code].title
	local line_color = system_data.lines[line_code].color
	local link_parts = mw.text.split(raw_title, '|')
	local tmpl = p.DEFAULT_RICH_LINK_TEMPLATE
	local link, _match
	
	if string.find(raw_title, '[[', 1, true) ~= nil then
		return raw_title
	end
	if system_data.rich_link_template ~= nil then
		tmpl = system_data.rich_link_template
	end
	
	if line_color == nil then
		if auto_degrade then
			return p._internalLineTitle(line_code, true, system)
		else
			line_color = p.DEFAULT_LINE_COLOR
		end
	end
	
	link, _match = string.gsub(tmpl, '\{line_link\}', link_parts[1])
	link, _match = string.gsub(link, '\{line_title\}', link_parts[2])
	link, _match = string.gsub(link, '\{line_color\}', line_color)
	return link
end

function p._internalLineTerminal(line_code, side, type_, branch, system)
	local system_data = _loadSystemData(system, false)
	local side_data, conds
	
	function get_condition(cond, data)
		local c
		if type(data) == 'table' then
			c = cond[data['#field']]
			if data[c] == nil then
				return get_condition(cond, data['#default'])
			else
				return get_condition(cond, data[c])
			end
		else
			return data
		end
	end
	
	if system_data ~= nil and system_data.lines[line_code] ~= nil and system_data.lines[line_code].terminals ~= nil then
		side_data = system_data.lines[line_code].terminals[side]
		conds = { ['type'] = type_, ['branch'] = branch }
		return get_condition(conds, side_data)
	else
		return nil
	end
end

function p._internalTrainTime(line_code, dir, type_, delta, system)
	function splitTime(time_str)
		local base_parts = mw.text.split(time_str, ':')
		local parts_size = table.getn(base_parts)
		local hour = 0
		local minute = 0
		local sec = 0
		if parts_size == 1 then
			minute = tonumber(mw.text.trim(base_parts[1]))
		elseif parts_size == 2 then
			hour = tonumber(mw.text.trim(base_parts[1]))
			minute = tonumber(mw.text.trim(base_parts[2]))
		elseif parts_size == 3 then
			hour = tonumber(mw.text.trim(base_parts[1]))
			minute = tonumber(mw.text.trim(base_parts[2]))
			sec = tonumber(mw.text.trim(base_parts[3]))
		else
			error('非法的时间输入')
		end
		return hour, minute, sec
	end
	
	local system_data = _loadSystemData(system)
	local dir_info = system_data.lines[line_code].trainTime[dir]
	local bhour, bmin, bsec = splitTime(dir_info[type_])
	local dhour, dmin, dsec = splitTime(delta)
	local totalsec = (bhour * 60 + bmin) * 60 + bsec + (dhour * 60 + dmin) * 60 + dsec
	local hour = math.floor(totalsec / 3600)
	local minute = math.floor((totalsec - hour * 3600) / 60)
	local sec = totalsec - hour * 3600 - minute * 60
	return dir_info.endService, hour, minute, sec
end

function p._internalLineDateMessage(line_code, type_, reprs, options, system)
	local system_data = _loadSystemData(system)
	local frame = mw.getCurrentFrame()
	local lowest_level = 0 -- 0: everything is ok, 1: year, 2: month, 3: day
	local cur_time = frame:callParserFunction{ name = '#time', args = { 'U' } }
	local open_date, year, month, day, new_year, new_month
	local ret_str = ''
	
	for i, repr in ipairs(reprs) do
		repr = mw.text.trim(repr)
		if lowest_level < 1 and (repr == 'year') then
			lowest_level = 1
		elseif lowest_level < 2 and (repr == 'month' or repr == 'ym') then
			lowest_level = 2
		elseif  lowest_level < 3 and (repr == 'day' or repr == 'date' or repr == 'ymd') then
			lowest_level = 3
		end
	end
	
	if options == nil then options = {} end
	options.auto_hide = _convBool(options.auto_hide, false)
	options.auto_defer = _convBool(options.auto_defer, true)
	if options.cur_time ~= nil then -- for testing purpose
		cur_time = frame:callParserFunction{ name = '#time', args = { 'U', options.cur_time } }
	end
	
	if system_data.lines[line_code] == nil then return nil end
	
	open_date = system_data.lines[line_code].openDates
	if open_date == nil then open_date = {} end
	if type(open_date) == 'table' then
		if open_date[type_] ~= nil then
			open_date = open_date[type_]
		elseif open_date['#default'] ~= nil then
			open_date = open_date['#default']
		elseif lowest_level == 0 then
			for i, repr in ipairs(reprs) do ret_str = ret_str .. repr end
			return ret_str
		else
			return p.DEFAULT_LINE_DATE_ABSENT_TEXT
		end
	end

	year = tonumber(frame:callParserFunction{ name = '#time', args = { 'Y', open_date } })
	month = tonumber(frame:callParserFunction{ name = '#time', args = { 'm', open_date } })
	day = tonumber(frame:callParserFunction{ name = '#time', args = { 'd', open_date } })
	if options.auto_defer then
		if lowest_level == 1 then
			open_date = string.format('%04d-01-01', year + 1)
		elseif lowest_level == 2 then
			new_year, new_month = year, month
			if month == 12 then
				new_year = new_year + 1
				new_month = 1
			else
				new_month = new_month + 1
			end
			open_date = string.format('%04d-%02d-01', new_year, new_month)
		end
	end
	open_date = frame:callParserFunction{ name = '#time', args = { 'U', open_date } }
	
	if options.auto_hide and cur_time >= open_date then
		return p.DEFAULT_DATE_ELAPSED_TEXT
	end
	for i, repr in ipairs(reprs) do
		if repr == 'year' then
			ret_str = ret_str .. year .. '年'
		elseif repr == 'month' then
			ret_str = ret_str .. month .. '月'
		elseif repr == 'day' then
			ret_str = ret_str .. day .. '日'
		elseif repr == 'ym' then
			ret_str = ret_str .. string.format('%s年%s月', year, month)
		elseif repr == 'date' or repr == 'ymd' then
			ret_str = ret_str .. string.format('%s年%s月%s日', year, month, day)
		else
			ret_str = ret_str .. repr
		end
	end
	return ret_str
end

function p.stationLink(frame)
	local a = frame.args
	local short = _convBool(a.short, true)
	local system_data = _loadSystemData(a.system, false)
	local ret = p._internalStationLink(a.name, a.style, short, system_data, frame)
	if ret == nil then
		return _safeExpandTemplate(frame, string.format('%s stations', a.system), {line=a.link, branch=a.branch, station=a.name, state=a.state})
	else
		return ret
	end
end

function p.lineColor(frame)
	local a = frame.args
	a.prefix = _convBool(a.prefix, false)
	
	local ret = p._internalLineColor(a.name, a.prefix, a.system)
	if ret == nil then
		ret = _safeExpandTemplate(frame, string.format('%s color', a.system), {a.name})
		if a.prefix then ret = '#' .. ret end
		return ret
	else
		return ret
	end
end

function p.lineTitle(frame)
	local a = frame.args
	local link = _convBool(a.link, true)
	local rev = p._internalLineTitle(a.name, link, a.system)
	if rev == nil then
		return _safeExpandTemplate(frame, string.format('%s lines', a.system), {a.name})
	else
		return rev
	end
end

function p.lineRichTitle(frame)
	local a = frame.args
	local degrade = _convBool(a.degrade, true)
	return p._internalLineRichTitle(a.name, degrade, a.system)
end

function p.lineTerminal(frame)
	local a = frame.args
	local term = p._internalLineTerminal(a.name, a.side, a['type'], a.branch, a.system)
	if term == nil then
		term = _safeExpandTemplate(frame, string.format('S-line/%s %s/%s', a.system, a.side, a.name), { ['type']=a['type'], branch=a.branch })
		return _safeExpandTemplate(frame, string.format('%s stations', a.system), {line=a.name, branch=a.branch, station=term, state=a.state})
	else
		return p._internalStationLink(term, '', true, a.system, frame)
	end
end

function p.lineTerminalName(frame)
	local a = frame.args
	local terminal_str = p._internalLineTerminal(a.name, a.side, a['type'], a.branch, a.system)
	
	if terminal_str == nil then
		return _safeExpandTemplate(frame, string.format('S-line/%s %s/%s', a.system, a.side, a.name), { ['type']=a['type'], branch=a.branch })
	else
		return terminal_str
	end
end

function p.trainTime(frame)
	local a = frame.args
	if a['type'] == 'f' or a['type'] == 'F' then a['type'] = 'first' end
	if a['type'] == 'l' or a['type'] == 'L' then a['type'] = 'last' end
	
	local terminal, hour, minute, sec = p._internalTrainTime(a.name, a.dir, a['type'], a.delta, a.system)
	return string.format('%02d:%02d', hour, minute)
end

function p.trainDirectionTime(frame)
	local str = frame.template
	if str == nil then str = p.DEFAULT_TRAIN_TIME_TEMPLATE end
	
	local a = frame.args
	if a['type'] == 'f' or a['type'] == 'F' then a['type'] = 'first' end
	if a['type'] == 'l' or a['type'] == 'L' then a['type'] = 'last' end
	
	local terminal, hour, minute, sec = p._internalTrainTime(a.name, a.dir, a['type'], a.delta, a.system)
	
	local l = 0
	while true do
		local r, c
		l, r, c = string.find(str, '\{([^\}]+)\}', l + 1)
		if l == nil then break end
		local sparts = mw.text.split(c, '|')
		if sparts[1] == 'station_name' then
			str = plain_replace(str, '{' .. c .. '}', terminal)
		elseif sparts[1] == 'station_link' then
			str = plain_replace(str, '{' .. c .. '}', p._internalStationLink(terminal, '', true, a.system, frame))
		elseif sparts[1] == 'train_time' then
			local tomorrow_prefix = ''
			if table.getn(sparts) >= 3 then
				if hour >= 24 then 
					tomorrow_prefix = mw.text.trim(sparts[3])
					if tomorrow_prefix == '' then tomorrow_prefix = '次日' end
					hour = hour - 24
				end
			end
			local time_str = string.format('%d:%d:%d', hour, minute, sec)
			local repl_time = frame:callParserFunction{ name = '#time', args = { sparts[2], time_str } }
			str = plain_replace(str, '{' .. c .. '}', tomorrow_prefix .. repl_time)
		end
	end
	return str
end

function p.lineDateMessage(frame)
	local a = frame.args
	local name, type_, system = a.name, a['type'], a.system
	local reprs, options = {}, {}
	local preserved = { name = true, ['type'] = true, system = true }
	for k, v in pairs(a) do
		if preserved[k] == nil then
			if type(k) == 'number' then
				reprs[k] = v
			else
				options[k] = v
			end
		end
	end 
	return p._internalLineDateMessage(name, type_, reprs, options, system)
end

function p.renderStationLinks(frame)
	local a = frame:getParent().args
	local short = _convBool(a.short, true)
	local spos, epos, station_name = 1, 1, nil
	local pos = 1
	local system_data = _loadSystemData(a.system)
	local sys_args = { system=true, short=true }
	
	local text = ''
	for k, v in frame:getParent():argumentPairs() do
		if type(k) == 'number' then
			if string.len(text) > 0 then
				text = text .. '|' .. v
			else
				text = v
			end
		else
			if sys_args[k] == nil then
				if string.len(text) > 0 then
					text = text .. '|' .. k .. '=' .. v
				else
					text = k .. '=' .. v
				end
			end
		end
	end 
	text = mw.text.trim(text)
	
	local out_text = ''
	while spos ~= nil do
		spos, epos, station_name = string.find(text, '%$%$([^%$]+)%$%$', pos)
		if spos ~= nil then
			out_text = out_text .. string.sub(text, pos, spos - 1) .. p._internalStationLink(station_name, '', short, system_data, frame)
			pos = epos + 1
		else
			out_text = out_text .. string.sub(text, pos, string.len(text))
		end
	end
	return out_text
end

function p.testCase(frame)
	local frame = mw.getCurrentFrame()
	local lineTerminalNameFrame
	local link, _match
	
	function mkfp(func_name)
		local fun = p[func_name]
		function call_frame(frame)
			return fun(mw.getCurrentFrame():newChild(frame))
		end
		return call_frame
	end
	
	local fp = {
		stationLink = mkfp('stationLink'),
		lineTitle = mkfp('lineTitle'),
		lineColor = mkfp('lineColor'),
		lineTerminal = mkfp('lineTerminal'),
		lineTerminalName = mkfp('lineTerminalName'),
		trainDirectionTime = mkfp('trainDirectionTime'),
	}
	
	-- stationLink test
	if fp.stationLink{args={name='宁波站', system='UseCase'}} ~= '[[宁波站|宁波站]]' then
		error('stationLink test failed: name=宁波站.')
	end
	if fp.stationLink{args={name='寧波站', system='UseCase'}} ~= '[[宁波站|宁波站]]' then
		error('stationLink test failed: name=宁波站.')
	end
	if fp.stationLink{args={name='高桥西', system='UseCase'}} ~= '[[高桥西站_(宁波)|高桥西]]' then
		error('stationLink test failed: name=高桥西.')
	end
	if fp.stationLink{args={name='高桥西', short='0', system='UseCase'}} ~= '[[高桥西站_(宁波)|高桥西站]]' then
		error('stationLink test failed: name=高桥西, short=0.')
	end
	if fp.stationLink{args={name='泽民', system='UseCase'}} ~= '[[泽民站|泽民]]' then
		error('stationLink test failed: name=泽民.')
	end
	if fp.stationLink{args={name='内嵌模板', system='UseCase'}} ~= '[[内嵌模板站|内嵌]]' then
		error('stationLink test failed: name=内嵌模板.')
	end
	
	-- lineColor test
	if p.lineColor{args={name='1', system='UseCase'}} ~= '3180b7' then
		error('lineColor test failed: name=1.')
	end
	if p.lineColor{args={name='1', prefix='1', system='UseCase'}} ~= '#3180b7' then
		error('lineColor test failed: name=1.')
	end
	if fp.lineColor{args={name='1', system='TestComp'}} ~= 'cc0000' then
		error('lineColor compatibility test failed: name=1.')
	end
	if fp.lineColor{args={name='1', prefix='1', system='NonExist'}} ~= '#[[:Template:NonExist_color|:Template:NonExist color]]' then
		error('lineColor compatibility test failed: name=1.')
	end
	
	-- lineTitle test
	if p.lineTitle{args={name='1', system='UseCase'}} ~= "'''[[宁波轨道交通1号线|1号线]]'''" then
		error('lineTitle test failed: name=1.')
	end
	if p.lineTitle{args={name='WRL', system='UseCase'}} ~= "'''[[西鐵綫|西鐵綫]]'''" then
		error('lineTitle test failed: name=WRL.')
	end
	if p.lineTitle{args={name='WRL', link='0', system='UseCase'}} ~= "西鐵綫" then
		error('lineTitle test failed: name=WRL, link=0.')
	end
	if fp.lineTitle{args={ name='1', system='TestComp'}} ~= '[[哈尔滨地铁1号线|1号线]]' then
		error('lineTitle compatibility test failed.')
	end
	if fp.lineTitle{args={ name='1', system='NonExist'}} ~= '[[:Template:NonExist_lines|:Template:NonExist lines]]' then
		error('lineTitle non compatibility test failed.')
	end
	
	-- lineRichTitle test
	link, _match = string.gsub(p.DEFAULT_RICH_LINK_TEMPLATE, '\{line_link\}', '宁波轨道交通1号线')
	link, _match = string.gsub(link, '\{line_title\}', '1号线')
	link, _match = string.gsub(link, '\{line_color\}', '3180b7')
	if p.lineRichTitle{args={name='1', system='UseCase'}} ~= link then
		error('lineRichTitle test failed: name=1.')
	end
	-- degrade case
	if p.lineRichTitle{args={name='S1', system='UseCase'}} ~= "'''[[宁波至余慈城际铁路|余慈线]]'''" then
		error('lineRichTitle test failed: name=1.')
	end
	
	-- lineTerminal test
	-- 仅此一例，其余测试覆盖由 lineTerminalName 完成
	if fp.lineTerminal{args={name='S1', side='left', type='F', system='UseCase'}} ~= '[[宁波东站|宁波东站]]' then
		error('lineTerminal test failed: name=S1, side=left, type=F.')
	end
	if fp.lineTerminal{args={ name='1', side='left', type='s1', branch='b1', system='TestComp'}} ~= '[[S1B1|S1B1]]' then
		error('lineTerminal compatibility test failed: name=S1, side=left, type=F.')
	end
	
	-- lineTerminalName test
	if fp.lineTerminalName{args={ name='1', side='left', system='UseCase'}} ~= '高桥西' then
		error('lineTerminalName test failed: name=1, side=left.')
	end
	if fp.lineTerminalName{args={ name='S1', side='left', system='UseCase'}} ~= '宁波站' then
		error('lineTerminalName test failed: name=1, side=left.')
	end
	if fp.lineTerminalName{args={ name='S1', side='left', type='F', system='UseCase'}} ~= '宁波东站' then
		error('lineTerminalName test failed: name=1, side=left.')
	end
	if fp.lineTerminalName{args={ name='S1', side='right', system='UseCase'}} ~= '余姚' then
		error('lineTerminalName test failed: name=1, side=left.')
	end
	if fp.lineTerminalName{args={ name='S1', side='right', type='F', system='UseCase'}} ~= '马渚' then
		error('lineTerminalName test failed: name=1, side=left.')
	end
	if fp.lineTerminalName{args={ name='S1', side='right', type='F', branch='HZW', system='UseCase'}} ~= '杭州湾' then
		error('lineTerminalName test failed: name=1, side=left.')
	end
	
	-- lineTerminalName compatibility test
	if fp.lineTerminalName{args={ name='1', side='left', ['type']='s1', branch='b1', system='TestComp'}} ~= 'S1B1' then
		error('lineTerminalName compatibility test failed: type=s1, branch=b1.')
	end
	if fp.lineTerminalName{args={ name='1', side='left', ['type']='s1', system='TestComp'}} ~= 'S1DEFAULT' then
		error('lineTerminalName compatibility test failed: type=s1.')
	end
	if fp.lineTerminalName{args={ name='1', side='left', ['type']='s2', system='TestComp'}} ~= 'DEFAULT' then
		error('lineTerminalName compatibility test failed: type=s2.')
	end
	if fp.lineTerminalName{args={ name='1', side='left', system='NonExist'}} ~= '[[:Template:S-line/NonExist_left/1|:Template:S-line/NonExist left/1]]' then
		error('lineTerminalName compatibility test failed: type=NonExist.')
	end
	
	-- trainTime test
	if p.trainTime{args={name='1', dir='霞高', ['type']='L', delta='0:3', system='UseCase'}} ~= '22:06' then
		error('trainTime test failed: dir=霞高, type=L, delta=0:3.')
	end
	if p.trainTime{args={name='1', dir='高霞', ['type']='F', delta='0:4', system='UseCase'}} ~= '06:08' then
		error('trainTime test failed: dir=高霞, type=F, delta=0:4.')
	end
	
	-- trainDirectionTime test
	if fp.trainDirectionTime{args={name='1', dir='霞高', ['type']='L', delta='0:3', system='UseCase'}} ~= "往[[高桥西站_(宁波)|高桥西]]方向：22:06" then
		error('trainDirectionTime test failed: dir=霞高, type=L, delta=0:3.')
	end
	if fp.trainDirectionTime{args={name='1', dir='高霞', ['type']='F', delta='0:4', system='UseCase'}} ~= "往[[霞浦站_(宁波)|霞浦]]方向：06:08" then
		error('trainDirectionTime test failed: dir=高霞, type=F, delta=0:4.')
	end
	
	-- lineDateMessage test
	if p.lineDateMessage{args = {'在', 'year', '的', 'month', '的', 'day', '，月份', 'ym', '，日期', 'date', '开通', name='1', ['type']='2', cur_time = '2016-1-1', auto_hide=true, system='UseCase'}} ~= '在2016年的3月的19日，月份2016年3月，日期2016年3月19日开通' then
		error('lineDateMessage test failed.')
	end
	if p.lineDateMessage{args = {'year', name='1', ['type']='2', cur_time = '2016-5-19', auto_hide=true, system='UseCase'}} ~= '2016年' then
		error('lineDateMessage test failed.')
	end
	if p.lineDateMessage{args = {'year', name='1', ['type']='2', cur_time = '2017-3-19', auto_hide=true, system='UseCase'}} ~= p.DEFAULT_DATE_ELAPSED_TEXT then
		error('lineDateMessage test failed.')
	end
	if p.lineDateMessage{args = {'ym', name='1', ['type']='2', cur_time = '2016-3-30', auto_hide=true, system='UseCase'}} ~= '2016年3月' then
		error('lineDateMessage test failed.')
	end
	if p.lineDateMessage{args = {'ym', name='1', ['type']='2', cur_time = '2016-4-19', auto_hide=true, system='UseCase'}} ~= p.DEFAULT_DATE_ELAPSED_TEXT then
		error('lineDateMessage test failed.')
	end
	if p.lineDateMessage{args = {'ymd', name='1', ['type']='2', cur_time = '2016-3-18', auto_hide=true, system='UseCase'}} ~= '2016年3月19日' then
		error('lineDateMessage test failed.')
	end
	if p.lineDateMessage{args = {'ymd', name='1', ['type']='2', cur_time = '2016-3-19', auto_hide=true, system='UseCase'}} ~= p.DEFAULT_DATE_ELAPSED_TEXT then
		error('lineDateMessage test failed.')
	end
	if p.lineDateMessage{args = {'ymd', name='S1', cur_time = '2016-3-18', auto_hide=true, system='UseCase'}} ~= '2019年12月28日' then
		error('lineDateMessage test failed.')
	end
	if p.lineDateMessage{args = {'二期规划中', name='2', auto_hide=true, system='UseCase'}} ~= '二期规划中' then
		error('lineDateMessage test failed.')
	end
	if p.lineDateMessage{args = {'ymd', name='2', auto_hide=true, system='UseCase'}} ~= p.DEFAULT_LINE_DATE_ABSENT_TEXT then
		error('lineDateMessage test failed.')
	end
	
	local child_frame = frame:newChild{title='RenderStations', args={system='UseCase', '1号线从$$高桥西$$\n', '到$$霞浦$$\n'}}
	if p.renderStationLinks(child_frame:newChild{title='renderStationLinks', args={}}) ~= '1号线从[[高桥西站_(宁波)|高桥西]]\n|到[[霞浦站_(宁波)|霞浦]]' then
		error('renderStationLinks test failed: short=true.')
	end
	child_frame = frame:newChild{title='RenderStations', args={system='UseCase', short='0', '1号线从$$高桥西$$\n', '到$$霞浦$$\n'}}
	if p.renderStationLinks(child_frame:newChild{title='renderStationLinks', args={}}) ~= '1号线从[[高桥西站_(宁波)|高桥西站]]\n|到[[霞浦站_(宁波)|霞浦站]]' then
		error('renderStationLinks test failed: short=false.')
	end
	
	return '所有用例均通过！'
end

return p