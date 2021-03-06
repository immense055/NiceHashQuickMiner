# NiceHash QuickMiner

Please use [Wiki](https://github.com/nicehash/NiceHashQuickMiner/wiki) (still work in progress but contains **most** of information you are after).

NiceHash QuickMiner is a very simple, easy and quick way to start mining and earning Bitcoins on PC with Windows 10 x64. It has following features:
* It uses ONLY [Excavator for GPU mining](https://github.com/nicehash/excavator) and XMRig for CPU mining. Excavator is in-house developed miner and is 100% free of any malware and [XMRig is open source CPU miner](https://github.com/xmrig/xmrig). There are some more open source C# libraries used. Code running as NiceHash QuickMiner is either developed by NiceHash or used from public repositories which means it is 100% safe and you do not have to worry about exposing your PC's sensitive data or getting infected with malware. NiceHash QuickMiner will (in the future) NEVER use any closed source software, software of unknown or unverified origin.
* No benchmarks, you start mining IMMEDIATELY.
* No third party tools for GPU overclocking needed. NiceHash QuickMiner has its own powerful and quick tool for setting overclocks and fan speeds. Determine best overclock easily and quickly as never before.
* Watchdog which takes care of restarting mining process if something fails.
* Windows autostart methods.
* Automatic updates.
* No developer fees. There is only NiceHash 2% pool-side fee with PPS (pay-per-share) payout scheme.

**URGENT NOTICE! NiceHash QuickMiner is currently in BETA testing. Anyone can participate. Please, submit bugs [here](https://github.com/nicehash/NiceHashQuickMiner/issues).**

Supported hardware:
* NVIDIA GeForce GTX 1000 series with min. 6 GB of GDDRAM,
* NVIDIA GeForce RTX 2000 series with min. 6 GB of GDDRAM,
* NVIDIA GeForce RTX 3000 series with min. 6 GB of GDDRAM,
* Intel CPUs with AVX2,
* AMD CPUs with AVX2.

NiceHash QuickMiner contains latest version of [Excavator](https://github.com/nicehash/excavator) bundled with:
* watchdog (NiceHashQuickMiner.exe),
* autostart service (nhqmservice.exe) and
* OCTune.

# Download

Download latest version from release page: https://github.com/nicehash/NiceHashQuickMiner/releases

Stable versions are marked with `stable`. `RC` versions are Release-Candidate versions (not considered stable).

# Installation
Simply extract all files in .zip archive into any folder you want.

# How to run
Simply double click `NiceHashQuickMiner.exe` and mining process will start. You have to change mining address (to your NiceHash Mining address) - you can do this by modifying `nhqm.conf` file. Besides configuring your BTC mining address, you may want to modify following:
* serviceLocation (0 is eu, 1 is usa),
* workerName (name of your rig),
* launchCommandLine (extra command line options) and
* consoleLogLevel & fileLogLevel (logging options).

**_Please, have fileLogLevel set to 0 at all times and submit all the errors and issues you find using this software bundle. Thanks!_**

Some extra features are available in your Windows tray (notification area); right click NiceHash icon and you can add/remove autostart task or service. By enabling autostart, NiceHash QuickMiner will start with Windows automatically. NiceHash QuickMiner is bundled with CPU miner - XMRig. CPU mining is possible on CPUs that support AVX2 instruction set.

# Uninstallation
Delete all files. If you have added autostart, make sure to disable autostart before you delete all files.
Alternative way; run `NiceHashQuickMiner.exe --uninstall` and all files and folders will be purged from your PC automatically. If you also add `--keepconfig`, then config files will be kept.

# Recommendations
Also use OCTune; run `octune.html` when Excavator is running. You will get a (not-so-good-looking) interface to manage Excavator over your web browser. You can play with overclock settings. Once you are happy with your settings, just save everything by clicking on the button `Save current configuration`. Next time Excavator is started, your saved configuration will be used. You do not have to modify `commands.json` file manually anymore.

# FAQ
_1. Why is NiceHashQuickMiner started with administrator privileges?_

Administrator privileges are needed for overclocking. MSI Afterburner (which you'd not need anymore) is started with administrator privileges aswell. Since up to 20% extra performance is possible to achieve with up to 50% reduced power load, we do not believe it is smart to mine without adjusting clocks and power limits which require administrator privileges.

_2. What happens when DAG is being generated?_

Excavator would set memory overclock to 0 for the time DAG is being generated. This is done to prevent generation of corrupted DAG which causes all future shares to be invalid. Note that this feature only works if you set your overclock with Excavator and it DOES NOT work when using MSI Afterburner for overclocking. With this feature you can clock your card higher and do not need to worry about corrupted DAGs because these cannot happen anymore.

_3. Where can I see number of accepted and rejects shares?_

You can see number of accepted and rejected shares by calling [API method algorithm.list](https://github.com/nicehash/excavator/tree/master/api#algorithm-list). Note that, to the contrary of other PPLNS pools, for NiceHash, these values are not important. The reason is, because each share has a certain value that may not be the same. NiceHash does not have a fixed difficulty but rather dynamic. Higher difficulty shares have higher value. Since NiceHash has a PPS payout scheme (pay-per-share), it is very important to know the value of the share (share at twice the difficulty is worth twice as much BTC). If you chart down shares with their values, you get accepted/rejected speed. These charts are already available at NiceHash - Rig Manager. Other pools often display accepted speed on their charts as the value that the miner is reporting to the pool - and this value can be cheated-out (sending some extreme large value for example). NiceHash does not support speed reported by miner, rather it calculates accepted/rejected speed out of your shares. Thus, contrary to the other pools, these charts have very high value as they represent direct performance of your miner and your mining payouts are based directly on that.

_4. Why do some jobs have "clean" suffix?_

When job has "clean" suffix, it means that miner needs to drop current job **immediately** and start working on the new job - all previous work is not valid anymore and would result in a rejected share (job not found - stale share). One of the miner qualities is defined by the amount of how much time it takes for it to drop current job and start working on the new one. The time in between receiving new clean job and start working on the new job is wasted as these found shares **always** result in rejection as stale shares. This quality is not visible by the reported hashing speed but rather as a calculated speed on server side (NiceHash - accepted speed -> your actual profitability) or rather amount of stale shares (also your high latency to server increase amount of stale shares). Excavator has this switching time in between 1-3 milliseconds on modern CPUs of latest generation (Intel 10th gen, AMD Ryzen, Threadripper). It can be also observed and calculated from logs (when full detailed logging with `-f 0` is enabled). This task is not so simple to optimize, because NVIDIA kernel launches are non preemptive, which means, once kernel is started, it cannot be terminated. Unfortunately, time-short kernel result in reduced speed because time is wasted managing kernel launch and finish. So, usually a certain balance is needed so that kernels do not take too long and cause a lot of wasted tame when jobs are switched and that also don't last too short time to waste mining time due to excessive kernel launch management. Of course, any miner developer can use various tricks to solve this issue and it is not only about tweaking these two values. Anyway... this is off topic now already. Back to "clean" jobs.

When job is **not** "clean" it means that miner **does not** have to drop current job, because previous job is still valid. Therefore when you see a job that isn't "clean" you know that it won't reduce your miner speed consequently. Excavator is made to simply ignore non-clean jobs (these only get printed out, but that's all).


# Troubleshooting

**_Error #102_** Excavator.exe is missing. Most likely your Antivirus (AV) or Microsoft Defender has deleted it. Make sure to add exception for following files:
* excavator.exe
* NiceHashQuickMiner.exe
* nhqmservice.exe
* xmrig.exe

**_Warning #103_** Your PC does not have enough Virtual Memory set. This can cause mining failure. Increase your Virtual Memory size to at least 6000 MB times number of GPUs your PC has. Read [here](https://www.nicehash.com/blog/post/how-to-increase-virtual-memory-on-windows) how to increase Virtual Memory size.

# Tips & Tricks

**Finding most efficient or the fastest combination of OC values for your card(s)**

Use OCTune. Look for values `Min KT` and `Avg KT`. Your goal is to get these values as low as possible. What is the meaning of these values? KT = Kernel Time. It is execution time of code running on GPU in microseconds. Min = minimal time (in previous X runs) and Avg = average time. Min can sometimes drop down to half of usual - you should ignore these values.

_1. Find your max memory clock_
First, you should find your max memory speed or determine at what memory speed you are prepared to run your GPU. Higher power limit and lower core clock can help memory stability. Therefore, set power limit to some high value (can be at 100% of your TDP - note the value needs to be in Watts!) and core clock to -600. Then increase memory clock by 25 each step and test at least several minutes. There should be no `HW err` and make sure there are at least 3 accepted shares on that card (`HW ok` increases by 3 at that speed). Once you find your max memory clock, you can go to step 2.

_2. Reduce power limit_
Now you can start reducing power limit. At some point, `Min KT` and `Avg KT` will start rising. That is a sign that you need to back off and stop reducing power limit (temporary). Now you need to increase core clock. Go to step 3.

_3. Increase core clock_
Increase core clock +25 at a time until your `Min KT` and `Avg KT` stop improving (values do not go lower anymore). Note that higher core clock can affect stability of your memory. You may have to decrease your memory clock if there are rejected shares (share above target) and value in `HW err` increases. Once you find ideal core clock, you can again try with reducing power limit.

_4. Play with settings to achieve best KT values_
You have to fiddle with core clock, memory clock and power limit until you find best KT values (lowest) and max stability (no shares above target - `HW err` stays at 0). Once you find best values, you will have the highest possible speed. You can save your OC configuration so it will be applied next time Excavator is started.


**Larger rigs - risers and BIOS settings**

When you are running cards over risers, this adds extra instability factor to your configuration. You need to make sure your risers (comm link between CPU and GPU) are max stable. To achieve this, you need to set PCI Generation to 1 in BIOS (or at least 2). Having higher Generation introduce more instability because communication between CPU and GPU is faster and there are more chances for something to go wrong. There is no speed penalty when running Gen1 - your cards will hash at the same speed.

If one of your cards crashes during mining and you see device `ERROR` it most likely means riser instability. This is especially true, if it happens when your card is not overclocked. In this case replace riser, cable and set PCIE gen1 in BIOS.


**Fine tune crash/error case scenarios**

NiceHash QuickMiner can detect two possible erroneous device states:
* when speed is abnormally high and
* when speed falls to 0.

For both cases, you can choose among three options how NiceHash QuickMiner should act:
* do nothing (value = 1),
* restart Excavator (value = 2) or
* restart whole rig (value = 3).

You can fine tune these values by adjusting values `whenDeviceSpeedTooHigh` and `whenDeviceSpeedZero` in _nhqm.conf_ file. In some cases, you may prefer to do simple restart of Excavator. In some cases, restart of Excavator is not enough and whole rig needs to be restarted. And some of you may deal with this case manually each time. For that purpose, error state of the device is displayed in NiceHash Rig Manager:

![Rig Manager - Device Error](https://github.com/nicehash/NiceHashQuickMiner/blob/main/images/error.png?raw=true)


# Additional information - 3rd party libraries and code used
* C# .NET library log4net: https://logging.apache.org/log4net/index.html
* C# .NET library managedCuda: http://kunzmi.github.io/managedCuda
* C# .NET library Microsoft.Win32.TaskScheduler: https://github.com/dahall/TaskScheduler#main-library
* C# .NET library websocket-sharp: https://github.com/sta/websocket-sharp
* XMRig miner (https://github.com/xmrig/xmrig); we have compiled version 6.7.0 with some modifications. The miner is stored in Excavator and it is extracted and launched when needed for CPU Mining.
* XMRig miner uses signed driver WinRing0x64.sys. This driver has been signed on Saturday, 26 July 2008 by Noriyuki MIYAZAKI. We do not have source for it, however, we believe that this driver pose no security threat at all as it has good reputation by Microsoft and has been used by various software for almost 13 years already.
