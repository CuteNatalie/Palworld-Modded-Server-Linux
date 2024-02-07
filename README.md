# Palworld-Modded-Server-Linux
This tutorial will guide you through the process of setting up a modded Palworld server on a Linux system using Wine

> [!IMPORTANT]
> You must have wine installed. Follow the tutorial at https://wiki.winehq.org/Download for your specific distro.
> I also recommend winetricks for this tutorial. (After you have installed wine)

Winetricks installation for various distros:

- **Ubuntu/Debian**:
```bash
sudo apt-get update
```
```bash
sudo apt-get install winetricks
```
- **Fedora**:
```bash
sudo dnf update
```
```bash 
sudo dnf install winetricks
```
- **Arch Linux**:
```bash
sudo pacman -Syu
```
```bash
sudo pacman -S winetricks
```

# Tutorial
---

### 1. Installing winbind

- **Ubuntu/Debian**:
```bash
sudo apt-get update
```
```bash
sudo apt-get install winbind
```
- **Fedora**:
```bash
sudo dnf update
```
```bash 
sudo dnf install samba-winbind
```
- **Arch Linux**:
> [!NOTE]
> - In Arch Linux, the winbind service is provided by the samba package.
```bash
sudo pacman -Syu
```
```bash
sudo pacman -S samba
```

### 2. You must install VC runtime 2015-2022 into your wine prefix or else the server will fail to run

> [!IMPORTANT]
> - Only for users who don't have a display hooked up to their server
> - You must have a terminal that supports the X11 Display Server.
> - I recommend https://mobaxterm.mobatek.net/ if using windows
> - Use xquartz if using Mac OS
> - Use the -X option when using ssh if using linux or using wsl

>[!IMPORTANT]
> - MAKE SURE THAT WINE IS SET TO A 64 BIT PREFIX OR ELSE NOTHING WILL RUN PROPERLY. To do this, make sure to set WINEARCH to win64 by doing `export WINEARCH=win64`.

# If you have winetricks installed:
```bash
winetricks vcrun2022
```

# If you don't have winetricks installed:
1. Download the latest supported Visual C++ redistributable x64 package for Visual Studio 2015, 2017, 2019, and 2022 from Microsoft's official site: [here](https://learn.microsoft.com/en-us/cpp/windows/latest-supported-vc-redist?view=msvc-170#visual-studio-2015-2017-2019-and-2022).
2. Once downloaded, navigate to the directory where the .exe file is located.
3. Use the following command to install the .exe file via Wine:
wine vc_redist.x64.exe
Replace `vc_redist.x64.exe` with the name of the file you downloaded.
4. During the installation, if you are prompted to install the mono package or html package, proceed with the installation. These packages are necessary for some applications to run correctly in Wine.

### 3. Install steamcmd into your wine prefix

1. Download the Windows version of SteamCMD from the official [Valve Developer Community](https://developer.valvesoftware.com/wiki/SteamCMD#Windows) page.
2. Extract the contents of the downloaded zip file.
3. Navigate to the root directory of your Wine prefix (default is `~/.wine/drive_c/`).
4. Create a new folder named `steamcmd` in this directory.
5. Move the extracted SteamCMD files into this newly created folder.
6. Now, you have installed SteamCMD into your Wine prefix.

> [!NOTE]
> - To simplify the process of running SteamCMD, you can create a new command that runs `wine` then `steamcmd.exe` as a normal command, enabling you to run it from anywhere. Here's how:
>   1. Create a new file called `steamcmd`.
>   2. Add the following line to it:
>   wine /home/(user)/.wine/drive_c/steamcmd/steamcmd.exe $@
>   3. Replace '(user)' with your actual username.
>   4. Save the file.
>   5. Make the file executable by running the following command:
>   chmod +x steamcmd
>   6. Move the `steamcmd` file to your `bin` directory:
>   sudo mv steamcmd /usr/local/bin/
>
> Now, you can run SteamCMD from anywhere by simply typing `steamcmd` in the terminal.

### 4. Setting up Steamcmd and PalServer
1. Go to /home/(user)/.wine/drive_c/steamcmd and execute the steamcmd command using wine so that it updates and installs the neccisarry files.
```bash
cd /home/(user)/.wine/drive_c/steamcmd
```
```bash
wine steamcmd.exe +quit
```
2. Now that we have the neccisarry files for steamcmd we can install the Palworld Server using steamcmd.
```bash
wine steamcmd.exe +login anonymous +app_update 2394010 validate +quit
```
3. PalServer should now be installed to `/home/(user)/.wine/drive_c/steamcmd/steamapps/common/PalServer`

### 5. Modding the PalServer
1. Download the winmm.dll file from the [discord](https://cdn.discordapp.com/attachments/1107095082567471114/1200053412126003250/winmm.zip) and the UE4SS modding framework from [UE4SS Github](https://github.com/UE4SS-RE/RE-UE4SS/releases/download/v3.0.0/UE4SS_v3.0.0.zip)
2. Move the extracted files to `/home/(user)/.wine/drive_c/steamcmd/steamapps/common/PalServer/Pal/Binaries/Win64`
3. Navigate into the `/home/(user)/.wine/drive_c/steamcmd/steamapps/common/PalServer/Pal/Binaries/Win64` folder and run `wine PalServer-Win64-Test-Cmd.exe'
4. (Optional) To verify if its working, I suggest setting `GuiConsoleVisible` in `UE4SS-settings.ini` to `1`
