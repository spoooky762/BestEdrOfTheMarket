
# <a href="https://xacone.github.io/BestEdrOfTheMarket.html"> Best EDR Of The Market (BEOTM) 🐲 </a>
<i>Little AV/EDR Evasion Lab for training & learning purposes.</i> (🏗️ under construction..)​

```
 ____            _     _____ ____  ____     ___   __   _____ _
| __ )  ___  ___| |_  | ____|  _ \|  _ \   / _ \ / _| |_   _| |__   ___
|  _ \ / _ \/ __| __| |  _| | | | | |_) | | | | | |_    | | | '_ \ / _ \
| |_) |  __/\__ \ |_  | |___| |_| |  _ <  | |_| |  _|   | | | | | |  __/
|____/_\___||___/\__| |_____|____/|_| \_\  \___/|_|     |_| |_| |_|\___|
|  \/  | __ _ _ __| | _____| |_
| |\/| |/ _` | '__| |/ / _ \ __|
| |  | | (_| | |  |   <  __/ |_           Yazidou - github.com/Xacone
|_|  |_|\__,_|_|  |_|\_\___|\__|
```

<br>BestEDROfTheMarket is a naive user-mode EDR (Endpoint Detection and Response) project, designed to serve as a testing ground for understanding and bypassing EDR's user-mode detection methods that are frequently used by these security solutions.
<br>These techniques are mainly based on a dynamic analysis of the target process state (memory, API calls, etc.), 

<a href="https://xacone.github.io/BestEdrOfTheMarket.html">Feel free to check this short article I wrote that describe the interception and analysis methods implemented by the EDR.</a>

## Defensive Techniques ⚔️​
- [x] <a href="#"> Multi-Levels API Hooking </a> <br>
- [x] <a href="#"> SSN Hooking/Crushing </a> <br>
- [x] <a href="#"> IAT Hooking </a> <br>
- [x] <a href="#"> Shellcode Injection Detection </a> <br>
- [x] <a href="#"> Reflective Module Loading Detection</a> <br>
- [x] <a href="#"> Call Stack Monitoring </a> <br>

<i>In progress</i>:
- [ ] <a href="#"> Heap Monitoring </a> <br>
- [ ] <a href="#"> ROP Mitigation </a> <br>
- [ ] <a href="#"> AMSI Patching Mitigation </a> <br>
- [ ] <a href="#"> ETW Patching Mitigation </a> <br>
<br>
<!--<a href="#"> Performance brief </a> <br>-->

<img src="Assets/beotmgif1.gif">

## Usage 📜
```
        Usage: BestEdrOfTheMarket.exe [args]

                 /help Shows this help message and quit
                 /v Verbosity                 
                 /iat IAT hooking
                 /stack Threads call stack monitoring
                 /nt Inline Nt-level hooking
                 /k32 Inline Kernel32/Kernelbase hooking
                 /ssn SSN crushing
```
```
BestEdrOfTheMarket.exe /stack /v /k32
BestEdrOfTheMarket.exe /stack /nt
BestEdrOfTheMarket.exe /iat
```

## Structure & Config files ⚙️
```
📁 BestEdrOfTheMarket/
    📄 BestEdrOfTheMarket.exe
    📁 DLLs/
        📄 Kernel32.dll
        📄 ntdll.dll
        📄 iat.dll
    📝 TrigerringFunctions.json
    📝 YaroRules.json
    📄 jsoncpp.dll
```

<b>YaroRules.json: </b>Contains a json array filled with the patterns you would like to be identified while monitoring threads call stacks.
```json
{
	"Patterns": [
		"d2 65 48 8b 52 60 48 8b 52 18 48 8b 52 20 48 8b 72 50 48",
		"49 be 77 73 32 5f 33 32 00 00",
                "..."
    ]
}
```

<b>TrigerringFunctions.json: </b>Describes the functions that are already hooked or/and to hook:


```json
{
  "DLLBasedHooking": {
    "NTDLL.dll": [
      "NtAllocateVirtualMemory",
      "..."
    ],
    "KERNELBASE.dll": [
      "VirtualAlloc"
      "..."
    ],
    "KERNEL32.dll": [
      "VirtualAlloc"
      "..."
    ]
  },
  "StackBasedHooking": {
    "Functions": [
      "NtCreateUserProcess",
      "..."
    ]
  },
  "SSNCrushingRoutines": {
    "Functions": [
      "NtCreateSection"
      "..."
    ]
  },
  "IATHooking": {
    "Functions": [
      "VirtualAlloc",
      "..."
    ]
  }
}

```

<ul>
    <li><b>DLLBasedHooking</b>: Not modifiable 🚫​​, changing its values will have absolutely no effect at all. Information purposes only.</li>
    <li><b>StackBasedHooking</b>: Modifiable ✅, the functions you specify here will be monitored and their call will trigger an analysis of the calling thread's call stack.</li>
    <li><b>SSNCrushingRoutines</b>: Modifiable ✅, the NT-level routines you will specify here will be attributed a corrupted SSN, Be careful of specifying NT-Level routines ONLY !</li>
    <li><b>IATHooking</b>: Modifiable ✅, the functions you specify here will be hooked at IAT level</li>
</ul>

If you don't compile your own DLLs, take a look at the functions already hooked into the DLLs provided <a href="DLLs/">in sources</a>.

## <a href="https://github.com/Xacone/BestEdrOfTheMarket/releases/tag/Beta">Releases</a> 📦

## <a href="Docs/Setup.md"> Project Setup 🛠️​ </a>

## Disclaimer ⚠️​
- Don't link the EDR to programs that are too CPU-intensive/thread-creating, as some detection techniques such as call stack analysis constantly monitor the stack state of each thread and this can quickly increase the load on the EDR, it's more relevant (that's also the point) that you link the tool to your own artifacts and keep in mind that a good evasive artifact tries to be as discrete as possible.