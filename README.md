# nvidia-force-comp-pipeline

A simple script that auto-enables "ForceCompositionPipeline", "ForceFullCompositionPipeline" and "AllowGSYNC" on all monitors at startup.

## The ForceCompositionPipeline mode

### What is ForceCompositionPipeline?

ForceCompositionPipeline is a special mode that can be enabled on monitors connected to Nvidia cards. It can be found in the Nvidia control panel : go to "X Server Display Configuration", select your monitor, click "Advanced".

![Screenshot of the ForceCompositionPipeline option in the Nvidia control panel](screenshots/nvidia-settings.png)

## Installation

**NOTE From Forker:** This repo is forked from [nvidia-force-comp-pipeline](https://github.com/Askannz/nvidia-force-comp-pipeline) and only change that are made are editing README file, removing ".desktop" "PKGBUILD" files and adding "ForceFullCompositionPipeline=On, AllowGSYNC=On" to line 39. So it enables all "ForceCompositionPipeline", "ForceFullCompositionPipeline" and "AllowGSYNC".

To use this script;
- Save "nvidia-force-comp-pipeline-and-GSync.py" to somewhere safe in your harddrive. You can do this by opening the file in GitHub, clicking "Raw" button and doing "Right-Click > Save As..." or cloning the repository using "git clone [repo_link]" and copying the file from clone path to wherever you'd like to.
- Open your operating system's Auto Start menu and add the script as a "Login Script". (Example for Manjaro KDE: Settings > Start and Shutdown > Autostart > Add > Add Login Script > Choose file path...)
- That's all.

## Known issues

### PRIME monitors

ForceCompositionPipeline does *not* work with monitors that show up up as "PRIME" in the Nvidia control panel (the screenshot above gives an example). A PRIME monitor is not directly connected to the Nvidia GPU but goes through another GPU first. This type of configuration can be found in gaming laptops (the "Optimus" setup) or in desktop computers that have an integrated GPU in their motherboard.

PRIME monitors can have tearing problems too, but the cause is entirely different. To fix the tearing, you should not look into enabling ForceCompositionPipeline but [PRIME Sync](https://devtalk.nvidia.com/default/topic/957814/linux/prime-and-prime-synchronization/) instead.

### DPI

Enabling ForceCompositionPipeline seems to randomly mess with the DPI settings of X11 (on my KDE desktop at least), which causes texts to suddenly become too small or the window decorations to overflow.

If you experience this as well, the fix is to set the DPI setting directly as an argument to the X server. On KDE, this can be achieved by adding the following section to `/etc/sddm.conf` (create the file if it does not exist) :

```
[X11]
ServerArguments=-nolisten tcp -dpi 96
```

Replace 96 by your desired DPI value.

### Multiple Nvidia GPUs

The script will probably not work on setups that have multiple monitors connected to different Nvidia GPUs. This is because it parses the output of `nvidia-settings` to get the current monitor metamode.
