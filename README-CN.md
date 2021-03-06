## 目录
 - [说明](#说明)
 - [使用](#使用)
 - [FAQ](#faq)
 - [Windows 版本一览](#windows-builds-overview)
 - [进阶使用](#进阶使用)
 - [维护自己的分支](#维护自己的分支)
 - [代码贡献指南](#代码贡献指南)

&nbsp;

## 说明

这是一个用来在新安装 Windows 10 和 Windows Server 2016 / 2019 之后自动化完成日常配置任务的 Powershell 脚本。这不是任何完整的 Windows 调整设置合集，也不是另一种“反间谍”脚本。这只是一个我喜欢使用的设置让系统在我看来不那么讨厌。 

This is a PowerShell script for automation of routine tasks done after fresh installations of Windows 10 and Windows Server 2016 / 2019. This is by no means any complete set of all existing Windows tweaks and neither is it another "antispying" type of script. It's simply a setting which I like to use and which in my opinion make the system less obtrusive.

&nbsp;

## 使用
如果你只想用默认配置运行脚本，请下载并解压[最新版本的压缩包](https://github.com/Disassembler0/Win10-Initial-Setup-Script/releases)，然后双击 *Default.cmd* 文件并在*用户账户控制* 窗口提示中点击确认。在脚本尝试提权运行时确保您的账户是*管理员*组中的一员。

If you just want to run the script with the default preset, download and unpack the [latest release](https://github.com/Disassembler0/Win10-Initial-Setup-Script/releases) and then simply double-click on the *Default.cmd* file and confirm *User Account Control* prompt. Make sure your account is a member of *Administrators* group as the script attempts to run with elevated privileges.

这个脚本支持命令行选项和参数，这些选项和参数可以帮助您自定义您的调整选择。甚至您还可以添加您自定义的调整，但是这些需要一些命令行和 Powershell 脚本编写的基础知识。更多细节请参考[进阶使用](#进阶使用)部分。

The script supports command line options and parameters which can help you customize the tweak selection or even add your own custom tweaks, however these features require some basic knowledge of command line usage and PowerShell scripting. Refer to [Advanced usage](#advanced-usage) section for more details.

&nbsp;

## FAQ

**Q:** Can I run the script safely?  
**Q:** 我能安全地运行脚本吗?  
**A:** 绝对不是，你必须了解每个函数的作用和运行后对你的电脑产生的影响。有些函数会降低安全性，隐藏控制或者卸载程序 **如果你不确定脚本的作用，请不要运行它！**
**A:** Definitely not. You have to understand what the functions do and what will be the implications for you if you run them. Some functions lower security, hide controls or uninstall applications. **If you're not sure what the script does, do not attempt to run it!**

**Q:** Can I run the script repeatedly?  
**Q:** 我可以重复运行脚本吗?  
**A:** 当然可以！事实上这个脚本正是为了支持这个功能才编写的，因为每个大版本的 Windows 更新之后有些设置被重置并不少见。
**A:** Yes! In fact the script has been written to support exactly that, as it's not uncommon that big Windows Updates reset some of the settings.

**Q:** Which versions and editions of Windows are supported?  
**Q:** 支持什么版本的 Windows ?  
**A:** 脚本的目标是完全兼容从半年频道获取更新的最新的 64 位 Windows 10。但是如果您创建了您自己的预设并排除了不兼容的调整，它可能可以在 LTSB/LTSC 和 32 位系统上。绝大多数的调整可以在所有的 Windows 版本上生效。一些调整依赖于组策略设置，所以对家庭版和教育版可能有一些限制。
**A:** The script aims to be fully compatible with the most up-to-date 64bit version of Windows 10 receiving updates from semi-annual channel, however if you create your own preset and exclude the incompatible tweaks, it will work also on LTSB/LTSC and possibly also on 32bit systems. Vast majority of the tweaks will work on all Windows editions. Some of them rely on group policy settings, so there may be a few limitations for Home and Education editions.

**Q:** Can I run the script on Windows Server 2016 or 2019?  
**Q:** 我可以在 Windows Server 2016 或者 2019 上运行这个脚本吗?  
**A:** 是的。从 2.5 版本开始支持在 Windows Server 上运行，甚至还有一些特定针对服务器的调整。但是请记住，这个脚本主要还是为 Windows 10 设计的，所以你必须创建您自己的预设。
**A:** Yes. Starting from version 2.5, Windows Server is supported. There are even few tweaks specific to Server environment. Keep in mind though, that the script is still primarily designed for Windows 10, so you have to create your own preset.

**Q:** Can I run the script on Windows 7, 8, 8.1 or other versions of Windows?  
**Q:** 我可以在 Windows 7, 8, 8.1 或者其他版本的 Windows 上运行这个脚本吗?  
**A:** 不可以。尽管有些调整依旧可以在其他版本的 Windows 上生效，但是这个脚本仅为 Windows 10 和 Windows Server 2016 / 2019 编写的。没有任何计划为旧版本提供支持。
**A:** No. Although some tweaks may work also on older versions of Windows, the script is developed only for Windows 10 and Windows Server 2016 / 2019. There are no plans to support older versions.

**Q:** Can I run the script in multi-user environment?  
**Q:** 我可以在多用户环境中运行这个脚本吗?  
**A:** 是的，在一定程度上。一些调整（最主要的是 UI 调整）只针对当前执行脚本的用户生效。如上所述，脚本可以重复运行，因此你可以用不同的用户身份多次运行这个脚本使其在多用户下生效。由于 Windows 中的身份验证和提权机制，大部分的调整只能在属于*管理员*组的用户下生效。标准用户会收到 UAC 提示要求提供管理员凭证，导致调整在给定的管理员账户下生效而非当前的非特权标准用户。有一些方法可以在程序上规避这个问题，但是我不打算提供任何方法，因为这些方法会对代码的复杂性和可读性产生负面影响。如果你还想在多用户下尝试运行这个脚本，请参阅在[issue #29](https://github.com/Disassembler0/Win10-Initial-Setup-Script/issues/29#issuecomment-333040591)中的回答以获得一些提示。
**A:** Yes, to certain extent. Some tweaks (most notably UI tweaks) are set only for the user currently executing the script. As stated above, the script can be run repeatedly; therefore it's possible to run it multiple times, each time as different user. Due to the nature of authentication and privilege escalation mechanisms in Windows, most of the tweaks can be successfully applied only by users belonging to *Administrators* group. Standard users will get an UAC prompt asking for admin credentials which then causes the tweaks to be applied to the given admin account instead of the original non-privileged one. There are a few ways how this can be circumvented programmatically, but I'm not planning to include any as it would negatively impact code complexity and readability. If you still wish to try to use the script in multi-user environment, check [this answer in issue #29](https://github.com/Disassembler0/Win10-Initial-Setup-Script/issues/29#issuecomment-333040591) for some pointers.

**Q:** Did you test the script?  
**Q:** 你测试过这个脚本吗? 
**A:** 当然。我在虚拟机中的最新版本的 64 位 Windows 家庭版和企业版测试新增加的内容。在所有的大版本的更新之后，我也经常在我所有的家庭安装中使用它。
**A:** Yes. I'm testing new additions on up-to-date 64bit Home and Enterprise editions in VMs. I'm also regularly using it for all my home installations after all bigger updates.

**Q:** I've run the script and it did something I don't like, how can I undo it?  
**Q:** 我运行了脚本但是我不喜欢这些更改，我该如何撤销?  
**A:** 对于每个调整，也有相应的函数恢复默认设置。默认值是被认为是新安装的 Windows 10 或 Windows Server 2016，在安装过程中或安装后没有进行任何调整。使用调整函数来创建和运行新的预设。另外，由于有些函数只是自动化的动作，可以使用 GUI 来完成，所以找到相应的空间，手动修改即可。
**A:** For every tweak, there is also a corresponding function which restores the default settings. The default is considered freshly installed Windows 10 or Windows Server 2016 with no adjustments made during or after the installation. Use the tweaks to create and run new preset. Alternatively, since some functions are just automation for actions which can be done using GUI, find appropriate control and modify it manually.

**Q:** I've run the script and some controls are now greyed out and display message "*Some settings are hidden or managed by your organization*", why?  
**Q:** 我运行了这个脚本之后，现在有些控件是灰色的，并显示“某些设置被您的组织隐藏或管理”的消息，为什么？ 
**A:** 为了保证整个系统内何以顺利可靠地应用这些调整，有些调整利用了 *组策略*（*GPO*）。在公司大规模的管理计算机中也采用了同样的机制，所以没有管理权限的用户无法更改设置。如果你想改变被 GPO 锁定的设置，应用相应的还原调整，控件就可以恢复使用了。
**A:** To ensure that system-wide tweaks are applied smoothly and reliably, some of them make use of *Group Policy Objects* (*GPO*). The same mechanism is employed also in companies managing their computers in large scale, so the users without administrative privileges can't change the settings. If you wish to change a setting locked by GPO, apply the appropriate restore tweak and the control will become available again.

**Q:** 我运行了这个脚本然后它破坏了我的电脑、杀了我邻居的狗、引发了第三次世界大战。
**Q:** I've run the script and it broke my computer / killed neighbor's dog / caused world war 3.  
**A:** 我不关心这个问题。另外，这不是一个问题。
**A:** I don't care. Also, that's not a question.

**Q:** I'm using a tweak for &lt;feature&gt; on my installation, can you add it?  
**Q:** 在我的安装中我使用了一个调整，你能添加它吗?  
**A:** 提交一个 PR ,创建一个 feature request 的 issue 或者给我留言。如果我发现这个功能简单有用，而且不依赖任何第三方模块或者可执行文件（包括 *Chovolatey*，*NuGet*, *Ninite* 或者其他自动化解决方案），我有可能会添加它。
**A:** Submit a PR, create a feature request issue or drop me a message. If I find the functionality simple, useful and not dependent on any 3rd party modules or executables (including also *Chocolatey*, *NuGet*, *Ninite* or other automation solutions), I might add it.

**Q:** Can I use the script or modify it for my / my company's needs?  
**Q:** 我可以根据我或者我公司的需求使用或者修改这个脚本吗?  
**A:** 当然可以。请自便。别忘记了根据 MIT 许可要求加入版权声明。我还建议添加这个 GitHub repo 的链接,因为很有可能会有一些改变，添加或者改进以跟上 Windows 10 的未来版本。
**A:** Sure, knock yourself out. Just don't forget to include copyright notice as per MIT license requirements. I'd also suggest including a link to this GitHub repo as it's very likely that something will be changed, added or improved to keep track with future versions of Windows 10.

**Q:** Why are there repeated pieces of code throughout some functions?  
**Q:** 为什么一些函数中的代码是重复的？ 
**A:** 所以你可以直接从一个函数中取出一个函数块或者一行，然后在其他地方使用而不需要任何依赖。
**A:** So you can directly take a function block or a line from within a function and use it elsewhere, without elaborating on any dependencies.

**Q:** For how long are you going to maintain the script?  
**Q:** 你准备维护这个脚本多久？ 
**A:** 只要我还在使用 Windows 10 我就会维护它。
**A:** As long as I use Windows 10.

**Q:** I really like the script. Can I send a donation?  
**Q:** 我真的和喜欢这个脚本，我可以捐赠吗？  
**A:** 欢迎通过 [Paypal](https://www.paypal.me/Disassembler) 给我捐款，金额随意，但请记住捐赠完全是自愿的，无论捐款金额多少，我都不会为你做任何的脚本调整。你也可以给我的邮箱留言讨论其他方式。
**A:** Feel free to send donations via [PayPal](https://www.paypal.me/Disassembler). Any amount is appreciated, but keep in mind that donations are completely voluntary and I'm not obliged to make any script adjustments in your favor regardless of the donated amount. You can also drop me a mail to discuss an alternative way.

&nbsp;

## Windows 版本一览

| Version |        Code name        |     Marketing name     | Build |
| :-----: | ----------------------- | ---------------------- | :---: |
|  1507   | Threshold 1 (TH1 / RTM) | N/A                    | 10240 |
|  1511   | Threshold 2 (TH2)       | November Update        | 10586 |
|  1607   | Redstone 1 (RS1)        | Anniversary Update     | 14393 |
|  1703   | Redstone 2 (RS2)        | Creators Update        | 15063 |
|  1709   | Redstone 3 (RS3)        | Fall Creators Update   | 16299 |
|  1803   | Redstone 4 (RS4)        | April 2018 Update      | 17134 |
|  1809   | Redstone 5 (RS5)        | October 2018 Update    | 17763 |
|  1903   | 19H1                    | May 2019 Update        | 18362 |
|  1909   | 19H2                    | November 2019 Update   | 18363 |
|  2004   | 20H1                    | May 2020 Update        | 19041 |

&nbsp;

## 进阶使用

    powershell.exe -NoProfile -ExecutionPolicy Bypass -File Win10.ps1 [-include filename] [-preset filename] [-log logname] [[!]tweakname]

    -include filename       load module with user-defined tweaks
    -preset filename        load preset with tweak names to apply
    -log logname            save script output to a file
    tweakname               apply tweak with this particular name
    !tweakname              remove tweak with this particular name from selection

### 预设

这个调整库由独立的幂等函数组成，每个函数包含了一个调整。预设 *presets* 中包含了这些函数。预设只是一个被调用的函数名列表。任何在预设中没有出现或者被注释的函数都不会被调用，因此相应的调整也不会生效。你需要通过 `-include` 或者通过 `-preset` 作为命令行参数直接提供一个调整库和至少一个调整名。

The tweak library consists of separate idempotent functions, containing one tweak each. The functions can be grouped to *presets*. Preset is simply a list of function names which should be called. Any function which is not present or is commented in a preset will not be called, thus the corresponding tweak will not be applied. In order for the script to do something, you need to supply at least one tweak library via `-include` and at least one tweak name, either via `-preset` or directly as command line argument.

调整名称可以使用感叹号（`!`）作为前缀，这将导致调整从选择中删除。当你想应用整个预设但是在当前运行中省略一些特定的调整时，这很有用。另外，您也可以通过添加或删除少量的调整来“修补”另一个预设。

The tweak names can be prefixed with exclamation mark (`!`) which will instead cause the tweak to be removed from selection. This is useful in cases when you want to apply the whole preset, but omit a few specific tweaks in the current run. Alternatively, you can have a preset which "patches" another preset by adding and removing a small amount of tweaks.


要提供一个自定义的预设，您可以直接传递函数名字作为参数。

    powershell.exe -NoProfile -ExecutionPolicy Bypass -File Win10.ps1 -include Win10.psm1 EnableFirewall EnableDefender

或者你可以创建一个文件，里面包含函数名称（每行一个函数名，没有逗号或者引号，允许包含空格，注释以 `#` 开头），然后使用 `-preset` 参数传递文件名。
一个预设文件 `mypreset.txt` 的范例：

Or you can create a file where you write the function names (one function name per line, no commas or quotes, whitespaces allowed, comments starting with `#`) and then pass the filename using `-preset` parameter.  
Example of a preset file `mypreset.txt`:

    # Security tweaks
    EnableFirewall
    EnableDefender

    # UI tweaks
    ShowKnownExtensions
    ShowHiddenFiles   # Only hidden, not system

使用以上预设文件的命令行：
Command using the preset file above:

    powershell.exe -NoProfile -ExecutionPolicy Bypass -File Win10.ps1 -include Win10.psm1 -preset mypreset.txt

### Includes

脚本也支持通过 `-include` 参数从用户提供的模块中加入自定义调整。用户提供的模块内容完全由用户决定，但是强烈建议像主调整库一样，将调整分隔在各自的函数中。用户提供的脚本是通过 `Import-Module` 加载到主脚本中的，所以这个库最好应该是一个 `.psm1` 的 PowerShell 模块。
一个用户提供的调整库 `mytweaks.psm1` 的范例：

The script also supports inclusion of custom tweaks from user-supplied modules passed via `-include` parameter. The content of the user-supplied module is completely up to the user, however it is strongly recommended to have the tweaks separated in respective functions as the main tweak library has. The user-supplied scripts are loaded into the main script via `Import-Module`, so the library should ideally be a `.psm1` PowerShell module. 
Example of a user-supplied tweak library `mytweaks.psm1`:

```powershell
Function MyTweak1 {
    Write-Output "Running MyTweak1..."
    # Do something
}

Function MyTweak2 {
    Write-Output "Running MyTweak2..."
    # Do something else
}
```

Command using the script above:
使用以上脚本的命令行：

    powershell.exe -NoProfile -ExecutionPolicy Bypass -File Win10.ps1 -include mytweaks.psm1 MyTweak1 MyTweak2

### 组合使用

上述所有功能可以结合起来使用。你可以有一个包括原始脚本调整和你个人调整的预设文件。 `-include` 和 `-preset`
都可以多次使用，所以你可以把你的调整分成几组，然后根据你当前的需要进行组合。 `-include` 模块总是在第一个调整应用之前被导入，所以命令行参数的顺序并不重要，调整的顺序也不重要（除了 `RequireAdmin` 总是应该最先被调用和 `Restart` 总是最后被调用以外）。在一次运行中，可能会发生一些调整被应用了不止一次，因为你把它们放在多个预设中。这不应该引起任何问题，因为这些调整都是幂等的(即多次执行的结果是一样的)。
一个预设文件 `otherpreset.txt` 的范例：

All features described above can be combined. You can have a preset which includes both tweaks from the original script and your personal ones. Both `-include` and `-preset` options can be used more than once, so you can split your tweaks into groups and then combine them based on your current needs. The `-include` modules are always imported before the first tweak is applied, so the order of the command line parameters doesn't matter and neither does the order of the tweaks (except for `RequireAdmin`, which should always be called first and `Restart`, which should be always called last). It can happen that some tweaks are applied more than once during a singe run because you have them in multiple presets. That shouldn't cause any problems as the tweaks are idempotent.  
Example of a preset file `otherpreset.txt`:

    MyTweak1
    MyTweak2
    !ShowHiddenFiles   # Will remove the tweak from selection
    WaitForKey

组合使用以上三个范例的命令行：
Command using all three examples combined:

    powershell.exe -NoProfile -ExecutionPolicy Bypass -File Win10.ps1 -include Win10.psm1 -include mytweaks.psm1 -preset mypreset.txt -preset otherpreset.txt Restart

&nbsp;

### 记录日志

如果你想要保存你脚本执行的的输出日志，你可以使用 `-log` 参数后面跟随一个你想创建的日志文件名实现。举个例子：

If you'd like to store output from the script execution, you can do so using `-log` parameter followed by a filename of the log file you want to create. For example:

    powershell.exe -NoProfile -ExecutionPolicy Bypass -File Win10.ps1 -include Win10.psm1 -preset mypreset.txt -log myoutput.log

日志记录使用 PowerShell 的 `Start-Transcript` 命令完成。它将有关当前环境的额外信息（日期，机器名和用户名、用于执行的命令等）写入文件开头，并记录标准输出和标准错误流。

The logging is done using PowerShell `Start-Transcript` cmdlet, which writes extra information about current environment (date, machine and user name, command used for execution etc.) to the beginning of the file and logs both standard output and standard error streams.

## 维护自己的分支

自定义脚本设置最简单的方法是创建你自己的预设，如上所述，如果需要你可以自己调整脚本。为了方便开始入手，你可以基于 *Default.cmd* 和 *Default.preset* 进行修改并只维护这两个文件。如果你还是选择，你不需要注释或删除 *Win10.psm1* 中的实际函数，因为如果不调用它们，它们就不会被使用。
The easiest way to customize the script settings it is to create your own preset and, if needed, your own tweak scripts as described above. For easy start, you can base the modifications on the *Default.cmd* and *Default.preset* and maintain just that. If you choose to fork the script anyway, you don't need to comment or remove the actual functions in *Win10.psm1*, because if they are not called, they are not used.

如果你想对基本的脚本进行更加细致的修改，并加入一些个人的调整，那么我建议用以下的方式来做：
If you wish to make more elaborate modifications of the basic script and incorporate some personal tweaks or adjustments, then I suggest doing it in a following way:

1. 在 GitHub 上 fork 这个 repo （显然的）
2. 在你的电脑上克隆你的 fork 
    
    ```
    git clone https://github.com/<yournamehere>/Win10-Initial-Setup-Script
    cd Win10-Initial-Setup-Script
    ```

3. 添加原来的 repo 作为 remote （*上游*）
4. 提交你认为合适的修改
5. 一旦上游有新增加的内容，就创建一个临时分支，获取变化并将该分支重置为本仓库相同
   
    ```
    git branch upstream
    git checkout upstream
    git fetch upstream
    git reset --hard upstream/master
    ```

6. 当你拥有最新的上游分支时，请检查你的主分支并根据基于上游分支重新构建。如果变化集之中有冲突，你会被要求手动解决它们。

    ```
    git checkout master
    git rebase upstream
    ```

7. 最终，删除上游分支并将你的变更强制推送回 GitHub 上。

    ```
    git branch -D upstream
    git push -f master
    ```

**警告：** 基于上游重新构建和强制推送会改变你的提交历史。好处是你的调整将始终保持在提交历史的顶部，缺点是所有远程跟踪你的版本库的人也都必须重新构建和强制推送，否则他们的提交历史将与你的历史不符。


&nbsp;

## 代码贡献指南

Following is a list of rules which I'm trying to apply in this project. The rules are not binding and I accept pull requests even if they don't adhere to them, as long as their purpose and content are clear. In cases when there are too many rule violations, I might simply redo the whole functionality and reject the PR while still crediting you. If you'd like to make my work easier, please consider adhering to the following rules too.

### Function naming
Try to give a function a meaningful name up to 25 characters long, which gives away the purpose of the function. Use verbs like `Enable`/`Disable`, `Show`/`Hide`, `Install`/`Uninstall`, `Add`/`Remove` in the beginning of the function name. In case the function doesn't fit any of these verbs, come up with another name, beginning with the verb `Set`, which indicates what the function does, e.g. `SetCurrentNetworkPrivate` and `SetCurrentNetworkPublic`.

### Revert functions
Always add a function with opposite name (or equivalent) which reverts the behavior to default. The default is considered freshly installed Windows 10 or Windows Server 2016 / 2019 with no adjustments made during or after the installation. If you don't have access to either of these, create the revert function to the best of your knowledge and I will fill in the rest if necessary.

### Function similarities
Check if there isn't already a function with similar purpose as the one you're trying to add. As long as the name and objective of the existing function is unchanged, feel free to add your tweak to that function rather than creating a new one.

### Function grouping
Try to group functions thematically. There are already several major groups (privacy, security, services etc.), but even within these, some tweaks may be related to each other. In such case, add a new tweak below the existing one and not to the end of the whole group.

### Default preset
Always add a reference to the tweak and its revert function in the *Default.preset*. Add references to both functions on the same line (mind the spaces) and always comment out the revert function. Whether to comment out also the tweak in the default preset is a matter of personal preference. The rule of thumb is that if the tweak makes the system faster, smoother, more secure and less obtrusive, it should be enabled by default. Usability has preference over performance (that's why e.g. indexing is kept enabled).

### Repeatability
Unless applied on unsupported system, all functions have to be applicable repeatedly without any errors. When you're creating a registry key, always check first if the key doesn't happen to already exist. When you're deleting registry value, always append `-ErrorAction SilentlyContinue` to prevent errors while deleting already deleted values.

### Input / output hiding
Suppress all output generated by commands and cmdlets using `| Out-Null` or `-ErrorAction SilentlyContinue` where applicable. Whenever an input is needed, use appropriate arguments to suppress the prompt and programmatically provide values for the command to run (e.g. using `-Confirm:$false`). The only acceptable output is from the `Write-Output` cmdlets in the beginning of each function and from non-suppressible cmdlets like `Remove-AppxPackage`.

### Registry
Create the registry keys only if they don't exist on fresh installation if Windows 10 or Windows Server 2016 / 2019. When deleting registry, delete only registry values, not the whole keys. When you're setting registry values, always use `Set-ItemProperty` instead of `New-ItemProperty`. When you're removing registry values, choose either `Set-ItemProperty` or `Remove-ItemProperty` to reinstate the same situation as it was on the clean installation. Again, if you don't know what the original state was, let me know in PR description and I will fill in the gaps. When you need to use `HKEY_USERS` registry hive, always add following snippet before the registry modification to ensure portability.

```powershell
If (!(Test-Path "HKU:")) {
    New-PSDrive -Name HKU -PSProvider Registry -Root HKEY_USERS | Out-Null
}
```

### Force usage
Star Wars jokes aside, don't use `-Force` option unless absolutely necessary. The only permitted case is when you're creating a new registry key (not a value) and you need to ensure that all parent keys will be created as well. In such case always check first if the key doesn't already exist, otherwise you will delete all its existing values.

### Comments
Always add a simple comment above the function briefly describing what the function does, especially if it has an ambiguous name or if there is some logic hidden under the hood. If you know that the tweak doesn't work on some editions of Windows 10 or on Windows Server, state it in the comment too. Add a `Write-Output` cmdlet with the short description of action also to the first line of the function body, so the user can see what is being executed and which function is the problematic one whenever an error occurs. The comment is written in present simple tense, the `Write-Output` in present continuous with ellipsis (resp. three dots) at the end.

### Coding style
Indent using tabs, enclose all string values in double quotes (`"`) and strictly use `PascalCase` wherever possible. Put opening curly bracket on the same line as the function name or condition, but leave the closing bracket on a separate line for readability.

### Examples

**Naming example**: Consider function `EnableFastMenu`. What does it do? Which menu? How fast is *fast*? A better name might be `EnableFastMenuFlyout`, so it's a bit clearer that we're talking about the menu flyouts delays. But the counterpart function would be `DisableFastMenuFlyouts` which is not entirely true. We're not *disabling* anything, we're just making it slow again. So even better might be to name them `SetFastMenuFlyouts` and `SetSlowMenuFlyouts`. Or better yet, just add the functionality to already existing `SetVisualFXPerformance`/`SetVisualFXAppearance`. Even though the names are not 100% match, they aim to tweak similar aspects and operate within the same registry keys.

**Coding example:** The following code applies most of the rules mentioned above (naming, output hiding, repeatability, force usage, comments and coding style).

```powershell
# Enable some feature
Function EnableSomeFeature {
    Write-Output "Enabling some feature..."
    If (!(Test-Path "HKLM:\Some\Registry\Key")) {
        New-Item -Path "HKLM:\Some\Registry\Key" -Force | Out-Null
    }
    Set-ItemProperty -Path "HKLM:\Some\Registry\Key" -Name "SomeValueName" -Type String -Value "SomeValue"
}

# Disable some feature
Function DisableSomeFeature {
    Write-Output "Disabling some feature..."
    Remove-ItemProperty -Path "HKLM:\Some\Registry\Key" -Name "SomeValueName" -ErrorAction SilentlyContinue
}
```
