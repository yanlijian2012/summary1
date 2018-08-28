#160 crackme
---
##160个CM的分类：
	NAG：25、50、100-103、130、141、145、
	KeyFile：32、34、47、76、91、96、99、139、155、156、157、160
	Name/Serial：1、2、3、4、6、7、9、13、19、21、23、24、26、28、29、33、39、40、44、45、49、54、62、64-71、73、75、78-85、93、97、106、109-111、116、122、125-128、136、137、140、143、147、148、154、158、159、
	Serial：5、8、10、11、12、14、15、16、17、18、20、22、30、31、35、36、37、51、57、58、59、60、61、63、72、74、77、88、90、104、105、108、113、115、123、129、131、132、146、153、
	CD-Check：27、134、
	Matrix(多选框矩阵)：48、114、149、
	Time：142

	Name\Serial：这种类型的一般是用Name进行一些运算，然后得到了Serial，接着Serial和自己输入的相比较。如果追码的话定位到程序最后比较处就可以了，如果写注册机的话就需要在程序取出Name的时候分析它的计算过程。
	常用断点：GetWindowText、GetDlgItemText、SendMessage(编辑框句柄,WM_GETTEXT,长度,缓冲区地址)。

	Nag：这种类型，无论是什么语言编写的，他最后总会调用windows API，可以从API断点入手，然后patch掉创建的窗口。
	常用断点：MessageBox、CreateWindow、DialogBoxParam、CreateDialogParam

	KeyFile：文件校验类型总会读文件，只有一条路走。
	常用断点：CreateFile、ReadFile、GetFileSize

	Serial：此类参考上边的Name\Serial类型，也是需要取出文本内容然后校验，简单的就是调用字符串比较函数，复杂的就是对Serial固定位或某几位运算求值，需要进行跟踪分析。

	Cd_Check：这种会获取磁盘的类型，然后在进行校验，例如磁盘名、磁盘内文件。
	常用断点：GetDriveType

	Matrix：多选框矩阵类型的，有两种校验：1、每个多选框有对应的值，根据选中的多选框，获取每个多选框的值然后校验。2、将多选框当做按钮处理，点击后会触发按钮事件，符合后才会进行下次的校验，这种校验方法大多需要按顺序点击。
	常用断点：IsDlgButtonChecked、按钮事件(不同语言按钮事件不同，通过反编译工具VB Decompiler、DeDe等查看)

##各种程序破解思路：
	VC程序：1、CreateWindow创建的窗口，向上找RegisterClass注册窗口类，这个API中有窗口的回调函数。2、DialogBoxParam创建的窗口，在参数中就有窗口回调函数。
	找到窗口回调以后一般会有比较代码，例如与0x110、0x111，0x110是WM_INITDIALOG负责初始化操作，0x111是WM_COMMAND，COMMAND消息处理，程序的事件如按钮事件就在这里边。
	
	VB程序：使用反编译工具查看反编译的代码或事件位置，我常用的工具：VB Decompiler、VBExplorer。
	Delphi程序：也是使用反编译查看代码和事件，工具：DeDeDark。
	
	MASM程序：如果没有加花指令的话这类型的是最简单了，没有编译器的附加代码，程序一目了然，又是上下浏览一下代码就能发现关键位置。因为也是调用的API，可以参考VC程序的思路。
	
	前边一部分注册机大多用C++写的，后边一部分开始学了点Python，所以用的Python写注册机练手。
	后边的破解大多用的x32dbg作为主要调试工具，顺便发一下我用的x32插件吧。
	"XLCB" ctrl+G可输入API名直接跳到对应地址
	"xAnalyzer" 显示API参数
	"x64dbg_tol" x32的插件栏about有功能提示
	"AutoCmdLine" 记住命令行 加载时自动填写
	"AttachHelper" 自动恢复DbgBreakPoint DbgUiRemoteBreakin 反反附加
	"CodeShot" x32的屏幕截图，保存在F:\龙阁破解联盟专用破解工具包\** 目录
	"APISearch"  选中API那一行右键有搜索方式，联网查看API手册
	"Highlightfish" 可以改变x32的配色和字体颜色等等，比如我习惯用黑色背景。
	这些插件在github上都有，自行下载就OK。链接：https://github.com/x64dbg/x64dbg/wiki/Plugins

##160 crackme 破解详解
###001
https://www.52pojie.cn/thread-264393-1-1.html


#ctf reverse
##nctf逆向
1. Hello,RE!
80

或许你需要去学习下IDA的使用，但是只需要学一点点就能做这题了
PS:IDA里面按R可以把奇怪的数字变成字符串

格式为flag{*****}包含flag{}提交

参考资料：
《IDA Pro 权威指南》
各种CTF比赛的逆向部分的writeup

解答：

	