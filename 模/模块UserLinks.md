--------------------------------------------------------------------------------
--                                 UserLinks                                  --
-- This module creates a list of links about a given user. It can be used on  --
-- its own or from a template. See the /doc page for more documentation.      --
--------------------------------------------------------------------------------

-- Require necessary modules
local yesno = require('Module:Yesno')

-- Lazily initialise modules that we might or might not need
local mExtra -- [[Module:UserLinks/extra|Module:UserLinks/extra]]
local mArguments -- [[Module:Arguments|Module:Arguments]]
local mToolbar -- [[Module:Toolbar|Module:Toolbar]]
local mCategoryHandler -- [[Module:Category_handler|Module:Category handler]]
local mTableTools -- [[Module:TableTools|Module:TableTools]]
local interwikiTable -- [[Module:InterwikiTable|Module:InterwikiTable]], loaded with mw.loadData

-- Load shared helper functions
local mShared = require('Module:UserLinks/shared')
local raiseError = mShared.raiseError
local maybeLoadModule = mShared.maybeLoadModule
local makeWikitextError = mShared.makeWikitextError
local makeWikilink = mShared.makeWikilink
local makeUrlLink = mShared.makeUrlLink
local makeFullUrlLink = mShared.makeFullUrlLink
local message = mShared.message

local p = {}

--------------------------------------------------------------------------------
-- Link table
--------------------------------------------------------------------------------

function p.getLinks(snippets)
	--[=[
	-- Get a table of links that can be indexed with link codes. The table
	-- returned is blank, but links are added to it on demand when it is
	-- indexed. This is made possible by the metatable and by the various link
	-- functions, some of which are defined here, and some of which are defined
	-- at [[Module:UserLinks/extra|Module:UserLinks/extra]].
	--]=]
	local links, linkFunctions = {}, {}

	----------------------------------------------------------------------------
	-- Link functions
	--
	-- The following functions make the links from the link codes and the user
	-- data snippets. New link functions should be added below the existing
	-- functions.
	----------------------------------------------------------------------------

	function linkFunctions.u(snippets)
		-- User page
		return makeWikilink(
			snippets.interwiki,
			2,
			snippets.username,
			snippets.username
		)
	end

	function linkFunctions.t(snippets)
		-- User talk page
		return makeWikilink(
			snippets.interwiki,
			3,
			snippets.username,
			message('display-talk')
		)
	end

	function linkFunctions.c(snippets)
		-- Contributions
		return makeWikilink(
			snippets.interwiki,
			-1,
			'Contribs/' .. snippets.username,
			message('display-contributions')
		)
	end

	function linkFunctions.ct(snippets)
		-- Edit count
		return makeUrlLink(
			{
				host = 'xtools.wmflabs.org',
				path = '/ec/',
				query = {
					username = snippets.username,
					project = snippets.toolLang .. '.' .. snippets.projectLong .. '.org'
				}
			},
			message('display-count')
		)
	end

	function linkFunctions.m(snippets)
		-- Page moves
		return makeWikilink(
			snippets.interwiki,
			-1,
			'Log/move/' .. snippets.username,
			message('display-moves')
		)
	end

	function linkFunctions.l(snippets)
		-- Logs
		return makeWikilink(
			snippets.interwiki,
			-1,
			'Log/' .. snippets.username,
			message('display-logs')
		)
	end

	function linkFunctions.ae(snippets)
		-- Automated edits (and non-automated contributions).
		return makeUrlLink(
			{
				host = 'xtools.wmflabs.org',
				path = '/autoedits/',
				query = {
					username = snippets.username,
					project = snippets.toolLang .. '.' .. snippets.projectLong .. '.org'
				}
			},
			message('display-autoedits')
		)
	end

	function linkFunctions.bl(snippets)
		-- Block log
		return makeFullUrlLink(
			snippets.interwiki,
			-1,
			'Log/block',
			{page = 'User:' .. snippets.username},
			message('display-blocklog')
		)
	end

	function linkFunctions.bls(snippets)
		-- Blocks
		return makeWikilink(
			snippets.interwiki,
			-1,
			'Log/block/' .. snippets.username,
			message('display-blocks')
		)
	end

	function linkFunctions.bu(snippets)
		-- Block user
		return makeWikilink(
			snippets.interwiki,
			-1,
			'Block/' .. snippets.username,
			message('display-blockuser')
		)
	end

	function linkFunctions.ca(snippets)
		-- Central auth
		return makeWikilink(
			snippets.interwiki,
			-1,
			'CentralAuth/' .. snippets.username,
			message('display-centralauth')
		)
	end

	function linkFunctions.dc(snippets)
		-- Deleted contribs
		return makeWikilink(
			snippets.interwiki,
			-1,
			'DeletedContributions/' .. snippets.username,
			message('display-deletedcontributions')
		)
	end

	function linkFunctions.e(snippets)
		-- Email
		return makeWikilink(
			snippets.interwiki,
			-1,
			'EmailUser/' .. snippets.username,
			message('display-email')
		)
	end

	function linkFunctions.es(snippets)
		-- Edit summaries
		return makeUrlLink(
			{
				host = 'xtools.wmflabs.org',
				path = '/editsummary/',
				query = {
					username = snippets.username,
					project = snippets.toolLang .. '.' .. snippets.projectLong .. '.org'
				}
			},
			message('display-editsummaries')
		)
	end

	function linkFunctions.del(snippets)
		-- Deletions
		return makeWikilink(
			snippets.interwiki,
			-1,
			'Log/delete/' .. snippets.username,
			message('display-deletions')
		)
	end

	function linkFunctions.lu(snippets)
		-- List user
		return makeFullUrlLink(
			snippets.interwiki,
			-1,
			'ListUsers',
			{limit = 1, username = snippets.username},
			message('display-listuser')
		)
	end

	function linkFunctions.sul(snippets)
		-- SUL
		return makeWikilink(
			nil,
			nil,
			'sulutil:' .. snippets.username,
			message('display-sul')
		)
	end

	function linkFunctions.tl(snippets)
		-- Target logs
		return makeFullUrlLink(
			snippets.interwiki,
			-1,
			'Log',
			{page = mw.site.namespaces[2].name .. ':' .. snippets.username},
			message('display-targetlogs')
		)
	end

	function linkFunctions.efl(snippets)
		-- Edit filter log
		return makeFullUrlLink(
			snippets.interwiki,
			-1,
			'AbuseLog',
			{wpSearchUser = snippets.username},
			message('display-abuselog')
		)
	end

	function linkFunctions.pr(snippets)
		-- Protections
		return makeWikilink(
			snippets.interwiki,
			-1,
			'Log/protect/' .. snippets.username,
			message('display-protections')
		)
	end

	function linkFunctions.rl(snippets)
		-- User rights
		return makeWikilink(
			snippets.interwiki,
			-1,
			'Log/rights/' .. snippets.username,
			message('display-rights')
		)
	end

	function linkFunctions.ren(snippets)
		-- Renames
		return makeWikilink(
			snippets.interwiki,
			-1,
			'Log/renameuser/' .. snippets.username,
			message('display-renames')
		)
	end

	function linkFunctions.rfa(snippets)
		-- Requests for adminship
		return makeWikilink(
			nil,
			-1,
			'PrefixIndex/' .. message('page-rfa') .. '/' .. snippets.username,
			message('display-rfa')
		)
	end

	function linkFunctions.api(snippets)
		-- API user data
		return makeUrlLink(
			{
				host = snippets.fullDomain,
				path = '/w/api.php',
				query = {
					action = 'query',
					list = 'users',
					usprop = 'groups|editcount',
					ususers = snippets.username
				}
			},
			message('display-api')
		)
	end

	function linkFunctions.up(snippets)
		-- Uploads
		return makeWikilink(
			snippets.interwiki,
			-1,
			'ListFiles/' .. snippets.username,
			message('display-uploads')
		)
	end
	
	----------------------------------------------------------------------------
	-- End of link functions
	----------------------------------------------------------------------------

	-- Define the metatable that memoizes the link functions, and fetches link
	-- functions from [[Module:UserLinks/extra|Module:UserLinks/extra]] if necessary.

	-- Lazily initialise the extraLinkFunctions table. We only want to load
	-- [[Module:UserLinks/extra|Module:UserLinks/extra]] as necessary, so it has a low transclusion
	-- count.
	local extraLinkFunctions

	-- Define functions for shared code in the metatable.
	local function validateCode(code)
		-- Checks whether code is a valid link code - i.e. checks that it is a
		-- string and that it is not the blank string. Returns the code if
		-- the check passes, and nil if not.
		if type(code) == 'string' and code ~= '' then
			return code
		else
			return nil
		end
	end

	local function getExtraLinkFunctions()
		-- Loads the table of extra link functions from the /extra module.
		-- If there is a problem with loading it, return false. We use the
		-- distinction between false and nil to record whether we have already
		-- tried to load it.
		if extraLinkFunctions ~= nil then
			return extraLinkFunctions
		end
		if mExtra == nil then
			-- If loading the module fails, maybeLoadModule returns false.
			-- Here we use the distinction between false and nil to record
			-- whether we have already tried to load the /extra module.
			mExtra = maybeLoadModule('Module:UserLinks/extra')
		end
		if type(mExtra) == 'table'
			and type(mExtra.linkFunctions) == 'table'
		then
			extraLinkFunctions = mExtra.linkFunctions
		else
			extraLinkFunctions = false
		end
		return extraLinkFunctions
	end

	local function memoizeExtraLink(code, func)
		local success, link = pcall(func, snippets)
		if success and type(link) == 'string' then
			links[code] = link
			return link
		end
		return nil
	end

	-- Define the metatable.
	setmetatable(links, {
		__index = function (t, key)
			local code = validateCode(key)
			if not code then
				raiseError(
					message('error-malformedlinkcode'),
					message('error-malformedlinkcode-section')
				)
			end
			local linkFunction = linkFunctions[code]
			local link
			if linkFunction then
				link = linkFunction(snippets)
				links[code] = link
			else
				extraLinkFunctions = getExtraLinkFunctions()
				if extraLinkFunctions then
					local extraLinkFunction = extraLinkFunctions[code]
					if type(extraLinkFunction) == 'function' then
						link = memoizeExtraLink(code, extraLinkFunction)
					end
				end
			end
			if link then
				return link
			else
				raiseError(
					message('error-invalidlinkcode', code),
					message('error-invalidlinkcode-section')
				)
			end
		end,
		__pairs = function ()
			extraLinkFunctions = getExtraLinkFunctions()
			if extraLinkFunctions then
				for code, func in pairs(extraLinkFunctions) do
					if validateCode(code) and type(func) == 'function' then
						memoizeExtraLink(code, func)
					end
				end
			end
			-- Allow built-in functions to overwrite extra functions.
			for code, func in pairs(linkFunctions) do
				local link = func(snippets)
				links[code] = link
			end
			return function (t, key)
				return next(links, key)
			end
		end
	})
	return links
end

--------------------------------------------------------------------------------
-- User data snippets
--------------------------------------------------------------------------------

function p.getSnippets(args)
	--[=[
	-- This function gets user data snippets from the arguments, and from
	-- [[Module:InterwikiTable|Module:InterwikiTable]]. The data is loaded as necessary and memoized
	-- in the snippets table for performance. 
	--
	-- Snippets default to the blank string, '', so they can be used in
	-- concatenation operations without coders having to worry about raising
	-- errors. Because of this, the local functions snippetExists and
	-- getSnippet have been written to aid people writing new snippets. These
	-- functions treat the blank string as false. It is not necessary to return
	-- the blank string from a snippet function, as nil and false values are
	-- automatically converted into the blank string by the metatable.
	--
	-- If you add a new snippet, please document it at
	-- [[Module:UserLinks#Adding_new_links|Module:UserLinks#Adding new links]].
	--]=]
	local snippets, snippetFunctions = {}, {}
	setmetatable(snippets, {
		__index = function (t, key)
			local snippetFunction = snippetFunctions[key]
			if snippetFunction then
				snippets[key] = snippetFunction() or ''
				return snippets[key]
			else
				raiseError(
					message('error-nosnippet', key),
					message('error-nosnippet-section')
				)
			end
		end
	})

	-- Define helper functions for writting the snippet functions.
	local function snippetExists(key)
		-- We have set the metatable up to make snippets default to '', so we
		-- don't have to test for false or nil.
		return snippets[key] ~= ''
	end

	local function getSnippet(key)
		local ret = snippets[key]
		if ret == '' then
			return nil
		else
			return ret
		end
	end

	-- Start snippet functions.

	function snippetFunctions.username()
		-- The username.
		local username = args.user or args.User
		return username or raiseError(
			message('error-nousername'),
			message('error-nousername-section')
		)
	end

	function snippetFunctions.usernameHtml()
		-- The username html-encoded. Spaces are encoded as pluses.
		return mw.uri.encode(snippets.username)
	end

	function snippetFunctions.project()
		-- The project name.
		-- Also does the work for snippetFunctions.interwikiTableKey, and adds
		-- the project value to snippets.lang if it is a valid language code.
		local project = args.Project or args.project
		if not project then
			return nil
		end
		local projectValidated, interwikiTableKey = p.validateProjectCode(project)
		if not projectValidated then
			if mw.language.isKnownLanguageTag(project) then
				if not snippetExists('lang') then
					snippets.lang = project
				end
			else
				raiseError(
					message('error-invalidproject', project),
					message('error-invalidproject-section')
				)
			end
		end
		snippets.interwikiTableKey = interwikiTableKey
		return project
	end

	function snippetFunctions.interwikiTableKey()
		-- The key for the project in Module:InterwikiTable.
		-- Relies on snippetFunctions.project to do the real work.
		local temp = snippets.project -- required; puts key in snippets table
		return rawget(snippets, 'interwikiTableKey')
	end

	function snippetFunctions.toolProject()
		-- The short project code for use with toolserver or labs. It is always
		-- present, even if the "project" argument is absent. The default value
		-- is the "snippet-project-default" message.
		local project = getSnippet('project')
		if not project then
			return message('snippet-project-default')
		else
			return project
		end
	end

	function snippetFunctions.projectLong()
		-- The long form of the project name, e.g. "wikipedia" or "wikibooks".
		local key = getSnippet('interwikiTableKey')
		if not key then
			return message('snippet-projectlong-default')
		end
		interwikiTable = interwikiTable or mw.loadData('Module:InterwikiTable')
		local prefixes = interwikiTable[key].iw_prefix
		-- Using prefixes[2] is a bit of a hack, but should find the long name
		-- most of the time.
		return prefixes[2] or prefixes[1] 
	end

	function snippetFunctions.lang()
		-- The language code.
		local lang = args.lang or args.Lang
		if not lang then
			return nil
		end
		if mw.language.isKnownLanguageTag(lang) then
			return lang
		else
			raiseError(
				message('error-invalidlanguage', lang),
				message('error-invalidlanguage-section')
			)
		end
	end

	function snippetFunctions.toolLang()
		-- The language code for use with toolserver or labs tools. It is always
		-- present, even if the "lang" argument is absent. The default value is
		-- the "snippet-lang-default" message. 
		return getSnippet('lang') or message('snippet-lang-default')
	end

	function snippetFunctions.interwiki()
		-- The interwiki prefix, consisting of the project and language values,
		-- separated by colons, e.g. ":wikt:es:".
		local project = getSnippet('project')
		local lang = getSnippet('lang')
		if not project and not lang then
			return nil
		end
		local ret = {}
		ret[#ret + 1] = project
		ret[#ret + 1] = lang
		return table.concat(ret, ':')
	end

	function snippetFunctions.fullDomain()
		-- The full domain name of the site, e.g. www.mediawiki.org,
		-- en.wikpedia.org, or ja.wikibooks.org.
		local fullDomain
		local lang = getSnippet('toolLang')
		local key = getSnippet('interwikiTableKey')
		if key then
			interwikiTable = interwikiTable or mw.loadData('Module:InterwikiTable')
			local domain = interwikiTable[key].domain
			local takesLangPrefix = interwikiTable[key].takes_lang_prefix
			if takesLangPrefix then
				fullDomain = lang .. '.' .. domain
			else
				fullDomain = domain
			end
		else
			fullDomain = lang .. '.wikipedia.org'
		end
		return fullDomain
	end

	-- End snippet functions. If you add a new snippet function, please
	-- document it at [[Module:UserLinks#Adding_new_links|Module:UserLinks#Adding new links]].

	return snippets
end 

function p.validateProjectCode(s)
	-- Validates a project code, by seeing whether it is present in
	-- [[Module:InterwikiTable|Module:InterwikiTable]]. If it is present, returns the code and the
	-- InterwikiTable key for the corresponding site. If not present,
	-- returns nil for both.
	interwikiTable = interwikiTable or mw.loadData('Module:InterwikiTable')
	for key, t in pairs(interwikiTable) do
		for i, prefix in ipairs(t.iw_prefix) do
			if s == prefix then
				return s, key
			end
		end
	end
	return nil, nil
end

--------------------------------------------------------------------------------
-- Main functions
--------------------------------------------------------------------------------

local function makeInvokeFunction(funcName)
	-- Makes a function that can be accessed from #invoke. This is only required
	-- for functions that need to access arguments.
	return function (frame)
		mArguments = require('Module:Arguments')
		local args = mArguments.getArgs(frame)
		return p[funcName](args)
	end
end

p.main = makeInvokeFunction('_main')

function p._main(args)
	-- The main function. This is the one called from [[Template:User-multi|Template:User-multi]],
	-- via p.main.
	local options = p.getOptions(args)
	local snippets = p.getSnippets(args)
	local codes = p.getCodes(args)
	local links = p.getLinks(snippets)
	-- Overload the built-in Lua error function to generate wikitext errors
	-- meant for end users to see. This makes things harder to debug when
	-- real errors occur, but it is the only realistic way to show wikitext
	-- errors and and still have sane code when using metatables, etc.
	local success, result = pcall(p.export, codes, links, options)
	if success then
		return result
	else
		return makeWikitextError(result, options.isDemo)
	end
end

function p.getOptions(args)
	-- Gets the options from the args table, so that we don't have to pass
	-- around the whole args table all the time.
	local options = {}
	options.isDemo = yesno(args.demo) or false
	options.toolbarStyle = yesno(args.small) and 'font-size: 90%;' or nil
	options.sup = yesno(args.sup, true)
	options.separator = args.separator
	options.span = args.span
	return options
end

function p.getCodes(args)
	-- Gets the link codes from the arguments. The codes aren't validated
	-- at this point.
	mTableTools = maybeLoadModule('Module:TableTools')
	local codes
	if mTableTools then
		codes = mTableTools.compressSparseArray(args)
	else
		codes = {}
		for i, code in ipairs(args) do
			codes[i] = code
		end
	end
	return codes
end

function p.export(codes, links, options)
	-- Make the user link.
	local userLink = links.u

	-- If we weren't passed any link codes, just return the user link.
	if #codes < 1 then
		return userLink
	end

	-- Make the toolbar.
	mToolbar = require('Module:Toolbar')
	local toolbarArgs = {}
	for i, code in ipairs(codes) do
		local link = links[code]
		toolbarArgs[#toolbarArgs + 1] = link
	end
	toolbarArgs.style = options.toolbarStyle
	toolbarArgs.separator = options.separator or 'dot'
	toolbarArgs.span = options.span
	local toolbar = mToolbar.main(toolbarArgs)

	-- Apply the sup option.
	if options.sup then
		toolbar = '<sup>' .. toolbar .. '</sup>'
	end
	
	-- If we are transcluding, add a non-breaking space, but if we are substing
	-- just use a normal space
	local space = mw.isSubsting() and ' ' or ' '
	
	return userLink .. space .. toolbar
end

--------------------------------------------------------------------------------
-- Single link function
--------------------------------------------------------------------------------

p.single = makeInvokeFunction('_single')

function p._single(args)
	-- Fetches a single link from the link table.
	local options = p.getOptions(args)
	local snippets = p.getSnippets(args)
	local links = p.getLinks(snippets)
	local code = args[1]
	local success, link = pcall(p.exportSingle, links, code)
	if success then
		return link
	else
		return makeWikitextError(link, options.isDemo)
	end
end

function p.exportSingle(links, code)
	-- If any errors occur, they will probably occur here. This function
	-- exists purely so that all the errors that will occur in p._single can
	-- be handled using a single pcall.
	if not code then
		raiseError(
			message('error-nolinkcode'),
			message('error-nolinkcode-section')
		)
	end
	return links[code]
end

--------------------------------------------------------------------------------
-- Link table
--------------------------------------------------------------------------------

function p.linktable()
	-- Returns a wikitext table of link codes, with an example link for each
	-- one. This function doesn't take any arguments, so it can be accessed
	-- directly from wiki pages without using makeInvokeFunction.
	local args = {user = 'Example'}
	local snippets = p.getSnippets(args)
	local links = p.getLinks(snippets)

	-- Assemble the codes and links in order
	local firstCodes = {'u', 't', 'c'}
	local firstLinks, firstCodesKeys = {}, {}
	for i, code in ipairs(firstCodes) do
		firstCodesKeys[code] = true
		firstLinks[#firstLinks + 1] = {code, links[code]}
	end
	local secondLinks = {}
	for code, link in pairs(links) do
		if not firstCodesKeys[code] then
			secondLinks[#secondLinks + 1] = {code, link}
		end
	end
	table.sort(secondLinks, function(t1, t2)
		return t1[1] < t2[1]
	end)
	local links = {}
	for i, t in ipairs(firstLinks) do
		links[#links + 1] = t
	end
	for i, t in ipairs(secondLinks) do
		links[#links + 1] = t
	end

	-- Output the code table in table format
	local ret = {}
	ret[#ret + 1] = '{| class="wikitable plainlinks sortable"'
	ret[#ret + 1] = '|-'
	ret[#ret + 1] = '! ' .. message('linktable-codeheader')
	ret[#ret + 1] = '! ' .. message('linktable-previewheader')
	for i, t in ipairs(links) do
		local code = t[1]
		local link = t[2]
		ret[#ret + 1] = '|-'
		ret[#ret + 1] = "| '''" .. code .. "'''" 
		ret[#ret + 1] = '| ' .. link
	end
	ret[#ret + 1] = '|}'
	return table.concat(ret, '\n')
end 

return p