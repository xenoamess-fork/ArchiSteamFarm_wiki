# 設定

如果你是第一次來到這裡，歡迎！ 我們很高興看到另一位對我們的項目感興趣的旅行者，但請記住能力越強責任越大 - 只要你**足夠小心學習如何使用它**，ASF 能夠完成許多不同的 Steam 相關事務。 這將會是一個陡峭的學習曲線，我們希望您在這方面閱讀維基，它會詳細解釋了一切如何運作。

如果你還在這裡，那就表示你可以忍受了我們上面的文字，非常好。 除非你跳過它，否則你很快就會有一個**[很糟的時刻](https://www.youtube.com/watch?v=WJgt6m6njVw)**... 無論如何，ASF是一個控制台應用程序，這意味著應用程式本身沒有你習慣的友好圖型介面。 ASF主要設計是在伺服器上執行，所以它只是一個服務 (daemon) 而非桌面應用程式。

然而這並不代表著你不能在 PC 上使用它，或它在某種程度上比平常更複雜，並不是這樣的。 ASF是一個獨立的程式，不需要安裝並且可以立即使用，但這之前需要進行設置。 設置 ASF 啟動它之後應該做什麼。 如果你在沒有設置的情況下啟動它，那麼 ASF 將不會做任何事情，就是這麼簡單。

* * *

## 特定作業系統設置

一般來說，這是我們在接下來的幾分鐘內要做的事情：

- 安裝 **[.NET 核心套件](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**。
- 下載適合你作業系統的**[最新版 ASF ](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**
- 將解壓縮到你指定的位置 ( 並且 `chmod +x ArchiSteamFarm`，若使用Linux/OS X 系統)。
- **[設定 ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**
- 執行 ASF 並觀賞神奇的一刻

聽起來有夠簡單，是吧? 所以讓我們繼續吧。

* * *

### .NET 核心套件

第一步是確保您的作業系統可以正確地啟動 ASF。 ASF 是用 c# 編寫的, 基於 .NET 核心, 並且可能需要你的系統上尚未使用的本機庫。 根據您是否使用 Windows、Linux 或 OS X, 您將有不同的要求, 它們都列在您應該遵循的 **[.NET 核心套件](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)** 文件中。 這是我們應該使用的參考資料，但為了簡單起見，我們還額外詳細說明了下面所有需要的軟體。因此您無需閱讀完整的文件。

It's perfectly normal that some (or even all) dependencies already exist on your system due to being installed by third-party software that you're using. Still, you should ensure that it's truly the case by running appropriate installer for your OS - without those dependencies ASF won't launch at all.

Keep in mind that you don't need to do anything else for OS-specific builds, especially installing .NET Core SDK or even runtime, since OS-specific package includes all of that already. You need only .NET Core prerequisites (dependencies) to run .NET Core runtime included in ASF.

#### **[Windows](https://docs.microsoft.com/en-us/dotnet/core/windows-prerequisites?tabs=netcore2x)**:

- **[Microsoft Visual C++ 2015 Redistributable Update 3 RC](https://www.microsoft.com/en-us/download/details.aspx?id=52685)** (x64 for 64-bit Windows, x86 for 32-bit Windows)
- 強烈建議確保已安裝所有Windows更新。 至少需要 **[KB2533623](https://support.microsoft.com/en-us/help/2533623/microsoft-security-advisory-insecure-library-loading-could-allow-remot)** 和 **[KB2999226](https://support.microsoft.com/en-us/help/2999226/update-for-universal-c-runtime-in-windows)**，並可能需要更多更新檔。 如果您的Windows更新到最新版，則上述所有都已安裝。 在安裝 Visual C ++ 之前，請確保滿足這些要求。

#### **[Linux](https://docs.microsoft.com/en-us/dotnet/core/linux-prerequisites?tabs=netcore2x)**:

Package names depend on the Linux distribution that you're using, we've listed the most common ones. You can obtain all of them with native package manager for your OS (such as `apt` for Debian or `yum` for CentOS).

- libcurl3 (libcurl)
- libicu (latest version for your distribution, for example `libicu57` for Debian 9)
- libkrb5-3 (krb5-libs)
- liblttng-ust0 (lttng-ust)
- libssl1.0.2 (libssl, openssl-libs, compat-openssl10, latest 1.0.X version for your distribution)
- zlib1g (zlib)

At least a few of those should be already natively available on your system (such as zlib1g that is required in almost every Linux distro today).

#### **[OS X](https://docs.microsoft.com/en-us/dotnet/core/macos-prerequisites?tabs=netcore2x)**:

- None for now

* * *

### 下載

Since we have all required dependencies already, the next step is downloading **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)**. ASF is available in many variants, but you're interested in package that matches your operating system and architecture. For example, if you're using `64`-bit `Win`dows, then you want `ASF-win-x64` package. For more information about available variants, visit **[compatibility](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility)** section. ASF is also able to run on OSes that we're not building OS-specific package for, such as **32-bit Windows**, head over to **[generic setup](#generic-setup)** for that.

![更新檔](https://i.imgur.com/Ym2xPE5.png)

Once you get your package and extract the zip file (we recommend using **[7-zip](https://www.7-zip.org)**), you'll have a huge mess of folders and files. Don't worry, we'll clean it up in a second.

If you're using Linux/OS X, don't forget to `chmod +x ArchiSteamFarm`, since permissions are not automatically set in the zip file. This has to be done only once after initial unpack.

Be advised to unpack ASF to **its own directory** and not to any existing directory you're already using for something else - ASF's auto-updates feature will delete all old and unrelated files when upgrading, which might lead to you losing anything unrelated you put in ASF directory. If you have any extra scripts or files that you want to use with ASF, put them in one folder above.

An example structure would look like this:

    C:\ASF (where you put your own things)
        ├── ASF shortcut.lnk (optional)
        ├── Config shortcut.lnk (optional)
        ├── Commands.txt (optional)
        ├── MyExtraScript.bat (optional)
        ├── ... (any other files of your choice, optional)
        └── Core (dedicated to ASF only, where you extract the archive)
             ├── ArchiSteamFarm.dll
             ├── config
             └── (...)
    

This is also a structure we'd recommend, so you don't need to go through a massive number of files and folders included in ASF, since for usage you only need a shortcut to config folder and main binary.

Okay, we'll now prepare ASF folder for usage. If you want to, you can now skip to the next step, since cleaning up ASF structure is not required, but it will make your life a bit easier.

Open ASF folder and find core executable file, this will be `ArchiSteamFarm.exe` on Windows, and `ArchiSteamFarm` on Linux/OS X. Right click it and select "copy". Now navigate to the place you actually want to have ASF shortcut in (such as your desktop), right click and choose "paste shortcut here". You can rename your shortcut if you'd like to, such as giving it "ASF" name. Now do the same with `config` directory that you can find in the same place as ASF binary.

After a small cleanup, you'll now have a very convenient structure similar to the one below:

![架構](https://i.imgur.com/k85csaZ.png)

This will allow you to easily access ASF binary and config files without much hassle. In my case I decided to use the structure mentioned above, so my ASF files are in "Core" directory directly inside. You can adapt this structure to your liking, such as having ASF + config shortcuts on the desktop and ASF directory e.g. in `C:\ASF` instead, it's up to you.

Linux/OS X users are advised to do the same, you can use excellent symbolic links mechanism available through `ln -s`.

* * *

### 配置

We're now ready to do the very last step, the configuration. This is by far the most complicated step, since it involves a lot of new information you're not familiar with yet, so we'll try to provide some easy to understand examples and simplified explanation here.

First and foremost, there is **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** page that explains **everything** that relates to configuration, but it's a massive amount of new information, a lot of which we don't need to know right away. Instead, we'll teach you how to get the information you're actually looking for.

ASF configuration can be done in two ways - either by using our web config generator, or manually. This is explained in-depth in **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section, so refer to that if you want more detailed information. We'll use web config generator way, since it's much easier.

Navigate to our **[web config generator](https://justarchinet.github.io/ASF-WebConfigGenerator)** page with your favourite browser, you'll need to have javascript enabled in case you manually disabled it. We recommend Chrome or Firefox, but it should work on all most popular browsers.

After opening the page, switch to "Bot" tab. You should now see a page similar to the one below:

![Bot tab](https://i.imgur.com/aF3k8Rg.png)

If by any chance the version of ASF that you've just downloaded is older than what config generator is set to use by default, simply choose your ASF version from the dropdown menu. This can happen as the config generator can be used for generating configs to newer (pre-release) ASF versions that weren't marked as stable yet. You've downloaded latest stable release of ASF that is verified to work reliably.

Start from putting name for your bot into the field highlighted as red. This can be any name you'd like to use, such as your nickname, account name, a number, or anything else. There is only one word that you can't use, `ASF`, as that keyword is reserved for global config file. In addition to that your bot name can't start with a dot (ASF intentionally ignores those files). We also recommend that you avoid using spaces, you can use `_` as a word separator if needed.

After you decided about your name, change `Enabled` switch to be on, this defines whether your bot is supposed to be started by ASF automatically after launch (of the program).

Now you can decide upon two things:

- You can put your login in `SteamLogin` field and your password in `SteamPassword` field
- Or you can leave them empty

Doing the first thing will allow ASF to automatically use your account credentials during startup, so you won't need to input them manually each time ASF needs them. You can however decide to omit them, in which case they're not being saved, so ASF won't be able to automatically start without your help and you'll need to input them during runtime.

ASF requires your login credentials because it includes its own implementation of Steam client and needs the same details to log in as the one that you use yourself. Your login credentials are not saved anywhere but on your PC in ASF `config` directory only, our web config generator is client-based which means that the code is run locally in your browser to generate valid ASF configs, without details you're inputting ever leaving your PC in the first place, so there is no need to worry about any possible sensitive data leak. Still, if you for whatever reason don't want to put your credentials there, we understand that, and you can put them manually later in generated files, or omit them entirely and put them only in ASF command prompt. More on security matter can be found in **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section.

You can also decide to leave just one field empty, such as `SteamPassword`, ASF will then be able to use your login automatically, but will still ask for password (similar to Steam Client). If you're using Steam parental to unlock the account, you'll need to put it into `SteamParentalCode` field.

After the decision and optional details, your web page will now look similar to the one below:

![Bot tab 2](https://i.imgur.com/yf54Ouc.png)

You can now hit "download" button and our web config generator will generate new `json` file based on your chosen name. Save that file into `config` directory of ASF. You can use previously-created `config` shortcut, or find `config` directory manually, directly in ASF file structure.

Your `config` directory will now look like this:

![架構 2](https://i.imgur.com/2s7ZUUu.png)

Congratulations! You've just finished the very basic ASF bot configuration. We'll extend this shortly, for now this is everything that you need.

* * *

### Running ASF

You're now ready to launch the program for the first time. Simply double-click ASF shortcut, or `ArchiSteamFarm(.exe)` binary in ASF directory.

After doing so, assuming you installed all required dependencies in the first step, ASF should launch properly, notice your first bot (if you didn't forget to put generated config in `config` directory), and attempt to log in:

![ASF](https://i.imgur.com/u5hrSFz.png)

If you supplied `SteamLogin` and `SteamPassword` for ASF to use, you'll be asked for your SteamGuard token only (e-mail, 2FA or none, depending on your Steam settings). If you didn't, you'll also be asked for your Steam login and password.

Now is a good time to review our **[privacy policy](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Statistics#current-privacy-policy)** section if you're concerned about what will happen next, as stated by ASF itself.

After passing through initial login gate, assuming your details are correct, you'll successfully log in, and ASF will start idling using default settings that you didn't change as of now:

![ASF 2](https://i.imgur.com/Cb7DBl4.png)

This proves that ASF is now successfully doing its job on your account, so you can now minimize the program and do something else. After enough of time (depending on **[performance](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Performance)**), you'll see Steam trading cards slowly being dropped. Of course, for that to happen you must have valid games to idle, showing as "you can get X more card drops from playing this game" on your **[badges page](https://steamcommunity.com/my/badges)** - if there are no games to idle, then ASF will state that there is nothing to do, as stated in our **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ#so-how-it-exactly-works)**.

This concludes our very basic setting up guide. You can now decide whether you want to configure ASF further, or let it do its job in default settings. We'll cover a few more basic details, then leave you entire wiki for discovery.

* * *

### Extended configuration

#### Idling several accounts at once

ASF supports idling more than one account at a time, which is its primary function. You can add more accounts to ASF by generating more bot config files, in exactly the same way as you've generated your first one just a few minutes ago. You need to ensure only two things:

- Unique bot name, if you already have your first bot named "MainAccount", you can't have another one with the same name.
- Valid login details, such as `SteamLogin`, `SteamPassword` and `SteamParentalCode` (if using Steam parental settings)

In other words, simply jump to configuration again and do exactly the same, just for your second or third account. Remember to use unique names for all of your bots.

* * *

#### Changing settings

You change existing settings in exactly the same way - by generating a new config file. If you didn't close our web config generator yet, click on "toggle advanced settings" and see what is there for you to discover. For this tutorial we'll change `CustomGamePlayedWhileFarming` setting, which allows you to set custom name being displayed when ASF is idling, instead of showing actual game.

So let's do that, if you run ASF and start idling, in default settings you'll see that your Steam account is in-game now:

![Steam](https://i.imgur.com/sCdSMZj.png)

Let's change that then. Toggle advanced settings in web config generator and find `CustomGamePlayedWhileFarming`. Once you do that, put your own custom text there that you want to display, such as "Idling cards":

![Bot tab 4](https://i.imgur.com/gHqdEqb.png)

Now download the new config file in exactly the same way, then **overwrite** your old config file with new one. You can also delete your old config file and put new one in its place of course.

Once you do that and start ASF again, you'll notice that ASF now displays your custom text in previous place:

![Steam 2](https://i.imgur.com/NeFYrdU.png)

This confirms that you've successfully edited your config. In exactly the same way you can change global ASF properties, by switching from bot tab to "ASF" tab, then downloading generated config and replacing core `ASF.json` file.

* * *

#### Using ASF-ui

ASF is a console app and doesn't include a graphical user interface. However, there is ongoing work on **[ASF-ui](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/IPC#asf-ui)** frontend to our IPC interface which is still in preview state, but can be used without bigger issues.

In order to use ASF-ui, you should ensure that you set up `IPC` and `SteamOwnerID` global configuration properties (ASF tab).

For `SteamOwnerID`, you need to input unique Steam identificator in 64-bit form of your account. You can look it up in various different ways, we'll use **[SteamRep](https://steamrep.com)** for that purpose. Open the website, locate sign in through Steam button in top right corner, then log in. Afterwards, in the same place, click on your avatar, and look up `steamID64` field on your profile.

![SteamRep](https://i.imgur.com/RUuJ63i.png)

For my account, this is `76561198006963719` number. You'll have a similar one, also starting from `7656`. Copy it.

Now navigate once again to our web config generator and input that number as SteamOwnerID.

![SteamOwnerID](https://i.imgur.com/V6jslfQ.png)

You need to do only one more thing, toggle advanced settings, find `IPC` option, and enable it.

![IPC](https://i.imgur.com/NhujZCN.png)

Now you can download your ASF config and put it in your `config` directory, as usual. Afterwards, launch ASF again, and you should be able to confirm that it properly started IPC interface:

![IPC 2](https://i.imgur.com/ZmkO8pk.png)

If you did everything properly, you'll now be able to access ASF's IPC interface under **[this](http://127.0.0.1:1242)** link, as long as ASF is running. You can use ASF-ui for various purposes, e.g. sending **[commands](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Commands)**. Feel free to take a look around in order to find out all ASF-ui functionalities.

![IPC 3](https://i.imgur.com/vCu2ZY5.png)

Please note that ASF-ui is currently in preview state and not everything is available/working yet, but it's more than enough for simple ASF usage.

* * *

### 概要

You've successfully set up ASF to use your Steam accounts and you've already customized it to your liking a little. If you followed our entire guide, then you even managed to send a simple command through our ASF-ui interface. Now is a good time to read our entire **[configuration](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)** section in order to learn what all those different settings you've seen in advanced tab actually do, and what ASF can offer. If you've stumbled upon some issue or you have some generic question, read **[FAQ](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/FAQ)** instead which should cover all, or at least majority of questions that you might have. If you want to learn everything about ASF and how it can make your life easier, head over to the rest of **[our wiki](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Home)**. Have fun!

* * *

## Generic setup

This setup is for advanced users that want to set up ASF to run in **[generic](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#generic)** variant. It's not recommended for people that can use **[OS-specific setup](#os-specific-setup)**.

You want to use generic variant mainly in three situations (but of course you can use it regardless):

- When you're using OS that we don't build OS-specific package for (such as 32-bit Windows)
- When you already have .NET Core Runtime/SDK, or want to install and use one
- When you want to minimize ASF structure size by handling runtime requirements yourself

However, keep in mind that you're in charge of .NET Core runtime in this case. This means that if your .NET Core SDK (runtime) is unavailable, outdated or broken, ASF won't work. This is why we don't recommend this setup for casual users, since you now need to ensure that your .NET Core SDK (runtime) matches ASF requirements and can run ASF, as opposed to **us** ensuring that our .NET Core runtime bundled with ASF can do so.

For generic package, you can follow entire OS-specific guide above, with two small changes. In addition to installing .NET Core prerequisites, you also want to install .NET Core SDK, and instead of having OS-specific `ArchiSteamFarm(.exe)` executable file, you now have a generic `ArchiSteamFarm.dll` binary only. Everything else is exactly the same.

With extra steps:

- 安裝 **[.NET 核心套件](https://github.com/dotnet/core/blob/master/Documentation/prereqs.md)**。
- Install **[.NET Core SDK](https://www.microsoft.com/net/download)** (or at least runtime) appropriate for your OS. You most likely want to use an installer. Refer to **[runtime requirements](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Compatibility#runtime-requirements)** if you're not sure which version to install.
- Download **[latest ASF release](https://github.com/JustArchiNET/ArchiSteamFarm/releases/latest)** in generic variant.
- Extract the archive into new location (and `chmod +x ArchiSteamFarm.sh` if you're on Linux/OS X).
- **[設定 ASF](https://github.com/JustArchiNET/ArchiSteamFarm/wiki/Configuration)**
- Launch ASF by either using a helper script or executing `dotnet /path/to/ArchiSteamFarm.dll` manually from your favourite shell.

Helper scripts (such as `ArchiSteamFarm.cmd` for Windows and `ArchiSteamFarm.sh` for Linux/OS X) are located next to `ArchiSteamFarm.dll` binary - those are included in generic variant only. You can use them if you don't want to execute `dotnet` command manually. You can also make a shortcut to those scripts like showed above, since they're supposed to provide binary replacement in a script way. Obviously helper scripts won't work if you didn't install .NET Core SDK and you don't have `dotnet` executable available in your `PATH`. Helper scripts are entirely optional to use, you can always `dotnet /path/to/ArchiSteamFarm.dll` manually.