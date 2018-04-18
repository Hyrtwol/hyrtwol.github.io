# Context Menu Handlers

## Links

* [Creating Shortcut Menu Handlers](http://msdn.microsoft.com/en-us/library/windows/desktop/cc144171.aspx)
* [How to Write Windows Shell Extension with .NET Languages](http://www.codeproject.com/Articles/174369/How-to-Write-Windows-Shell-Extension-with-NET-Lang)
* [Creating Context Menu Handlers](http://msdn.microsoft.com/en-us/library/bb776881%28VS.85%29.aspx)
* [Regasm.exe (Assembly Registration Tool)](http://msdn.microsoft.com/en-us/library/tzat5yw6.aspx)
* [RegisterAssembly Task](http://msdn.microsoft.com/en-us/library/dakwb8wf.aspx)

## Howto

Register:

	Regasm.exe CSShellExtContextMenuHandler.dll /codebase

Unregister:

	Regasm.exe CSShellExtContextMenuHandler.dll /unregister
