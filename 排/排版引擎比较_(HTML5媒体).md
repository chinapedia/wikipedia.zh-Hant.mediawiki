{{HTML}}
[[HTML5|HTML5]]规范定义了几个标签，允许在语义上原生包含视频和音频。下表比较了排版引擎之间对这一规范各个方面的支持。

{{浏览器引擎命名}}
{{功能成熟度表格圖例}}

==元素属性==
媒体元素允许在标签中直接设置某些属性
{| style="text-align: center; width: 95%" class="wikitable"
|-
!
! style="width: 18%;" | [[Trident_(排版引擎)|Trident]]
! style="width: 18%;" | [[Gecko_(layout_engine)|Gecko]]
! style="width: 18%;" | [[WebKit|WebKit]]
! style="width: 18%;" | [[Presto|Presto]]
|-
! colspan="5" | <code><audio></code>属性
|-
! style="text-align: left;" | <code>src</code>
| rowspan="5" {{yes|5.0}}<ref group="t" name="ie9-preview">{{citation |url=http://msdn.microsoft.com/en-us/ie/ff468705.aspx |title=Internet Explorer Platform Preview Guide for Developers |publisher=Microsoft}}</ref>
| {{yes|1.9.1}}
| rowspan="5" {{yes|525}}
| {{yes|2.5}}
|-
! style="text-align: left;" | <code>preload</code>
| {{yes|2.0}}<ref group="note" name="autobuffer"><code>preload</code>以旧名字<code>autobuffer</code>支持。</ref><ref group="g" name="moz-preload">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=548523 |title=Bug 548523 - HTML 5 media attribute 'autobuffer' has been renamed to 'preload' |publisher=Mozilla}}</ref>
| {{table-experimental}}<ref group="note" name="autobuffer"/>
|-
! style="text-align: left;" | <code>autoplay</code>
| {{yes|1.9.1}}
| rowspan="3" {{yes|2.5}}
|-
! style="text-align: left;" | <code>loop</code>
| {{yes|11.0}}<ref group="g" name="moz-loop">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=449157 |title=Bug 449157 - Implement the looping attributes in media elements |publisher=Mozilla}}</ref>
|-
! style="text-align: left;" | <code>controls</code>
| {{yes|1.9.1}}
|-
! colspan="5" | <code><video></code>属性
|-
! style="text-align: left;" | <code>src</code>
| rowspan="8" {{yes|5.0}}<ref group="t" name="ie9-preview"/>
| {{yes|1.9.1}}
| rowspan="8" {{yes|525}}
| {{yes|2.5}}
|-
! style="text-align: left;" | <code>preload</code>
| {{yes|2.0}}<ref group="note" name="autobuffer"/><ref group="g" name="moz-preload"/>
| {{table-experimental}}<ref group="note" name="autobuffer"/>
|-
! style="text-align: left;" | <code>autoplay</code>
| {{yes|1.9.1}}
| rowspan="6" {{yes|2.5}}
|-
! style="text-align: left;" | <code>loop</code>
| {{yes|11.0}}<ref group="g" name="moz-loop"/>
|-
! style="text-align: left;" | <code>controls</code>
| {{yes|1.9.1}}
|-
! style="text-align: left;" | <code>poster</code>
| {{yes|1.9.2}}<ref group="g">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=449156 |title=Bug 449156 - Implement the poster attribute for the <video> element |publisher=Mozilla}}</ref>
|-
! style="text-align: left;" | <code>width</code>
| rowspan="2" {{yes|1.9.1}}
|-
! style="text-align: left;" | <code>height</code>
|-
! colspan="5" | <code><source></code>属性
|-
! style="text-align: left;" | <code>src</code>
| rowspan="3" {{yes|5.0}}<ref group="t" name="ie9-preview"/>
| rowspan="2" {{yes|1.9.1}}
| rowspan="3" {{yes|525}}<ref group="w">{{citation |url=http://lists.whatwg.org/pipermail/whatwg-whatwg.org/2009-December/024519.html |title={{Bracket|whatwg}} Quality Values for Media Source Elements |first=Silvia |last=Pfeiffer |date=2009-12-13 |deadurl=yes |archiveurl=https://web.archive.org/web/20110719183341/http://lists.whatwg.org/pipermail/whatwg-whatwg.org/2009-December/024519.html |archivedate=2011-07-19 }}</ref>
| rowspan="3" {{yes|2.5}}
|-
! style="text-align: left;" | <code>type</code>
|-
! style="text-align: left;" | <code>media</code>
| {{yes|15.0}}<ref group="g">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=449363 |title=Bug 449363 - Support media attribute of <source> elements |publisher=Mozilla}}</ref>
|-
! colspan="5" | <code><track></code>属性
|-
! style="text-align: left;" | <code>kind</code>
| rowspan="4" {{yes|6.0}}<ref group="t">{{cite web |url=http://msdn.microsoft.com/en-us/library/ie/hh772556 |title=track element - track object (Internet Explorer) |publisher=Microsoft |deadurl=no |accessdate=12 July 2013}}</ref>
| rowspan="4" {{depends|24.0}}<ref group="g">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=629350 |title=Bug 629350 - Implement the track element |publisher=Mozilla}}</ref>
| rowspan="4" {{yes}}<ref group="w">{{cite | url =http://trac.webkit.org/wiki/April%202012%20HTML5%20Media%20Element%20%26%20WebAudio | title=April 2012 HTML5 Media Element & WebAudio – WebKit }}</ref>
| rowspan="4" {{no}}
|-
! style="text-align: left;" | <code>label</code>
|-
! style="text-align: left;" | <code>src</code>
|-
! style="text-align: left;" | <code>srclang</code>
|-
|}

==DOM属性==
与媒体元素有关的一些属性包含在[[文档对象模型|DOM]]中。
{| style="text-align: center; width: 95%" class="wikitable"
|-
!
! style="width: 18%;" | [[Trident_(排版引擎)|Trident]]
! style="width: 18%;" | [[Gecko_(layout_engine)|Gecko]]
! style="width: 18%;" | [[WebKit|WebKit]]
! style="width: 18%;" | [[Presto|Presto]]<ref group="p">{{citation |url=http://dev.opera.com/articles/view/everything-you-need-to-know-about-html5-video-and-audio/ |title= Everything you need to know about HTML5 video and audio |first=Simon |last=Pieters |date=2010-03-10 |publisher=Opera}}</ref>
|-
! colspan="5" | 错误状态
|-
! style="text-align: left;" | <code>MediaError</code>
| {{yes|5.0}}  <ref group="t" name="mediaerror-ie9">{{citation |url=http://msdn.microsoft.com/en-us/library/gg130962(v=VS.85).aspx |title=MSDN HTMLMediaError Object for Internet Explorer 9 |publisher=Microsoft}}</ref>
| {{yes|1.9.1}}
| ?
| {{yes|2.5}}
|-
! colspan="5" | 网络状态
|-
! style="text-align: left;" | <code>src</code>
| rowspan="7" {{yes|5.0}}<ref group="t" name="ie9-preview"/>
| rowspan="3" {{yes|1.9.1}}
| rowspan="6" | ?
| rowspan="3" {{yes|2.5}}
|-
! style="text-align: left;" | <code>currentSrc</code>
|-
! style="text-align: left;" | <code>networkState</code>
|-
! style="text-align: left;" | <code>preload</code>
| {{yes|2.0}}<ref group="note" name="autobuffer" /><ref group="g" name="moz-preload" />
| rowspan="2" {{no}}
|-
! style="text-align: left;" | <code>buffered</code>
| {{yes|2.0}}<ref group="g">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=462957 |title=Bug 462957 - Implement nsIDOMHTMLMediaElement::GetBuffered() |publisher=Mozilla}}</ref>
|-
! style="text-align: left;" | <code>load()</code>
| rowspan="2" {{yes|1.9.1}}
| rowspan="2" {{yes|2.5}}
|-
! style="text-align: left;" | <code>canPlayType()</code>
| {{yes|533}}<ref group="w">{{citation |url=https://bugs.webkit.org/show_bug.cgi?id=24364 |title= Bug 24364 -  Add HTMLMediaElement canPlayType method |publisher=WebKit}}</ref>
|-
! colspan="5" | 就绪状态
|-
! style="text-align: left;" | <code>readyState</code>
| rowspan="2" {{yes|5.0}}<ref group="t" name="ie9-preview"/>
| rowspan="2" {{yes|1.9.1}}
| rowspan="2" | ?
| rowspan="2" {{yes|2.5}}
|-
! style="text-align: left;" | <code>seeking</code>
|-
! colspan="5" | 回放状态
|-
! style="text-align: left;" | <code>currentTime</code>
| rowspan="13" {{yes|5.0}}<ref group="t" name="ie9-preview"/>
| {{yes|1.9.1}}
| rowspan="13" | ?
| rowspan="4" {{yes|2.5}}
|-
! style="text-align: left;" | <code>startTime</code>
| {{no}}
|-
! style="text-align: left;" | <code>duration</code>
| rowspan="2" {{yes|1.9.1}}
|-
! style="text-align: left;" | <code>paused</code>
|-
! style="text-align: left;" | <code>defaultPlaybackRate</code>
| rowspan="2" {{yes|20.0}}<ref group="g">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=495040 |title=Bug 495040 - Implement playbackRate |publisher=Mozilla}}</ref>
| rowspan="4" {{no}}
|-
! style="text-align: left;" | <code>playbackRate</code>
|-
! style="text-align: left;" | <code>played</code>
| {{yes|15.0}}<ref group="g">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=462959 |title=Bug 462959 - Implement nsIDOMHTMLMediaElement::GetPlayed() |publisher=Mozilla}}</ref>
|-
! style="text-align: left;" | <code>seekable</code>
| {{yes|8.0}}<ref group="g">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=462960 |title=Bug 462960 - Implement nsIDOMHTMLMediaElement::GetSeekable() |publisher=Mozilla}}</ref>
|-
! style="text-align: left;" | <code>ended</code>
| rowspan="2" {{yes|1.9.1}}
| rowspan="5" {{yes|2.5}}
|-
! style="text-align: left;" | <code>autoplay</code>
|-
! style="text-align: left;" | <code>loop</code>
| {{yes|11.0}}<ref group="g" name="moz-loop" />
|-
! style="text-align: left;" | <code>play()</code>
| rowspan="2" {{yes|1.9.1}}
|-
! style="text-align: left;" | <code>pause()</code>
|-
! colspan="5" | 控制
|-
! style="text-align: left;" | <code>controls</code>
| rowspan="3" {{yes|5.0}}<ref group="t" name="ie9-preview"/>
| rowspan="3" {{yes|1.9.1}}
| rowspan="3" | ?
| rowspan="3" {{yes|2.5}}
|-
! style="text-align: left;" | <code>volume</code></code>
|-
! style="text-align: left;" | <code>muted</code>
|-
|}

==DOM事件==
媒体元素引入新的事件处理仅适用于那些元素的情况，如暂停/恢复。
{| style="text-align: center; width: 95%" class="wikitable"
|-
!
! style="width: 18%;" | [[Trident_(排版引擎)|Trident]]
! style="width: 18%;" | [[Gecko_(layout_engine)|Gecko]]<ref group="g">{{citation |url=https://developer.mozilla.org/En/Using_audio_and_video_in_Firefox#Media_events |title=Using audio and video in Firefox - Media events |publisher=Mozilla |access-date=2016-02-06 |archive-url=https://web.archive.org/web/20120508051152/https://developer.mozilla.org/En/Using_audio_and_video_in_Firefox#Media_events |archive-date=2012-05-08 |dead-url=yes }}</ref>
! style="width: 18%;" | [[WebKit|WebKit]]
! style="width: 18%;" | [[Presto|Presto]]
|-
! style="text-align: left;" | <code>loadstart</code>
| rowspan="22" {{yes|5.0}}<ref group="t" name="ie-msdn-video-element">{{cite web |url=http://msdn.microsoft.com/en-us/library/ff975073 |publisher=Microsoft |title=video object |deadurl=no |accessdate=12 July 2013}}</ref>
| rowspan="2" {{yes|1.9.1}}
| rowspan="22" | ?
| rowspan="22" | ?
|-
! style="text-align: left;" | <code>progress</code>
|-
! style="text-align: left;" | <code>suspend</code>
| {{yes|1.9.2}}
|-
! style="text-align: left;" | <code>abort</code>
| rowspan="3" {{yes|1.9.1}}
|-
! style="text-align: left;" | <code>error</code>
|-
! style="text-align: left;" | <code>emptied</code>
|-
! style="text-align: left;" | <code>stalled</code>
| {{yes|8.0}}<ref group="g">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=481082 |title=Bug 481082 - Video controls should listen for |stalled| event |publisher=Mozilla}}</ref>
|-
! style="text-align: left;" | <code>play</code>
| rowspan="5" {{yes|1.9.1}}
|-
! style="text-align: left;" | <code>pause</code>
|-
! style="text-align: left;" | <code>loadedmetadata</code>
|-
! style="text-align: left;" | <code>loadeddata</code>
|-
! style="text-align: left;" | <code>waiting</code>
|-
! style="text-align: left;" | <code>playing</code>
| {{no}}
|-
! style="text-align: left;" | <code>canplay</code>
| rowspan="9" {{yes|1.9.1}}
|-
! style="text-align: left;" | <code>canplaythrough</code>
|-
! style="text-align: left;" | <code>seeking</code>
|-
! style="text-align: left;" | <code>seeked</code>
|-
! style="text-align: left;" | <code>timeupdate</code>
|-
! style="text-align: left;" | <code>ended</code>
|-
! style="text-align: left;" | <code>ratechange</code>
|-
! style="text-align: left;" | <code>durationchange</code>
|-
! style="text-align: left;" | <code>volumechange</code>
|-
|}

==视频格式支持==
{{main|HTML5视频}}
{| style="text-align: center; width: 95%" class="wikitable"
|-
!
! style="width: 18%;" | [[Trident_(排版引擎)|Trident]]
! style="width: 18%;" | [[Gecko_(layout_engine)|Gecko]]
! style="width: 18%;" | [[WebKit|WebKit]]
! style="width: 18%;" | [[Presto|Presto]]
|-
! style="text-align: left;" | [[Ogg|Ogg]] [[Theora|Theora]]
| {{depends|手动安装}}{{#tag:ref|Google为多媒体应用库发布了一个WebM组件以允许WebM文件在IE9中通过标准HTML5<video>标签播放<ref group="t" name="googleplugin">{{citation |url=http://blog.chromium.org/2011/01/more-about-chrome-html-video-codec.html |title=More about the Chrome HTML Video Codec Change |date=2011-01-14 |first=Mike |last=Jazayeri |publisher=Google}}</ref>。[[Xiph.org基金会|Xiph.org]]分发了[[OpenCodecs|OpenCodecs]]包，其修正了Google的基于[[DirectShow|DirectShow]]的VP8解码器。[[VLC多媒体播放器|VLC多媒体播放器]]附带有“网页插件”，从而使VLC从<code><video></code>与<code><audio></code>标签中播放多媒体，让其支持所有VLC支持的格式。|group=note|name=ie-codecs}}
| {{yes|1.9.1}}<ref group="g" name="gecko191-media">{{citation |title=Media formats supported by the audio and video elements |date=2010-01-28 |last=Shepherd |first=Eric |publisher=Mozilla |url=https://developer.mozilla.org/En/Media_formats_supported_by_the_audio_and_video_elements |accessdate=2009-10-11 |deadurl=yes |archiveurl=https://web.archive.org/web/20120504080457/https://developer.mozilla.org/En/Media_formats_supported_by_the_audio_and_video_elements |archivedate=2012年5月4日 |df= }}</ref>
| {{depends}}{{#tag:ref|在Mac OS X下WebKit支持[[QuickTime|QuickTime]]<ref group="w">{{citation |url=http://webkit.org/blog/140/html5-media-support/ |title=HTML5 Media Support |publisher=WebKit |first=Antti |last=Koivisto |date=2007-11-12}}</ref>，默认情况下，其支持H.264、MP3、AAC和WAV PCM格式，如果安装了XiphQT等第三方编解码器则会额外支持Ogg Theora与Vorbis格式。Google Chrome支持Theora、Vorbis、WebM与MP3格式<ref group="w">{{citation |url=http://src.chromium.org/viewvc/chrome/trunk/src/net/base/mime_util.cc?view=markup#vc_markup |title=Look for "GOOGLE_CHROME_BUILD"}}</ref>。Chromium可以被编译以支持任意[[FFmpeg|FFmpeg]]支持的格式，并可以选择是否支持诸如H.264和MP3的专利格式<ref group="w">{{citation |url=http://lists.whatwg.org/pipermail/whatwg-whatwg.org/2009-June/020035.html |title={{Bracket|whatwg}} Google's use of FFmpeg in Chromium and Chrome Was: Re: MPEG-1 subset proposal for HTML5 video codec |publisher=Google |first=Chris |last=DiBona |date=2009-06-01 |deadurl=yes |archiveurl=https://web.archive.org/web/20110719183325/http://lists.whatwg.org/pipermail/whatwg-whatwg.org/2009-June/020035.html |archivedate=2011-07-19 }}</ref>。[[MorphOS|MorphOS]]上的[[Origyn网页浏览器|Origyn网页浏览器]]也采用[[FFmpeg|FFmpeg]]播放HTML5媒体内容。<ref group="w">{{citation |url=http://fabportnawak.free.fr/owb/ |title=Origyn Web Browser for MorphOS |publisher=Fabian Coeurjoly |accessdate=2010-01-04}}</ref><ref group="w">{{citation |url=http://www.osnews.com/story/22971/Origyn_Web_Browser_1_7_Supports_HTML5_Media_More |title=Origyn Web Browser 1.7 Supports HTML5 Media, More |publisher=OSNews |first=Thom |last=Holwerda |date=2010-03-08 |accessdate=2010-03-08}}</ref>|group=note|name=webkit-codecs}}
| {{yes|2.5}}
|-
! style="text-align: left;" | [[H.264|H.264]]
| {{yes|5.0}}<ref group="t">{{citation |url=http://technologizer.com/2010/03/16/ie9-platform-preview/ |title=Microsoft Previews the Revamped Internet Explorer 9 Platform |first=Harry |last=McCracken |date=2010-03-16 |publisher=Technologizer}}</ref>
| {{nightly|33.0}}{{#tag:ref|via [[OpenH264|OpenH264]]}}<ref group="g">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=799318 |title=Bug 799318 - [meta] Support H.264/AAC/MP3 video/audio playback on desktop Firefox  |publisher=Mozilla}}</ref>
| {{depends|依赖（525）}}<ref group="note" name="webkit-codecs" /><ref group="w">{{citation |url=http://blog.chromium.org/2011/01/html-video-codec-support-in-chrome.html |title=HTML Video Codec Support in Chrome | accessdate=2010-01-22}}</ref>
| {{depends}}{{#tag:ref|在Linux与FreeBSD上，Presto 2.5使用[[GStreamer|GStreamer]]库的系统版本，并且可以播放任何GStreamer支持的格式（包括H.264、MP3、AAC等, 前提是已安装解码器） 。在其他平台上，它只支持Ogg Theora格式的视频和Ogg Vorbis与WAVE PCM的音频。<ref group="p">{{citation|url=http://my.opera.com/core/blog/2009/12/31/re-introducing-video |title=(re-)Introducing <video> |publisher=Opera |first=Philip |last=Jägenstedt |date=2009-12-31 |archiveurl=https://web.archive.org/web/20100104205648/http://my.opera.com/core/blog/2009/12/31/re-introducing-video |archivedate=2010-01-04 |deadurl=yes }}</ref>|group=note|name=opera-av}}
|-
! style="text-align: left;" | [[WebM|WebM]] [[VP8|VP8]]
| {{depends|手动安装}}<ref group="note" name="ie-codecs" />
| {{yes|2.0}}<ref group="g">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=566243 |title=Bug 566243 - Merge mozilla-webmedia repository to mozilla-central |publisher=Mozilla}}</ref><ref group="g">{{citation |url=http://nightly.mozilla.org/webm/ |title=Firefox WebM Builds |publisher=Mozilla}}</ref>
| {{depends|依赖（534）}}<ref group="w">{{citation |url=http://blog.chromium.org/2010/05/webm-and-vp8-land-in-chromium.html |title=WebM and VP8 land in Chromium |date=2010-05-19 |first=Jim |last=Bankoski |publisher=Google}}</ref>
| {{yes|2.6.30}}<ref group="p">{{citation|url=http://labs.opera.com/news/2010/05/19/ |title=Welcome, WebM <video>! |first=Håkon Wium |last=Lie |date=2010-05-19 |publisher=Opera |deadurl=yes |archiveurl=https://web.archive.org/web/20110321150357/http://labs.opera.com/news/2010/05/19/ |archivedate=2011-03-21 }}</ref><ref group="p">{{citation |url=http://dev.opera.com/articles/view/opera-supports-webm-video/ |title=Opera supports the WebM video format |date=2010-05-19 |first=Chris |last=Mills |publisher=Opera}}</ref><ref group="p">{{citation|url=http://my.opera.com/desktopteam/blog/2010/07/01/opera-10-60-goes-final |title=Opera 10.60 goes final |first=Huib |last=Kleinhout |publisher=Opera |date=2010-07-01 |deadurl=yes |archiveurl=https://web.archive.org/web/20100702152315/http://my.opera.com/desktopteam/blog/2010/07/01/opera-10-60-goes-final |archivedate=2010-07-02 }}</ref>
|-
|}

==音频格式支持==
{{main|HTML5音频}}
{| style="text-align: center; width: 95%" class="wikitable"
|-
!
! style="width: 18%;" | [[Trident_(排版引擎)|Trident]]
! style="width: 18%;" | [[Gecko_(layout_engine)|Gecko]]
! style="width: 18%;" | [[WebKit|WebKit]]
! style="width: 18%;" | [[Presto|Presto]]
|-
! style="text-align: left;" | Ogg [[Vorbis|Vorbis]]
| rowspan="2" {{depends|手动安装}}<ref group="note" name="ie-codecs" />
| rowspan="2" {{yes|1.9.1}}<ref group="g" name="gecko191-media" />
| {{depends}}<ref group="note" name="webkit-codecs" />
| {{yes|2.5}}
|-
! style="text-align: left;" | [[WAV|WAV]] [[脉冲编码调制|PCM]]
| rowspan="3" {{yes|525}}<ref group="note" name="webkit-codecs" />
| {{yes|2.0}}
|-
! style="text-align: left;" | [[MP3|MP3]]
| rowspan="2" {{yes|5.0}}<ref group="t">{{citation |url=http://www.techradar.com/news/internet/web/microsoft-steps-towards-faster-internet-explorer-9-677263 |title=Microsoft previews Internet Explorer 9 |first=Mary |last=Branscombe |date=2010-03-16 |publisher=TechRadar UK |deadurl=yes |archiveurl=https://web.archive.org/web/20100322191541/http://www.techradar.com/news/internet/web/microsoft-steps-towards-faster-internet-explorer-9-677263 |archivedate=2010-03-22 }}</ref>
| {{no}}<ref group="g">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=562730 |title=Bug 562730 - Reproducing Mp3 files with html5   |publisher=Mozilla}}</ref>
| rowspan="2" {{depends}}<ref group="note" name="opera-av"/>
|-
! style="text-align: left;" | [[Advanced_Audio_Coding|AAC]]
| {{no}}
|-
! style="text-align: left;" | [[Speex|Speex]]
| rowspan="1" {{depends|手动安装}}<ref group="note" name="ie-codecs" />
| {{no}}<ref group="g">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=476752 |title=Bug 476752 - support the speex voice codec in <audio> and <video> elements |publisher=Mozilla}}</ref>
| {{depends}}<ref group="note" name="webkit-codecs" />
| {{no}}
|-
! style="text-align: left;" | [[Opus_codec|Opus]]
| IE 12 ?? <ref group="g">{{citation |url=http://blogs.skype.com/2014/10/27/bringing-interoperable-real-time-communications-to-the-web/ |title=Bringing Interoperable Real-Time Communications to the Web |publisher=Skype}}</ref>
| {{yes|15.0}}<ref group="g">{{citation |url=https://bugzilla.mozilla.org/show_bug.cgi?id=674225 |title=Bug 674225 - support the Opus voice codec in <audio> and <video> elements |publisher=Mozilla}}</ref>
| {{depends}}<ref group="note" name="webkit-codecs" />
| {{no}}
|-
|}

==注释==
{{Reflist | group=note}}

==参考==
{{reflist}}
===Trident参考===
{{Reflist | group=t}}

===Gecko参考===
{{Reflist | group=g}}

===WebKit参考===
{{Reflist | group=w}}

===Presto参考===
{{Reflist | group=p}}

== 外部链接 ==
* [https://web.archive.org/web/20100420024047/http://www.whatwg.org/specs/web-apps/current-work/multipage/video.html HTML5 - 视频元素]

{{-}}
{{Layout engines}}

[[Category:HTML5|Category:HTML5]]
[[Category:排版引擎比较|Category:排版引擎比较]]