**How to Remove DRM from Nook Books** (as of 11/22/2020)

The previous guide by [Aric Renzo](https://www.aricrenzo.com/2019-12-13-Liberate-Your-Nook-Ebooks/ "Aric Renzo") was super helpful, but dated. So this is the updated one for noobs.

This guide is for **Windows**. Also Before you begin, JFYI. Android Studio is a [b1tch to remove](https://stackoverflow.com/questions/39953495/how-to-completely-uninstall-android-studio-from-windowsv10 "beach to remove") and is huge.. Stores files all over. Just be forewarned.

## Requirements

**[Android Studio](https://developer.android.com/studio/ "Android Studio")** and Android Virtual Device (Check the box when installing)   
**[Calibre](https://download.calibre-ebook.com/ "Calibre")** Use version 4.xx. DeDRM does not work on 5xx, as of 11/01/2020   
**[DeDRM Plugin](https://github.com/apprenticeharper/DeDRM_tools/releases/latest "DeDRM Plugin")** - Version 6.6.3 as of as of 11/01/2020   
**[Nook APK](https://apkpure.com/nook-read-ebooks-magazines/bn.ereader/versions)** - I used v5.0.2.38.

For sake of simplicity. I am just going to assume you have Calibre installed and the De-DRM plugin already working. If not, go read [Alfs blog](https://apprenticealf.wordpress.com/ "Alf's blog"). Come back once that is done.

## Setup

**Installing Android Studio**
- Download Android Studio
- Run installer
- Leave Android Virtual Device [Selected](https://github.com/L0garithmic/remove-nook-drm-2020/blob/main/images/001-avd.jpg) 
- Leave Default directory
- Run Android Studio

**Android Studio Setup**

If this is your first install of Android Studio
- Do not import settings, `OK`
- `Don't send` usage statistics
- `Next`, Standard, `Next`, Light, `Next`, `Finish`
- Wait for large download/extract
- `Create New Project`
- No Activity, `Next`, Leave Default, `Next`

If you have already ran Android Studio, start here
- [Open](https://github.com/L0garithmic/remove-nook-drm-2020/blob/main/images/002-avdmgr.jpg) the `AVD Manager`.
- `Create Virtual Device`
- Nexus 5 `Next`
- x86 Images tab, scroll down.
- `Download` Android 7.1.1 x86 [(Not Google APK](https://github.com/L0garithmic/remove-nook-drm-2020/blob/main/images/003-nougat.jpg))
- `Accept` if necessary, otherwise `Next`
- Leave Default, `Finish`
- Click `Play` under [Actions](https://github.com/L0garithmic/remove-nook-drm-2020/blob/main/images/004-playact.jpg)
- You should now see an android Emulator

**Nook App Setup**
Assuming you have already downloaded the Nook APK. 
- [drag the APK](https://github.com/L0garithmic/remove-nook-drm-2020/blob/main/images/005-dragapk.jpg) into the virtual device
- Click [up arrow](https://github.com/L0garithmic/remove-nook-drm-2020/blob/main/images/006-uparrow.jpg) on bottom of device
- Open Nook App
- Login to your account
- Wait for your Library to load
- Click on any items you want to download

## Getting Books and Database

Assuming you listened and your install directories are default. This should all work.
- Open up a fresh [Command Prompt](https://www.lifewire.com/how-to-open-command-prompt-2618089 "CMD") from start menu
- Paste this into it `cd %LOCALAPPDATA%\Android\Sdk\platform-tools`
- Run these commands. `adb root`, `adb pull data/data/bn.ereader`

**The transfer didn\'t work. Haaaalp!**
If the above download did not work. Means more work for you.
- run `adb root`,`adb shell`,`ls data/data`
- look for a folder named something similar to Nook, BN, Noble, Barnes.. Idk.
- run above commands changing out `bn.reader` for new foldername.

## Extracting Hash from Database
The hash file will be saved as `mykey.b64` in `%LOCALAPPDATA%\Android\Sdk\platform-tools`
Just paste the location into the url bar of any file explorer in windows.
- Open up a fresh [Command Prompt](https://www.lifewire.com/how-to-open-command-prompt-2618089 "CMD") from start menu
- run `cd %LOCALAPPDATA%\Android\Sdk\platform-tools`
- run `sqlite3 bn.ereader/databases/cchashdata.db`
- run `.output mykey.b64`
- run `select hash from cc_hash_data;`
- then `.quit` and `exit`

## Configuring De-DRM
Open Calibre. Open Plugins settings. `Preferences -> Plugins` (or hit `Ctrl+P`), expand `File Type`, select DeDRM and `double-click` (or click `Customize Plugin`)

Click `Barnes and Noble ebooks`, click `Import Existing Keyfiles`, browse to the `mykey.b64` file mentioned above. Click `Open`. Then click `Close`, `OK`, `Apply` and `Close`

Now you can import your Nook Books. Which if you used the Nook Android App to download them as mentioned in the **Nook App Setup** part, they should all be here `%LOCALAPPDATA%\Android\Sdk\platform-tools\bn.ereader\files\B&N Downloads\Books`. If your ebooks are already imported, delete them and re-import them. DeDRM only removes the DRM on books during the import phase. If you missed the \"download in the app\" step and don\'t want to start all over again..

You can use the [Nook for PC app](http://www.nook.com/nookapp/ "Nook for PC app") to download the books. (It\'s a Windows Store App, yuck), install it then download any books you wanted to De-DRM, they will be stored in  `%LOCALAPPDATA%\Packages\BarnesNoble.Nook_ahnzqzva31enc\LocalState` (or similarly named) and will be numbered .epub files.
