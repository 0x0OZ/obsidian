
- Microsoft supported naming scheme  
```cpp
A:
Represents an 8-bit character set with ANSI encoding  
W:
Represents a Unicode encoding  
Ex:
Provides extended functionality or in/out parameters to the API call
```
- windows has two implementations for API calls:
-  P/Invoke: designed by micrsoft for .NET and Powershell to import DLLs
- Windows header file: regular header files -> `windows.h`

- **C** API call example
```cpp
HWND CreateWindowExA(
  [in]           DWORD     dwExStyle, // Optional windows styles
  [in, optional] LPCSTR    lpClassName, // Windows class
  [in, optional] LPCSTR    lpWindowName, // Windows text
  [in]           DWORD     dwStyle, // Windows style
  [in]           int       X, // X position
  [in]           int       Y, // Y position
  [in]           int       nWidth, // Width size
  [in]           int       nHeight, // Height size
  [in, optional] HWND      hWndParent, // Parent windows
  [in, optional] HMENU     hMenu, // Menu
  [in, optional] HINSTANCE hInstance, // Instance handle
  [in, optional] LPVOID    lpParam // Additional application data
);
```
- the API call example implementation
```cpp
WND hwnd = CreateWindowExA(
	0, 
	CLASS_NAME, 
	L"Hello THM!", 
	WS_OVERLAPPEDWINDOW, 
	CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, CW_USEDEFAULT, 
	NULL, 
	NULL, 
	hInstance, 
	NULL
	);
```
- an application to run the API call
```cpp
BOOL Create(
        PCWSTR lpWindowName,
        DWORD dwStyle,
        DWORD dwExStyle = 0,
        int x = CW_USEDEFAULT,
        int y = CW_USEDEFAULT,
        int nWidth = CW_USEDEFAULT,
        int nHeight = CW_USEDEFAULT,
        HWND hWndParent = 0,
        HMENU hMenu = 0
        )
    {
        WNDCLASS wc = {0};

        wc.lpfnWndProc   = DERIVED_TYPE::WindowProc;
        wc.hInstance     = GetModuleHandle(NULL);
        wc.lpszClassName = ClassName();

        RegisterClass(&wc);

        m_hwnd = CreateWindowEx(
            dwExStyle, ClassName(), lpWindowName, dwStyle, x, y,
            nWidth, nHeight, hWndParent, hMenu, GetModuleHandle(NULL), this
            );

        return (m_hwnd ? TRUE : FALSE);
    }
```

- **P/Invoke** .NET
- intPtr is the pointer to the API call in the target DLL
```csharp
class Win32 {
	[DllImport("kernel32")]
	public static extern IntPtr GetComputerNameA(StringBuilder lpBuffer, ref uint lpnSize);
}

static void Main(string[] args) {
	bool success;
	StringBuilder name = new StringBuilder(260);
	uint size = 260;
	success = GetComputerNameA(name, ref size);
	Console.WriteLine(name.ToString());
}
```
- **P/Invoke** .Powershell
```powershell
$MethodDefinition = @"
    [DllImport("kernel32")]
    public static extern IntPtr GetProcAddress(IntPtr hModule, string procName);
    [DllImport("kernel32")]
    public static extern IntPtr GetModuleHandle(string lpModuleName);
    [DllImport("kernel32")]
    public static extern bool VirtualProtect(IntPtr lpAddress, UIntPtr dwSize, uint flNewProtect, out uint lpflOldProtect);
"@;
$Kernel32 = Add-Type -MemberDefinition $MethodDefinition -Name 'Kernel32' -NameSpace 'Win32' -PassThru;
[Win32.Kernel32]::<Imported Call>()

```

- some of most used API calls in malicous softwares
```csharp
LoadLibraryA  
Maps a specified DLL  into the address space of the calling process  

GetUserNameA  
Retrieves the name of the user associated with the current thread  

GetComputerNameA  
Retrieves a NetBIOS or DNS  name of the local computer  

GetVersionExA  
Obtains information about the version of the operating system currently running  

GetModuleFileNameA  
Retrieves the fully qualified path for the file of the specified module and process  

GetStartupInfoA  
Retrieves contents of STARTUPINFO structure (window station, desktop, standard handles, and appearance of a process)  

GetModuleHandle  
Returns a module handle for the specified module if mapped into the calling process's address space  

GetProcAddress  
Returns the address of a specified exported DLL  function  

VirtualProtect  
Changes the protection on a region of memory in the virtual address space of the calling process
```