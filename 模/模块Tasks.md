list = {
	
-- Inner Project
	{'collaborate', '[[WP:Collaborations|合作]]'},
	{'assess', '[[Wikipedia:条目质量评级标准|评级]]'},

-- Deletion	
	{'deletion', '[[WP:删除|删除]]'},
	{'notability', '[[Wikipedia:关注度|关注度]]'},
	{'merge', '[[Wikipedia:合并和移动页面|合并]]'},
	{'split', '[[WP:分割|分割]]'},
	{'move', '[[WP:移动请求|移动]]'},
	
-- Maintain	
	{'category', '[[Wikipedia:頁面分類|分类]]'},
	{'maintain', '[[Wikipedia:維基百科維護|维护]]'},
	{'cleanup', '[[Wikipedia:清理|清理]]'},
	{'wikify', '[[Wikipedia:维基化|维基化]]'},
	{'orphans', '[[Wikipedia:孤立页面|孤立]]'},
	{'disambiguation', '[[:Category:消歧义|消歧义]]'},

-- Expand / Improve	
	{'expand', '[[Wikipedia:更优秀条目写作指南|扩充]]'},
	{'stubs', '[[Wikipedia:小作品|小作品]]'},
	{'requests', '[[Wikipedia:条目请求|条目请求]]'},
	{'copyedit', '[[Wikipedia:如何審核校對|校订]]'},
	{'npov', '[[Wikipedia:中立的观点|中立]]'},
	{'update', '[[WP:As_of|更新]]'},
	
-- Source	
	{'citations', '[[Wikipedia:列明来源|来源引用]]'},
	{'unreferenced', '[[:Category:缺少来源的条目|补充来源]]'},
	{'verify', '[[Wikipedia:可供查證|查证]]'},

-- Other requests
	{'infobox', '[[Wikipedia:格式手册/信息框|信息框]]'},
	{'photo', '[[Wikipedia:图片请求|图片]]'},
	{'map', '[[Wikipedia:地图专题|地图]]'},
	{'geocoord', '[[WP:WikiProject_Geographical_coordinates|Geographical coordinates]]'},
	
-- Reviews	
	{'fac', '[[Wikipedia:特色条目评选|特色条目评选]]'},
	{'far', '[[WP:特色条目重审|特色条目重审]]'},
	{'flc', '[[Wikipedia:特色列表评选|特色列表评选]]'},
--	{'fsc', '[[WP:Featured_sound_candidates|Featured sound candidates]]'},
	{'gan', '[[Wikipedia:優良條目評選|優良條目評選]]'},
	{'gar', '[[Wikipedia:優良條目重審|優良條目重審]]'},
	{'pr', '[[Wikipedia:同行评审|同行评审]]'},
	
}

local getArgs = require('Module:Arguments').getArgs

p = {}

local function core(name, value)
	local ret
	
	ret = '<li><b>' .. (name or '') .. '：</b>' .. (value or '') .. '</li>'
	
	return ret
end

local function tasklist(args)
--	table.insert(list, -1, args.othertext or '其它')
	local ret = ''
	
	for _, v in ipairs(list) do
		if args[v[1]] then
			ret = ret .. core(v[2], args[v[1]])	
		end
	end
	
	return ret
end

function p.main(frame)
	local args = getArgs(frame)
	navbar0 = frame:expandTemplate{ title = 'navbar', args = { args.name, plain = 'yes', style = 'float: right;'} }
	return p._main(args)
end

function p._main(args)
	table.insert(list, {'other', args.othertext or '其它'})
	
	local td1

	if args.image ~= 'off' then
			
		td1 = mw.html.create('td')
		td1
			:css('vertical-align', 'top')
			:css('width', '55px')
			:wikitext('[[File:Nuvola_apps_korganizer.svg|50px]]')
	else
		td1 = ''
	end
	
	td2text = '<div style="position: relative; left: 0px; margin-right: 0px; z-index: 15;">下方列出了<b>[[Wikipedia:社群首页/活跃任务|需要关注的任务]]：</b>'
	if args.navbar == 'yes' then
		td2text = td2text .. navbar0
	end
	td2text = td2text .. '<ul style="font-size: 100%; margin: 0px; padding: 0.3em 0px 0.3em 25px;">'
	td2text = td2text .. tasklist(args)
	td2text = td2text .. '</ul>'
	td2text = td2text .. '</div>'	
	
	local td2 = mw.html.create('td')
	td2:wikitext(td2text)
	
	local tr = mw.html.create('tr')
	tr:wikitext(tostring(td1) .. tostring(td2))
		
	local framee = mw.html.create('table')
	framee
		:css('background', 'none')
		:css('width', 'auto')
		:wikitext( tostring(tr) )
	
	return framee
end

return p