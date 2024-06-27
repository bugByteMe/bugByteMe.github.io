#### Pre fetch (Before sn check)

1. File R/W
Tool: FileMon
File
	CreatFile
	CreatFileEx
	ReadFile
	WriteFile

Tool: RegMon
Reg
	RegQueryValueExA
	RegOpenKeyExA
	RegCreatKey
	RegSetValue

2. Breakpoint at register code
	Set a hardware breakpoint at the memory that store the input reg code
	eg: *GetWindowTextA*, *GetText*, *WM_GETTEXT* may read the reg code to memory
	Windows message handle center : **0x77D18731** in x32

3. WM_COMMAND breakpoint (0x111)
	break at button down, menu down...
	View->Windows, select a windows and Right Click
	创建窗口WM_CREAT,读取文本框WM_GETTEXT, 110初始化对话框。

#### Post fetch (After sn check)

MessageBox (Registration failed message)
	MessageBoxA
	MessageBoxW
	MessageBoxExA
	MessageBoxExW
	MessageBoxIndirectA
	MessageBoxIndirectW

DestroyWindow (Close message, actually dialog)

Set breakpoint after a function call

