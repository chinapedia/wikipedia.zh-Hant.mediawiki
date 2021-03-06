{{noteTA
|G1=IT
|1=zh-hans:编译的HTML帮助文件;zh-tw:已編譯的HTML說明檔案}}
{{Infobox file format
| name                   = 微软HTML帮助集
| icon                   =
| logo                   =
| screenshot             =
| caption                =
| extension              = .chm
| mime                   = application/vnd.ms-htmlhelp<ref>{{cite web |last=Techtonik |first=Anatoly |title=application/vnd.ms-htmlhelp |url=http://www.iana.org/assignments/media-types/application/vnd.ms-htmlhelp |date=11 April 2006 |accessdate=7 March 2012}}</ref>
| type code              =
| uniform type           =
| magic                  =
| owner                  = [[微軟公司|微軟公司]]
| released               = 1997
| latest release version = 1.4<ref>{{cite web|title=Microsoft HTML Help 1.4|url=https://msdn.microsoft.com/en-us/library/windows/desktop/ms670169(v=vs.85).aspx|website=Windows Dev Center|publisher=Microsoft|accessdate=10 January 2017}}</ref>
| latest release date    =
| genre                  =
| container for          =
| contained by           =
| extended from          =
| extended to            = {{tsl|en|Microsoft Reader|微軟閱讀器|.lit}}
| standard               =
| url                    =
}}
{{Infobox Windows component
| name                   = 微软HTML帮助集
| type                   = 幫助系統
| included_with          = [[Windows_98|Windows 98]]
| replaces               = [[Microsoft_WinHelp|Microsoft WinHelp]]
| replaced_by            = [[Microsoft_Help_2|Microsoft Help 2]]
}}
[[File:HTMLHelpV1Console.png|thumb]]
[[File:HTMLHelpV2Console.png|thumb]]
'''微软HTML幫助集'''，即'''已編譯的HTML說明檔案'''（{{lang-en|Microsoft Compiled HTML Help, '''CHM'''}}），是[[微軟|微軟]]继承早先的{{tsl|en|WinHelp}}發展的一种[[檔案格式|檔案格式]]，用来提供{{tsl|en|online help|線上幫助}}，是一种应用较广泛的文件格式。因为CHM檔案如一本書一樣，可以提供內容目錄、索引和搜尋等功能，所以也常被用来制作[[电子书|电子书]]。<ref>[http://www.comicer.com/stronghorse/software/html/uncompile.htm#CHM 格式]</ref>實際上，{{tsl|en|Microsoft Reader|微軟閱讀器}}的<code>.lit</code>就是由CHM擴充而成。

== 歷史 ==
*1996年2月，微軟宣布終止WinHelp的發展，並開始研發HTML幫助集。
*1997年8月，HTML幫助集 1.0與[[Internet_Explorer|Internet Explorer]] 4.0一起發表。
*1998年2月，HTML幫助集 1.1a與Windows 98一起發表。
*2000年1月，HTML幫助集 1.3與Windows 2000一起發表。
*2000年7月，HTML幫助集 1.32與Internet Explorer 5.5與Windows Me一起發表。
*2001年10月，HTML幫助集 1.33與Internet Explorer 6與Windows XP一起發表。
*2001年3月，微軟在[http://www.winwriters.com/index.html WritersUA]（舊稱WinWriters）研討會中，宣布下一代Microsoft Help 2.x的計畫，且仍然為HTML為主的說明格式。
*2003年1月，微軟決定不釋出Microsoft Help 2作為一般化的說明平台，並將Help 2轉入到Visual Studio Help Integration Tool中。
*2003年8月，Borland發表C# Builder，其文件是使用Microsoft Help 2格式且使用DExplore (Document Explorer)顯示。
*2005年12月，微軟發表在Visual Studio 2005上使用的Visual Studio Help Integration工具，繼續支援Microsoft Help 2。

== 檔案格式 ==
CHM是一種用[[LZX|LZX]]算法壓縮的HTML文件集，除了文件本身外，也有索引資料檔以及影像檔等，在撰寫完成後，使用HTML幫助集 Compiler（內含於[http://www.microsoft.com/downloads/details.aspx?FamilyID=00535334-c8a6-452f-9aa0-d597d16580cc&DisplayLang=en HTML幫助集 Workshop]中），編譯為一個CHM的格式檔案（此格式也可以被反編譯成原始檔案），並且跟隨應用程式或是獨立散布，應用程式可以利用內含於<code>shdocvw.dll</code>函式庫中的HTML幫助集 API來呼叫使用，目前此格式也被微軟用來散布一些獨立的開發文件（例如Silverlight 2.0 SDK中的說明檔就是CHM格式）。
由於在HTML幫助集中可以使用JavaScript來增加互動性，因此在微軟的許多說明檔中，多利用JavaScript來增加文件的可讀性（例如程式碼縮放或是導覽等）。

=== 制作CHM的工具 ===
*开源软件
**{{en}}[http://code.google.com/p/chmcreator/ chmcreator]强大的chm编辑软件，完全开源。
**{{en}}[http://xchm.sourceforge.net xCHM]
**{{en}}[http://gnochm.sourceforge.net/ GnoCHM]
*網頁或部落格轉換成chm電子書的免費工具
**{{zh-tw}}[http://www.usb20.idv.tw/ScrapbookToChmEbookConverter/ Scrapbook2Chm]{{Dead link|date=2018年9月 |bot=InternetArchiveBot |fix-attempted=yes }}（中国大陆需翻墙）
* Microsoft免費編譯chm工具
** {{en}}[http://www.microsoft.com/downloads/details.aspx?FamilyID=00535334-c8a6-452f-9aa0-d597d16580cc&DisplayLang=en HTML幫助集 Workshop and Documentation]
*Microsoft免費編譯chm教程
**{{zh-cn}}[http://iseeiseek.blog.sohu.com/134400291.html]Microsoft HTML幫助集 Workshop全图教程
*付費工具
** Microsoft Help Compiler
** Help and Manual
*付費在PDA Pocket PC上閱讀CHM電子書的工具
**[http://www.limelink.com/big5/cebook/ CeBook]
*在线制作
**{{zh-cn}}[https://web.archive.org/web/20180224061212/http://www.makechm.com/ MakeCHM]
*其它工具
**{{zh-cn}}[http://www.comicer.com/stronghorse/software/index.htm#HugeCHM HugeCHM]直接通过ITStorage接口对CHM文件进行操作，可以把海量HTML文件打包成CHM

=== 閱讀CHM的工具 ===
*跨平台
**[https://addons.mozilla.org/zh-CN/firefox/addon/chmfox/ ChmFox]
*Windows
**[[Sumatra_PDF|Sumatra PDF]]
*iOS
**{{zh-cn}}[https://itunes.apple.com/cn/app/chmplus-pro-chm-reader/id441521818?ls=1&mt=8 ChmPlus阅读器]
*Mac OS X
**{{zh-cn}}[https://itunes.apple.com/cn/app/chmplus-chm-reader/id588628901?ls=1&mt=12 ChmPlus阅读器]

== Microsoft Help 2 ==
{{lang|en|Microsoft Help 2}}（微軟幫助檔案二代）以.hxs (Microsoft Help Compiled Storage File)作为扩展名，能由Microsoft Document Explorer來瀏覽，也有一些第三方的软件，比如H2Viewer和Help Explorer Viewer支持这种格式。這種格式先后用在Microsoft Visual Studio 2002/2003/2005/2008和Office 2007中。

== 參考資料 ==
<div class="references-small">
<references />
</div>
#[http://msdn.microsoft.com/en-us/library/bb286967.aspx Visual Studio SDK - Help Authoring and Integration]
#[http://msdn.microsoft.com/en-us/library/ms670169(VS.85).aspx Microsoft HTML幫助集 1.4]

{{Microsoft development tools}}

[[Category:微軟|Category:微軟]]
[[Category:文件格式|Category:文件格式]]