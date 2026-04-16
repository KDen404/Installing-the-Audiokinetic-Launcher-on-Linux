# Installing-the-Audiokinetic-Launcher-on-Linux
This will teach you how you can Setup the Audiokinetic Launcher on Linux and Install the Wwise Authoringtool
It may not be perfect but an important first step in making The Wwise Sound Engine Authoringtool and its Tools available on Linux

I Used a fedora 44 for this.

prerequisites:
```bash
dnf install wine winetricks qt5-qttools
```

Then reboot your device.

wine setup:
```bash
# some may not be necessary if you want test which are really necessary
winetricks winhttp # windows http support
winetricks wininet # another internet protocol api widely adapted on windows
winetricks xaudio29 # wwise uses xaudio2 as its audio interfeace therefore it might be necessary
winetricks vcrun2022 # Visual C++ Redistributable 2015 - 2022 THIS IS IMPORTANT WITHOUT IT EVERY INSTALL ATTEMPT WITHIN THE LAUNCHER WILL FAIL
```

Installing the Audiokinetic Launcher:
1. Download the Windows Audiokinetic Launcher: https://www.audiokinetic.com/en/download-launcher/?platform=1
2. `wine ~/Downloads/AudiokineticLauncher-*.exe`
3. confirm
4. setting the xdg-scheme:   
4.1. `nano ~/Desktop/Wwise\ Launcher.desktop ~/.local/share/applications/wwise-launcher.desktop`
  ```ini
  # This is how it roughly should look like %u in exec is important and the MimeType Line is important
  [Desktop Entry]
  Name=Wwise Launcher
  Exec=env "WINEPREFIX=/home/[Your Username]/.wine" wine "C:\\\\users\\\\Public\\\\Desktop\\\\Wwise Launcher.lnk" %u
  Type=Application
  StartupNotify=true
  Comment=Launcher for Wwise
  Path=/home/[Your Username]/.wine/drive_c/Program Files/Wwise Launcher
  Icon=14AD_Wwise Launcher.0
  StartupWMClass=wwise launcher.exe
  MimeType=x-scheme-handler/wwise-launcher;x-scheme-handler/ak-launcher;
  ```
4.2 refreshing the Database and setting the url-schemes:
  ```bash
  cp ~/Desktop/Wwise\ Launcher.desktop ~/.local/share/applications/wwise-launcher.desktop
  update-desktop-database ~/.local/share/applications
  xdg-mime default wwise-launcher.desktop x-scheme-handler/wwise-launcher
  xdg-mime default wwise-launcher.desktop x-scheme-handler/ak-launcher
  ```

5. check if the schemes were set correctly:
```bash
xdg-mime query default x-scheme-handler/wwise-launcher
xdg-mime query default x-scheme-handler/ak-launcher
```
expected output:
```
wwise-launcher.desktop
wwise-launcher.desktop
```

6. Test: If successful no terminal output and the audiokinetic launcher should start if it was running yet.
```bash
xdg-open "wwise-launcher://test"
xdg-open "ak-launcher://test"
```

Afterwards the prior not working login should work, afterwards you can install a Wwise Version.
If the install throws an error at the first attempt press the retry button, it should restart the install and everything should work from now on.
If you done everything correct you should be able to install the Wwise Authoringtool and use it on Linux.

Not tested yet:
- Unity Integration Setup using the Audiokinetic Launcher
- Unreal Engine Integration Setup using the Audiokinetic Launcher
- Installing Samples from the Samples section
