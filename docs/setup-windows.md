# Windows setup

This file is a step-by-step guide to installing everything needed to build and run PhoenixOS on Windows. It was written on my Windows desktop, and every step is meant to work for someone starting from a fresh machine.

## Before you start

- Windows 10 or 11.
- **PowerShell.** Press the Windows key, type `PowerShell`, and open it. All commands below run there.
- **winget**, the Windows package manager. Check it with `winget --version`. If that fails, install "App Installer" from the Microsoft Store.
- Some commands below use `winget`. If it says a package ID isn't found, run `winget search <name>` to find the current ID.

## How to check a tool

Run the check command for each tool. If it prints a version, it's installed. If it says "not recognized", either it isn't installed or its folder isn't on your PATH. See "How to add a folder to your PATH" below.

## How to add a folder to your PATH

Some tools install fine but Windows can't find them by name, because their folder isn't on the PATH. To fix it:

1. Find the folder that contains the program. If you don't know where it went, search for it. This example looks for `nasm.exe`:
   ```powershell
   Get-ChildItem "C:\Program Files", "C:\Program Files (x86)", "$env:LOCALAPPDATA" -Recurse -Filter nasm.exe -ErrorAction SilentlyContinue | Select-Object FullName
   ```
2. Press the Windows key, search for `environment variables`, and open "Edit environment variables for your account".
3. Under "User variables", select `Path` and click Edit.
4. Click New and paste the full folder path (the folder, not the `.exe` file).
5. Click OK on every window to save.
6. **Close PowerShell and open a new window.** Windows that were already open won't see the change.
7. Run the tool's check command again.

## Needed for Milestone 0

### Git
- **What it's for**: version control, and it includes ssh for pushing to GitHub
- **Check**: `git --version`
- **Install**: `winget install Git.Git`
- **After installing**: set your name and email once:
  ```powershell
  git config --global user.name "Your Name"
  git config --global user.email "you@example.com"
  ```

### QEMU
- **What it's for**: the emulator that runs my boot sector as if it were a real computer
- **Check**: `qemu-system-x86_64 --version`
- **Install**: `winget install SoftwareFreedomConservancy.QEMU`
- **PATH**: add `C:\Program Files\qemu` to the user PATH

### NASM
- **What it's for**: the assembler that turns `boot.asm` into machine code
- **Check**: `nasm -v`
- **Install**: `winget install NASM.NASM`
- **PATH**: it installs per user, so the folder is `C:\Users\<your username>\AppData\Local\bin\NASM`. To print the exact path for your machine, run `echo "$env:LOCALAPPDATA\bin\NASM"`, then add that folder to the user PATH.

### Editor
- **What it's for**: writing the code. Any editor works, so this is optional.
- **Check**: `code --version`
- **Install**: `winget install Microsoft.VisualStudioCode`

### Test that the Milestone 0 tools work together

This builds a blank 512-byte boot sector in your temp folder, so it doesn't touch the repo:

```powershell
cd $env:TEMP
Set-Content -Path t.asm -Value "times 510 db 0`ndw 0xAA55"
nasm -f bin t.asm -o t.bin
(Get-Item t.bin).Length
Format-Hex t.bin | Select-Object -Last 3
```

The length should print `512`, and the last hex row should end in `55 AA`. Those two bytes are the signature the BIOS looks for. To see it boot in QEMU (it will show a screen that does nothing, since there is no code in it yet), run:

```powershell
qemu-system-x86_64 -drive format=raw,file=t.bin
```

## Needed later (Milestone 4 and on)

### WSL2
- **What it's for**: a Linux environment inside Windows, so the toolchain matches my Ubuntu laptop
- **Check**: `wsl --version`
- **Install**: open PowerShell as administrator (right-click, "Run as administrator") and run `wsl --install`, then restart the computer. It installs Ubuntu by default, and the first launch asks you to create a Linux username and password.

### Cross-compiler
- The gcc from MSYS2 builds Windows programs, which assume Windows underneath. A kernel needs a freestanding compiler that doesn't. I haven't decided how I'll get one, so this section will be written when I reach Milestone 4.
- **Check** (current gcc, for reference): `gcc --version`

### make
- **What it's for**: runs the build steps
- **Check**: `make --version`
- **Install**: `winget install ezwinports.make`
- This is a Windows copy. If I install make inside WSL2 later, that will be a separate one.

## Optional

### GitHub CLI
- **What it's for**: working with GitHub from the terminal
- **Check**: `gh --version`
- **Install**: `winget install GitHub.cli`
- **Login**: `gh auth login`. Choose HTTPS or SSH to match how your repo connects. Run `git remote -v` in the repo: a URL starting with `https://` means HTTPS, and one starting with `git@github.com:` means SSH. On this PC the repo uses HTTPS, and my Ubuntu laptop uses SSH.
- **Confirm**: `gh auth status`

## Check everything at once

Paste this into PowerShell. Each tool prints either a version or MISSING. `make` and `gh` are expected to be missing until you install them.

```powershell
$checks = @(
  @{ Name = "git";   Cmd = { git --version } },
  @{ Name = "ssh";   Cmd = { ssh -V 2>&1 } },
  @{ Name = "qemu";  Cmd = { qemu-system-x86_64 --version | Select-Object -First 1 } },
  @{ Name = "nasm";  Cmd = { nasm -v } },
  @{ Name = "code";  Cmd = { code --version | Select-Object -First 1 } },
  @{ Name = "gcc";   Cmd = { gcc --version | Select-Object -First 1 } },
  @{ Name = "make";  Cmd = { make --version | Select-Object -First 1 } },
  @{ Name = "wsl";   Cmd = { wsl --version | Select-Object -First 1 } },
  @{ Name = "gh";    Cmd = { gh --version | Select-Object -First 1 } }
)
foreach ($c in $checks) {
  try {
    $out = & $c.Cmd 2>$null
    if ($out) { "{0,-5} OK       {1}" -f $c.Name, $out } else { "{0,-5} MISSING" -f $c.Name }
  } catch {
    "{0,-5} MISSING" -f $c.Name
  }
}
```

## Problems I ran into

- **NASM was "not recognized" after installing.** The first `winget install` hadn't actually run (`winget list NASM` showed nothing). After a second install, the file still wasn't found because it installs to `%LOCALAPPDATA%\bin\NASM`, not Program Files. Fix: find it with the search command above, then add that folder to the user PATH and open a new PowerShell window.
- **QEMU was "not recognized" after installing.** Same cause: the folder wasn't on PATH. Fix: add `C:\Program Files\qemu` to the user PATH and open a new PowerShell window.
- **`gh auth login` asked for HTTPS or SSH and I didn't know which.** Run `git remote -v` in the repo to find out.

## Setup log

Windows desktop, last checked 2026-11-05.

| Tool | Version |
|---|---|
| Git | 2.56.0.windows.4 |
| QEMU | 11.1.0 |
| NASM | 3.02 |
| VS Code | 1.145.0 |
| gcc (MSYS2) | 15.2.0 |
| WSL | 2.6.3.0 |
| GitHub CLI | 2.103.3 |
