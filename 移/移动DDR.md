{{NoteTA
|G1 = IT
|1 = zh-cn:移动; zh-hk:流動; zh-tw:行動;
}}{{编修}}
'''Mobile DDR'''（也称'''MDDR'''、'''Low Power DDR'''或'''LPDDR'''）是[[DDR_SDRAM|DDR SDRAM]]的一種，專門用於[[移动设备|移動式電子產品]]，例如手机等。

[[DDR|DDR内存]]从[[DDR_SDRAM|DDR]]、[[DDR2_SDRAM|DDR2]][[DDR3_SDRAM|、DDR3]]发展到[[DDR4_SDRAM|DDR4]]，频率更高、电压更低的同时卻也讓反應時間不断变大，改变着内存子系统。而[[DDR4_SDRAM|DDR4]]最重要的使命是提高频率和带宽，每个针脚都可以提供2Gbps(256MB/s)的带宽，拥有高达4266MHz的频率，内存容量最大可达到128GB，运行电压正常可降低到1.1V~1.2V。

相对于[[DDR|DDR内存]],[[MDDR|MDDR]]具有低功耗、高可靠性的特点，目前韩国三星与美光科技（Micron Technology）掌握该项技术。[[Apple|Apple]] [[iPad|iPad]], [[Samsung_Galaxy_Tab|Samsung Galaxy Tab]] 以及 [[Motorola_Droid_X|Motorola Droid X]]都使用LPDDR<ref>[http://www.anandtech.com/show/4062/samsung-galaxy-tab-the-anandtech-review Anandtech Samsung Galaxy Tab - The AnandTech Review], December 23, 2010</ref>。

MDDR的运行电压（工作电压）相比DDR的标准电压要低，从第一代LPDDR到如今的LPDDR4，每一代LPDDR都使内部读取大小和外部传输速度加倍。其中[[DDR4_SDRAM|LPDDR4]]可提供32Gbps的带宽，输入/输出接口数据传输速度最高可达3200Mbps，电压降到了1.1V。

==LPDDR2==
一个新的[[JEDEC|JEDEC]]标准[http://www.jedec.org/user/register?destination=node/16037 JESD209-2F]定义了low-power DDR介面标准。它与DDR1或[[DDR2_SDRAM|DDR2 SDRAM]]并不兼容，但类似：
* LPDDR2-S2: 2n prefetch memory (像 DDR1)，
* LPDDR2-S4: 4n prefetch memory (像 DDR2)，或是
* LPDDR2-N: 非易失性（[[NAND|NAND]]）内存。

低功耗狀態基本相似，LPDDR，一些額外的部分陣列更新選項。
{|class="wikitable" style="text-align:center;"
|+ LPDDR2 命令编码<ref name="lpddr2">{{Citation |url=http://www.jedec.org/sites/default/files/docs/JESD209-2B.pdf |title=JEDEC Standard: Low Power Double Data Rate 2 (LPDDR2) |date=February 2010 |accessdate=2010-12-30 |publisher=JEDEC Solid State Technology Association}}</ref>
|-
! CK || CA0<br/>({{overline|RAS}}) || CA1<br/>({{overline|CAS}}) || CA2<br/>({{overline|WE}}) || CA3 || CA4 || CA5 || CA6 || CA7 || CA8 || CA9 || 操作
|-
| ↑ ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''H''' ||colspan=7 bgcolor=lightgrey| — || rowspan=2| NOP
|-
| ↓ ||colspan=10 bgcolor=lightgrey| —
|-
| ↑ ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''H''' ||colspan=5 bgcolor=lightgrey| — ||rowspan=2| 預先充電所有的 banks
|-
| ↓ ||colspan=10 bgcolor=lightgrey| —
|-
| ↑ ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''L''' ||colspan=2 bgcolor=lightgrey| — ||bgcolor=PaleTurquoise | BA2 ||bgcolor=PaleTurquoise | BA1 ||bgcolor=PaleTurquoise | BA0 ||rowspan=2| 預先充電一個 bank
|-
| ↓ ||colspan=10 bgcolor=lightgrey| —
|-
| ↑ ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''H''' || A30 || A31 || A32 ||bgcolor=PaleTurquoise | BA2 ||bgcolor=PaleTurquoise | BA1 ||bgcolor=PaleTurquoise | BA0 ||rowspan=2| Preactive<br/>(LPDDR2-N only)
|-
| ↓ || A20 || A21 || A22 || A23 || A24 || A25 || A26 || A27 || A28 || A29
|-
| ↑ ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''L''' ||colspan=6 bgcolor=lightgrey| — || rowspan=2| 突發中止
|-
| ↓ ||colspan=10 bgcolor=lightgrey| —
|-
| ↑ ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''H''' ||colspan=2 bgcolor=lightgrey| ''reserved'' || C1 || C2 ||bgcolor=PaleTurquoise | BA2 ||bgcolor=PaleTurquoise | BA1 ||bgcolor=PaleTurquoise | BA0 ||rowspan=2| 讀<br/>(AP=auto-precharge)
|-
| ↓ ||bgcolor=lightpink| AP || C3 || C4 || C5 || C6 || C7 || C8 || C9 || C10 || C11
|-
| ↑ ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''L''' ||colspan=2 bgcolor=lightgrey| ''reserved'' || C1 || C2 ||bgcolor=PaleTurquoise | BA2 ||bgcolor=PaleTurquoise | BA1 ||bgcolor=PaleTurquoise | BA0 ||rowspan=2| 寫<br/>(AP=auto-precharge)
|-
| ↓ ||bgcolor=lightpink| AP || C3 || C4 || C5 || C6 || C7 || C8 || C9 || C10 || C11
|-
| ↑ ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''H''' || R8 || R9 || R10 || R11 || R12 ||bgcolor=PaleTurquoise | BA2 ||bgcolor=PaleTurquoise | BA1 ||bgcolor=PaleTurquoise | BA0 ||rowspan=2| 激活<br/>(R0–14=Row address)
|-
| ↓ || R0 || R1 || R2 || R3 || R4 || R5 || R6 || R7 || R13 || R14
|-
| ↑ ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''H''' || A15 || A16 || A17 || A18 || A19 ||bgcolor=PaleTurquoise | BA2 ||bgcolor=PaleTurquoise | BA1 ||bgcolor=PaleTurquoise | BA0 ||rowspan=2| 激活<br/>(LPDDR2-N only)
|-
| ↓ || A5 || A6 || A7 || A8 || A9 || A10 || A11 || A12 || A13 || A14 
|-
| ↑ ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''H''' ||colspan=6 bgcolor=lightgrey| — ||rowspan=2| 更新所有的 banks<br/>(LPDDR2-Sx only)
|-
| ↓ ||colspan=10 bgcolor=lightgrey| —
|-
| ↑ ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''H''' ||bgcolor=palegreen| '''L''' ||colspan=6 bgcolor=lightgrey| — ||rowspan=2| 更新一個 bank<br/>(Round-robin addressing)
|-
| ↓ ||colspan=10 bgcolor=lightgrey| —
|-
| ↑ ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''H''' || MA0 || MA1 || MA2 || MA3 || MA4 || MA5 ||rowspan=2| Mode register read<br/>(MA0–7=Address)
|-
| ↓ || MA6 || MA7 ||colspan=8 bgcolor=lightgrey| —
|-
| ↑ ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''L''' ||bgcolor=palegreen| '''L''' || MA0 || MA1 || MA2 || MA3 || MA4 || MA5 ||rowspan=2| Mode register write<br/>(OP0–7=Data)
|-
| ↓ || MA6 || MA7 || OP0 || OP1 || OP2 || OP3 || OP4 || OP5 || OP6 || OP7
|}

列位址（Column address）C0 bit從未轉讓，並假定為零。 突發傳輸（Burst transfers）從而始終在偶數地址開始。

LPDDR2也有一個低電平有效的chip select（高時，一切都是NOP）和時鐘（clock）開啟CKE信號，操作像SDRAM。
*如果晶片被激活（active），它凍結（freeze）到位。
*如果該命令是一個NOP（CS低或CA0 - 2 = HHH），晶片空閒。
*如果該命令是一個更新命令（CA0 - 2 = LLH），晶片進入自更新狀態。
*如果該命令是一個突發終止（CA0 - 2 = HHL），晶片進入深斷電狀態。 （完全復位序列時，需要離開。）

== LPDDR3 ==
2012年5月，[[JEDEC|JEDEC]]公佈JESD209-3低功耗記憶裝置標準。比起LPDDR2，LPDDR3提供更高的數據傳輸速率、更高的頻寬與功率效率。LPDDR3可達到1600 MT/s的數據傳輸速率，並利用關鍵的新技術write-leveling、command/address training、選擇性ODT（On Die Termination）與低I/O電容。LPDDR3支援PoP封裝（package-on-package）與離散封裝兩種形式。

== LPDDR4 ==
2013年3月14日，[[JEDEC|JEDEC固態技術協會]]舉辦會議，探討未來移動裝置的新標準，如LPDDR4。

=== LPDDR4X ===
[[三星電子|三星電子]]提出最新的LPDDR4標準，与LPDDR4相同，只是通过将I / O电压降低到0.6 V而不是1.1 V来节省额外的功耗，也就是更省电。

== LPDDR5 ==
2017年，JEDEC正在研究LP-DDR5記憶裝置標準。儘管負責制定記憶體標準的JEDEC組織尚未發表LPDDR5的正式規格，但三星已經完成8Gbit LPDDR5模組原型的功能測試與驗證。

== 注釋 ==
{{reflist}}
== 外部連結 ==
* [http://www.samsung.com/global/business/semiconductor/products/dram/Products_MobileSDRAM.html Samsung]
* [https://web.archive.org/web/20080520121425/http://www.micron.com/products/dram/mobiledram Micron]
* [http://www.elpida.com/en/products/mobile.html Elpida]

{{DRAM}}
[[分類:電腦記憶體|分類:電腦記憶體]]