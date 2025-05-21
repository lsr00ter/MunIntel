# MunIntel

kASLR bypass technique on Intel CPUs using `PREFETCHh` to leak the Windows kernel base via Cache timing.

![](/imgs/20250517011029.png)

For a detailed explanation of the technique, see my blog post [Bypassing kASLR via Cache Timing](https://r0keb.github.io/posts/Bypassing-kASLR-via-Cache-Timing/)

## Building MunIntel

This project targets **x64 architecture**. Mixing 32-bit (x86) and 64-bit (x64) build tools or outputs will result in linker errors such as:

```powershell
fatal error LNK1112: module machine type 'x64' conflicts with target machine type 'x86'
```

### Prerequisites

- Microsoft Visual Studio (any recent version)
- Windows SDK with x64 toolchain
- Assembly tool: `ml64.exe` (comes with Visual Studio)
- C++ compiler: `cl.exe` (comes with Visual Studio)

### Build Instructions

#### 1. Open the Correct Developer Command Prompt

Open the **"x64 Native Tools Command Prompt for VS"**. This sets the environment to target 64-bit.

#### 2. Assemble the Assembly File

```cmd
ml64 /c /Fo sideChannel.obj sideChannel.asm
```

#### 3. Compile the C++ File and Link

```cmd
cl /EHsc /Fe:main.exe main.cpp sideChannel.obj
```

- `/EHsc`: Enables standard C++ exception handling.
- `/Fe`: Sets the output executable name.

#### 4. Clean Old Build Files

If you encounter linker errors, delete any `.obj`, `.exe`, or `.lib` files from previous builds to ensure there are no architecture conflicts.

#### 5. (Optional) Visual Studio IDE

If building in Visual Studio IDE, ensure the Solution Platform is set to **x64** (not x86).

---

## Troubleshooting

- **LNK1112 error?**  
  Double-check that all tools and source files are targeting x64. Do not use x86 tools or settings.
