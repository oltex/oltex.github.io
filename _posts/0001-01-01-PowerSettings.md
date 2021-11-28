---
title: "PowerSettings"

categories:
  - registry
tags:
  - tag
---

경로
HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Control\Power\PowerSettings

# Hard disk
Specify power management settings for your hard disk. Group GUID: `0012ee47-9041-4b5d-9b77-535fba8b1442`

* AHCI Link Power Management - HIPM/DIPM
  * GUID: `0b2d69d7-a2a1-449c-9680-f91c70521c60`
  * Configures the LPM state.
  * Possible values: Active, HIPM, HIPM+DIPM, DIPM, Lowest
* Maximum Power Level
  * GUID: `51dea550-bb38-4bc4-991b-eacf37be5ec8`
  * Specifies the the power consumption level storage devices should not exceed.
  * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
* Turn off hard disk after
  * GUID: `6738e2c4-e8a5-4a42-b16a-e040e769756e`
  * Specify how long your hard drive is inactive before the disk turns off.
  * ValueMax: 4294967295 Seconds | ValueMin: 0 Seconds | ValueIncrement: 1 Seconds
* Hard disk burst ignore time
  * GUID: `80e3c60e-bb94-4ad8-bbe0-0d3195efc663`
  * Ignore a burst of disk activity up to the specified time when determining if the disk is idle.
  * ValueMax: 4294967295 Seconds | ValueMin: 0 Seconds | ValueIncrement: 1 Seconds
* NVMe Idle Timeout
  * GUID: `d639518a-e56d-4345-8af2-b9f32fb26109`
  * Specifies the amount of time the NVMe device must be idle before transitioning to a non-operational power state.
  * ValueMax: 60000 milliseconds | ValueMin: 0 milliseconds | ValueIncrement: 1 milliseconds
* AHCI Link Power Management - Adaptive
  * GUID: `dab60367-53fe-4fbc-825e-521d069d2456`
  * Automatically transit from Partial to Slumber.
  * ValueMax: 300000 millisecond | ValueMin: 0 millisecond | ValueIncrement: 1 millisecond
* NVMe Power State Transition Latency Tolerance
  * GUID: `fc95af4d-40e7-4b6d-835a-56d131dbc80e`
  * When the NVMe device has been idle for a certain amount of time, transition to the lowest non-operational power state whose ENLAT+EXLAT value is less than or equal to the value specified by this setting.
  * ValueMax: 60000 milliseconds | ValueMin: 0 milliseconds | ValueIncrement: 1 milliseconds

# Internet Explorer
> Specify Internet Explorer power management settings. Group GUID: `02f815b5-a5cf-4c84-bf20-649d1f75d3d8`
> 
> * JavaScript Timer Frequency
>   
>   * GUID: `4c793e7d-a264-42e1-87d3-7a0d2f523ccd`
>   * Possible values: Maximum Power Savings, Maximum Performance
> 
> # Desktop background settings
> Change power management settings for your desktop background. Group GUID: `0d7dbae2-4294-402a-ba8e-26777e8488cd`
> 
> * Slide show
>   
>   * GUID: `309dce9b-bef4-4119-9921-a851fb12f0f4`
>   * Specify when you want the desktop background slide show to be available.
>   * Possible values: Available, Paused
> 
> # Wireless Adapter Settings
> Configure wireless adapter power settings. Group GUID: `19cbb8fa-5279-450e-9fac-8a3d5fedd0c1`
> 
> * Power Saving Mode
>   
>   * GUID: `12bbebe6-58d6-4636-95bb-3217ef867c1a`
>   * Control the power saving mode of wireless adapters.
>   * Possible values: Maximum Performance, Low Power Saving, Medium Power Saving, Maximum Power Saving
> 
> # Sleep
> Specify sleep settings. Group GUID: `238c9fa8-0aad-41ed-83f4-97be242c8f20`
> 
> * Allow Away Mode Policy
>   
>   * GUID: `25dfa149-5dd1-4736-b5ab-e8a37b5b8187`
>   * Allow away mode to be enabled for your computer
>   * Possible values: No, Yes
> * Sleep after
>   
>   * GUID: `29f6c1db-86da-48c5-9fdb-f2b67b1f44da`
>   * Specify how long your computer is inactive before going to sleep.
>   * ValueMax: 4294967295 Seconds | ValueMin: 0 Seconds | ValueIncrement: 1 Seconds
> * System unattended sleep timeout
>   
>   * GUID: `7bc4a2f9-d8fc-4469-b07b-33eb785aaca0`
>   * Idle timeout before the system returns to a low power sleep state after waking unattended.
>   * ValueMax: 4294967295 Seconds | ValueMin: 0 Seconds | ValueIncrement: 1 Seconds
> * Allow hybrid sleep
>   
>   * GUID: `94ac6d29-73ce-41a6-809f-6363ba21b47e`
>   * Allow Windows to save your work and enter a low-power state so that you can resume working almost immediately.
>   * Possible values: Off, On
> * Hibernate after
>   
>   * GUID: `9d7815a6-7ee4-497e-8888-515a05f02364`
>   * Specify how long your computer is inactive before hibernating.
>   * ValueMax: 4294967295 Seconds | ValueMin: 0 Seconds | ValueIncrement: 1 Seconds
> * Allow system required policy
>   
>   * GUID: `a4b195f5-8225-47d8-8012-9d41369786e2`
>   * Allow programs to prevent machine from going to sleep automatically
>   * Possible values: No, Yes
> * Allow Standby States
>   
>   * GUID: `abfc2519-3608-4c2a-94ea-171b0ed546ab`
>   * Allow Windows to use the standby states when sleeping your computer.
>   * Possible values: Off, On
> * Allow wake timers
>   
>   * GUID: `bd3b718a-0680-4d9d-8ab2-e1d2b4ac806d`
>   * Specify if timed events should be allowed to wake the computer from sleep.
>   * Possible values: Disable, Enable, Important Wake Timers Only
> * Allow sleep with remote opens
>   
>   * GUID: `d4c1d4c8-d5cc-43d3-b83e-fc51215cb04d`
>   * Allow your machine to go to sleep when files opened remotely have not been written to.
>   * Possible values: Off, On
> 
> # USB settings
> Specify USB power settings for the USB hub driver Group GUID: `2a737441-1930-4402-8d77-b2bebba308a3`
> 
> * Hub Selective Suspend Timeout
>   
>   * GUID: `0853a681-27c8-4100-a2fd-82013e970683`
>   * This value will be used as idle timeouts for all USB hubs
>   * ValueMax: 100000 Millisecond | ValueMin: 0 Millisecond | ValueIncrement: 1 Millisecond
> * USB selective suspend setting
>   
>   * GUID: `48e6b7a6-50f5-4782-a5d4-53bb8f07e226`
>   * Specify whether USB selective suspend is turned on or off
>   * Possible values: Disabled, Enabled
> * Setting IOC on all TDs
>   
>   * GUID: `498c044a-201b-4631-a522-5c744ed4e678`
>   * Should IOC be set for all TDs
>   * Possible values: Disabled, Enabled
> * USB 3 Link Power Mangement
>   
>   * GUID: `d4e98f31-5ffe-4ce1-be31-1b38b384c009`
>   * Specifies the power management policy to use for USB 3 links when they are idle
>   * Possible values: Off, Minimum power savings, Moderate power savings, Maximum power savings
> 
> # Idle Resiliency
> Idle resiliency settings. Group GUID: `2e601130-5351-4d9d-8e04-252966bad054`
> 
> * Execution Required power request timeout
>   
>   * GUID: `3166bc41-7e98-4e03-b34e-ec0f5f2b218e`
>   * Specifies Execution Required power request timeout
>   * ValueMax: 4294967295 Seconds | ValueMin: 0 Seconds | ValueIncrement: 1 Seconds
> * IO coalescing timeout
>   
>   * GUID: `c36f0eb4-2988-4a70-8eee-0884fc2c2433`
>   * Specifies IO coalescing timeout
>   * ValueMax: 4294967295 Milliseconds | ValueMin: 0 Milliseconds | ValueIncrement: 1 Milliseconds
> * Processor Idle Resiliency Timer Resolution
>   
>   * GUID: `c42b79aa-aa3a-484b-a98f-2cf32aa90a28`
>   * Specifies Processor Idle Resiliency Timer Resolution
>   * ValueMax: 65000 Milliseconds | ValueMin: 0 Milliseconds | ValueIncrement: 1 Milliseconds
> * Deep Sleep Enabled/Disabled
>   
>   * GUID: `d502f7ee-1dc7-4efd-a55d-f04b6f5c0545`
>   * Specifies if Deep Sleep is Enabled
>   * Possible values: Deep Sleep Disabled, Deep Sleep Enabled
> 
> # Interrupt Steering Settings
> Interrupt Steering Settings Group GUID: `48672f38-7a9a-4bb2-8bf8-3d85be19de4e`
> 
> * Interrupt Steering Mode
>   
>   * GUID: `2bfc24f9-5ea2-4801-8213-3dbae01aa39d`
>   * Interrupt Steering Mode
>   * Possible values: Default, Any processor, Any unparked processor with time delay, Any unparked processor, Lock Interrupt Routing, Processor 0, Processor 1
> * Target Load
>   
>   * GUID: `73cde64d-d720-4bb2-a860-c755afe77ef2`
>   * Target Load for each Processor
>   * ValueMax: 10000 Tenths of a percent | ValueMin: 0 Tenths of a percent | ValueIncrement: 1 Tenths of a percent
> * Unparked time trigger
>   
>   * GUID: `d6ba4903-386f-4c2c-8adb-5c21b3328d25`
>   * Time a processor must remain unparked before interrupts are moved onto it
>   * ValueMax: 100000 Milliseconds | ValueMin: 0 Milliseconds | ValueIncrement: 1 Milliseconds
> 
> # Power buttons and lid
> Specify what your computer does when you close the lid and press the power buttons. Group GUID: `4f971e89-eebd-4455-a8de-9e59040e7347`
> 
> * Lid close action
>   
>   * GUID: `5ca83367-6e45-459f-a27b-476b1d01c936`
>   * Specify the action that your computer takes when you close the lid on your mobile PC.
>   * Possible values: Do nothing, Sleep, Hibernate, Shut down
> * Power button action
>   
>   * GUID: `7648efa3-dd9c-4e3e-b566-50f929386280`
>   * Specify the action to take when you press the power button.
>   * Possible values: Do nothing, Sleep, Hibernate, Shut down, Turn off the display
> * Enable forced button/lid shutdown
>   
>   * GUID: `833a6b62-dfa4-46d1-82f8-e09e34d029d6`
>   * Enable forced shutdown for button and lid actions
>   * Possible values: Off, On
> * Sleep button action
>   
>   * GUID: `96996bc0-ad50-47ec-923b-6f41874dd9eb`
>   * Specify the action to take when you press the sleep button.
>   * Possible values: Do nothing, Sleep, Hibernate, Shut down, Turn off the display
> * Lid open action
>   
>   * GUID: `99ff10e7-23b1-4c07-a9d1-5c3206d741b4`
>   * Specify the action to take when the lid is opened.
>   * Possible values: Do nothing, Turn on the display
> * Start menu power button
>   
>   * GUID: `a7066653-8d6c-40a8-910e-a1f54b84c7e5`
>   * Specify the action to take when you press the Start menu power button.
>   * Possible values: Sleep, Hibernate, Shut down
> 
> # PCI Express
> PCI Express Power Management Settings Group GUID: `501a4d13-42af-4429-9fd1-a8218c268e20`
> 
> * Link State Power Management
>   
>   * GUID: `ee12f906-d277-404b-b6da-e5fa1a576df5`
>   * Specifies the Active State Power Management (ASPM) policy to use for capable links when the link is idle.
>   * Possible values: Off, Moderate power savings, Maximum power savings
> 
> # Processor power management
> Specify power management settings for your computer’s processor. Group GUID: `54533251-82be-4824-96c1-47b60b740d00`
> 
> * Processor performance increase threshold
>   
>   * GUID: `06cadf0e-64ed-448a-8927-ce7bf90eb35d`
>   * Specify the upper busy threshold that must be met before increasing the processor's performance state (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance increase threshold for Processor Power Efficiency Class 1
>   
>   * GUID: `06cadf0e-64ed-448a-8927-ce7bf90eb35e`
>   * Specify the upper busy threshold that must be met before increasing the processor's performance state (in percentage) for Processor Power Efficiency Class 1.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance core parking min cores
>   
>   * GUID: `0cc5b647-c1df-4637-891a-dec35c318583`
>   * Specify the minimum number of unparked cores/packages allowed (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance core parking min cores for Processor Power Efficiency Class 1
>   
>   * GUID: `0cc5b647-c1df-4637-891a-dec35c318584`
>   * Specify the minimum number of unparked cores/packages allowed for Processor Power Efficiency Class 1 (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance decrease threshold
>   
>   * GUID: `12a0ab44-fe28-4fa9-b3bd-4b64f44960a6`
>   * Specify the lower busy threshold that must be met before decreasing the processor's performance state (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance decrease threshold for Processor Power Efficiency Class 1
>   
>   * GUID: `12a0ab44-fe28-4fa9-b3bd-4b64f44960a7`
>   * Specify the lower busy threshold that must be met before decreasing the processor's performance state (in percentage) for Processor Power Efficiency Class 1.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Initial performance for Processor Power Efficiency Class 1 when unparked
>   
>   * GUID: `1facfc65-a930-4bc5-9f38-504ec097bbc0`
>   * Initial performance state for Processor Power Efficiency Class 1 when woken from a parked state.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance core parking concurrency threshold
>   
>   * GUID: `2430ab6f-a520-44a2-9601-f7f23b5134b1`
>   * Specify the busy threshold that must be met when calculating the concurrency of a node.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance core parking increase time
>   
>   * GUID: `2ddd5a84-5a71-437e-912a-db0b8c788732`
>   * Specify the minimum number of perf check intervals that must elapse before more cores/packages can be unparked.
>   * ValueMax: 100 Time check intervals | ValueMin: 1 Time check intervals | ValueIncrement: 1 Time check intervals
> * Processor energy performance preference policy
>   
>   * GUID: `36687f9e-e3a5-4dbf-b1dc-15eb381c6863`
>   * Specify how much processors should favor energy savings over performance when operating in autonomous mode.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Allow Throttle States
>   
>   * GUID: `3b04d4fd-1cc7-4f23-ab1c-d1337819c4bb`
>   * Allow processors to use throttle states in addition to performance states.
>   * Possible values: Off, On, Automatic
> * Processor performance increase time for Processor Power Efficiency Class 1
>   
>   * GUID: `4009efa7-e72d-4cba-9edf-91084ea8cbc3`
>   * Specify the minimum number of perf check intervals since the last performance state change before the performance state may be increased for Processor Power Efficiency Class 1.
>   * ValueMax: 100 Time check intervals | ValueMin: 1 Time check intervals | ValueIncrement: 1 Time check intervals
> * Processor performance decrease policy
>   
>   * GUID: `40fbefc7-2e9d-4d25-a185-0cfd8574bac6`
>   * Specify the algorithm used to select a new performance state when the ideal performance state is lower than the current performance state.
>   * Possible values: Ideal, Single, Rocket
> * Processor performance decrease policy for Processor Power Efficiency Class 1
>   
>   * GUID: `40fbefc7-2e9d-4d25-a185-0cfd8574bac7`
>   * Specify the algorithm used to select a new performance state when the ideal performance state is lower than the current performance state for Processor Power Efficiency Class 1.
>   * Possible values: Ideal, Single, Rocket
> * Processor performance core parking parked performance state
>   
>   * GUID: `447235c7-6a8d-4cc0-8e24-9eaf70b96e2b`
>   * Specify what performance state a processor enters when parked.
>   * Possible values: No Preference, Deepest Performance State, Lightest Performance State
> * Processor performance core parking parked performance state for Processor Power Efficiency Class 1
>   
>   * GUID: `447235c7-6a8d-4cc0-8e24-9eaf70b96e2c`
>   * Specify what performance state a Processor Power Efficiency Class 1 processor enters when parked.
>   * Possible values: No Preference, Deepest Performance State, Lightest Performance State
> * Processor performance boost policy
>   
>   * GUID: `45bcc044-d885-43e2-8605-ee0ec6e96b59`
>   * Specify how much processors may opportunistically increase frequency above maximum when allowed by current operating conditions.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance increase policy
>   
>   * GUID: `465e1f50-b610-473a-ab58-00d1077dc418`
>   * Specify the algorithm used to select a new performance state when the ideal performance state is higher than the current performance state.
>   * Possible values: Ideal, Single, Rocket, IdealAggressive
> * Processor performance increase policy for Processor Power Efficiency Class 1
>   
>   * GUID: `465e1f50-b610-473a-ab58-00d1077dc419`
>   * Specify the algorithm used to select a new performance state when the ideal performance state is higher than the current performance state for Processor Power Efficiency Class 1.
>   * Possible values: Ideal, Single, Rocket, IdealAggressive
> * Processor idle demote threshold
>   
>   * GUID: `4b92d758-5a24-4851-a470-815d78aee119`
>   * Specify the upper busy threshold that must be met before demoting the processor to a lighter idle state (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance core parking distribution threshold
>   
>   * GUID: `4bdaf4e9-d103-46d7-a5f0-6280121616ef`
>   * Specify the percentage utilization used to calculate the distribution concurrency (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance time check interval
>   
>   * GUID: `4d2b0152-7d5c-498b-88e2-34345392a2c5`
>   * Specify the amount that must expire before processor performance states and parked cores may be reevaluated (in milliseconds).
>   * ValueMax: 5000 Milliseconds | ValueMin: 1 Milliseconds | ValueIncrement: 1 Milliseconds
> * Processor duty cycling
>   
>   * GUID: `4e4450b3-6179-4e91-b8f1-5bb9938f81a1`
>   * Specify whether the processor may use duty cycling.
>   * Possible values: Disabled, Enabled
> * Processor idle disable
>   
>   * GUID: `5d76a2ca-e8c0-402f-a133-2158492d58ad`
>   * Specify if idle states should be disabled.
>   * Possible values: Enable idle, Disable idle
> * Latency sensitivity hint min unparked cores/packages
>   
>   * GUID: `616cdaa5-695e-4545-97ad-97dc2d1bdd88`
>   * Specify the minimum number of unparked cores/packages when a latency hint is active (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Latency sensitivity hint min unparked cores/packages for Processor Power Efficiency Class 1
>   
>   * GUID: `616cdaa5-695e-4545-97ad-97dc2d1bdd89`
>   * Specify the minimum number of unparked cores/packages when a latency hint is active for Processor Power Efficiency Class 1 (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Latency sensitivity hint processor performance
>   
>   * GUID: `619b7505-003b-4e82-b7a6-4dd29c300971`
>   * Specify the processor performance in response to latency sensitivity hints.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Latency sensitivity hint processor performance for Processor Power Efficiency Class 1
>   
>   * GUID: `619b7505-003b-4e82-b7a6-4dd29c300972`
>   * Specify the processor performance in response to latency sensitivity hints for Processor Power Efficiency Class 1.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor idle threshold scaling
>   
>   * GUID: `6c2993b0-8f48-481f-bcc6-00dd2742aa06`
>   * Specify if idle state promotion and demotion values should be scaled based on the current performance state.
>   * Possible values: Disable scaling, Enable scaling
> * Processor performance core parking decrease policy
>   
>   * GUID: `71021b41-c749-4d21-be74-a00f335d582b`
>   * Specify the number of cores/packages to park when fewer cores are required.
>   * Possible values: Ideal number of cores, Single core, All possible cores, One eighth cores
> * Maximum processor frequency
>   
>   * GUID: `75b0ae3f-bce0-45a7-8c89-c9611c25e100`
>   * Specify the approximate maximum frequency of your processor (in MHz).
>   * ValueMax: 4294967295 MHz | ValueMin: 0 MHz | ValueIncrement: 1 MHz
> * Maximum processor frequency for Processor Power Efficiency Class 1
>   
>   * GUID: `75b0ae3f-bce0-45a7-8c89-c9611c25e101`
>   * Specify the approximate maximum frequency of your Processor Power Efficiency Class 1 processor (in MHz).
>   * ValueMax: 4294967295 MHz | ValueMin: 0 MHz | ValueIncrement: 1 MHz
> * Processor idle promote threshold
>   
>   * GUID: `7b224883-b3cc-4d79-819f-8374152cbe7c`
>   * Specify the lower busy threshold that must be met before promoting the processor to a deeper idle state (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance history count
>   
>   * GUID: `7d24baa7-0b84-480f-840c-1b0743c00f5f`
>   * Specify the number of processor performance time check intervals to use when calculating the average utility.
>   * ValueMax: 128 Time check intervals | ValueMin: 1 Time check intervals | ValueIncrement: 1 Time check intervals
> * Processor performance history count for Processor Power Efficiency Class 1
>   
>   * GUID: `7d24baa7-0b84-480f-840c-1b0743c00f60`
>   * Specify the number of processor performance time check intervals to use when calculating the average utility for Processor Power Efficiency Class 1.
>   * ValueMax: 128 Time check intervals | ValueMin: 1 Time check intervals | ValueIncrement: 1 Time check intervals
> * Processor performance decrease time for Processor Power Efficiency Class 1
>   
>   * GUID: `7f2492b6-60b1-45e5-ae55-773f8cd5caec`
>   * Specify the minimum number of perf check intervals since the last performance state change before the performance state may be decreased for Processor Power Efficiency Class 1.
>   * ValueMax: 100 Time check intervals | ValueMin: 1 Time check intervals | ValueIncrement: 1 Time check intervals
> * Heterogeneous policy in effect
>   
>   * GUID: `7f2f5cfa-f10c-4823-b5e1-e93ae85f46b5`
>   * Specify what policy to be used on systems with at least two different Processor Power Efficiency Classes.
>   * Possible values: Use heterogeneous policy 0, Use heterogeneous policy 1, Use heterogeneous policy 2, Use heterogeneous policy 3, Use heterogeneous policy 4
> * Minimum processor state
>   
>   * GUID: `893dee8e-2bef-41e0-89c6-b55d0929964c`
>   * Specify the minimum performance state of your processor (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Minimum processor state for Processor Power Efficiency Class 1
>   
>   * GUID: `893dee8e-2bef-41e0-89c6-b55d0929964d`
>   * Specify the minimum performance state of your Processor Power Efficiency Class 1 processor (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance autonomous mode
>   
>   * GUID: `8baa4a8a-14c6-4451-8e8b-14bdbd197537`
>   * Specify whether processors should autonomously determine their target performance state.
>   * Possible values: Disabled, Enabled
> * Processor performance core parking overutilization threshold
>   
>   * GUID: `943c8cb6-6f93-4227-ad87-e9a3feec08d1`
>   * Specify the busy threshold that must be met before a parked core is considered overutilized (in percentage).
>   * ValueMax: 100 % | ValueMin: 5 % | ValueIncrement: 1 %
> * System cooling policy
>   
>   * GUID: `94d3a615-a899-4ac5-ae2b-e4d8f634367f`
>   * Specify the cooling mode for your system
>   * Possible values: Passive, Active
> * Processor performance increase time
>   
>   * GUID: `984cf492-3bed-4488-a8f9-4286c97bf5aa`
>   * Specify the minimum number of perf check intervals since the last performance state change before the performance state may be increased.
>   * ValueMax: 100 Time check intervals | ValueMin: 1 Time check intervals | ValueIncrement: 1 Time check intervals
> * Processor performance increase time for Processor Power Efficiency Class 1
>   
>   * GUID: `984cf492-3bed-4488-a8f9-4286c97bf5ab`
>   * Specify the minimum number of perf check intervals since the last performance state change before the performance state may be increased for Processor Power Efficiency Class 1.
>   * ValueMax: 100 Time check intervals | ValueMin: 1 Time check intervals | ValueIncrement: 1 Time check intervals
> * Processor idle state maximum
>   
>   * GUID: `9943e905-9a30-4ec1-9b99-44dd3b76f7a2`
>   * Specify the deepest idle state that should be used by Hyper-V.
>   * ValueMax: 20 State Type | ValueMin: 0 State Type | ValueIncrement: 1 State Type
> * Processor performance level increase threshold for Processor Power Efficiency Class 1 processor count increase
>   
>   * GUID: `b000397d-9b0b-483d-98c9-692a6060cfbf`
>   * Specifies the performance level increase threshold at which the Processor Power Efficiency Class 1 processor count is increased (in units of Processor Power Efficiency Class 0 processor performance).
> * Maximum processor state
>   
>   * GUID: `bc5038f7-23e0-4960-96da-33abaf5935ec`
>   * Specify the maximum performance state of your processor (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Maximum processor state for Processor Power Efficiency Class 1
>   
>   * GUID: `bc5038f7-23e0-4960-96da-33abaf5935ed`
>   * Specify the maximum performance state of your Processor Power Efficiency Class 1 processor (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance boost mode
>   
>   * GUID: `be337238-0d82-4146-a960-4f3749d470c7`
>   * Specify how processors select a target frequency when allowed to select above maximum frequency by current operating conditions.
>   * Possible values: Disabled, Enabled, Aggressive, Efficient Enabled, Efficient Aggressive, Aggressive At Guaranteed, Efficient Aggressive At Guaranteed
> * Processor idle time check
>   
>   * GUID: `c4581c31-89ab-4597-8e2b-9c9cab440e6b`
>   * Specify the time that elapsed since the last idle state promotion or demotion before idle states may be promoted or demoted again (in microseconds).
>   * ValueMax: 200000 Microseconds | ValueMin: 1 Microseconds | ValueIncrement: 1 Microseconds
> * Processor performance core parking increase policy
>   
>   * GUID: `c7be0679-2817-4d69-9d02-519a537ed0c6`
>   * Specify the number of cores/packages to unpark when more cores are required.
>   * Possible values: Ideal number of cores, Single core, All possible cores, One eighth cores
> * Processor autonomous activity window
>   
>   * GUID: `cfeda3d0-7697-4566-a922-a9086cd49dfa`
>   * Specify the time period over which to observe processor utilization when operating in autonomous mode.
>   * ValueMax: 1270000000 Microseconds | ValueMin: 0 Microseconds | ValueIncrement: 1 Microseconds
> * Processor performance decrease time
>   
>   * GUID: `d8edeb9b-95cf-4f95-a73c-b061973693c8`
>   * Specify the minimum number of perf check intervals since the last performance state change before the performance state may be decreased.
>   * ValueMax: 100 Time check intervals | ValueMin: 1 Time check intervals | ValueIncrement: 1 Time check intervals
> * Processor performance decrease time for Processor Power Efficiency Class 1
>   
>   * GUID: `d8edeb9b-95cf-4f95-a73c-b061973693c9`
>   * Specify the minimum number of perf check intervals since the last performance state change before the performance state may be decreased for Processor Power Efficiency Class 1.
>   * ValueMax: 100 Time check intervals | ValueMin: 1 Time check intervals | ValueIncrement: 1 Time check intervals
> * Processor performance core parking decrease time
>   
>   * GUID: `dfd10d17-d5eb-45dd-877a-9a34ddd15c82`
>   * Specify the minimum number of perf check intervals that must elapse before more cores/packages can be parked.
>   * ValueMax: 100 Time check intervals | ValueMin: 1 Time check intervals | ValueIncrement: 1 Time check intervals
> * Processor performance core parking utility distribution
>   
>   * GUID: `e0007330-f589-42ed-a401-5ddb10e785d3`
>   * Specify whether the core parking engine should distribute utility across processors.
>   * Possible values: Disabled, Enabled
> * Processor performance core parking max cores
>   
>   * GUID: `ea062031-0e34-4ff1-9b6d-eb1059334028`
>   * Specify the maximum number of unparked cores/packages allowed (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance core parking max cores for Processor Power Efficiency Class 1
>   
>   * GUID: `ea062031-0e34-4ff1-9b6d-eb1059334029`
>   * Specify the maximum number of unparked cores/packages allowed for Processor Power Efficiency Class 1 (in percentage).
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance core parking concurrency headroom threshold
>   
>   * GUID: `f735a673-2066-4f80-a0c5-ddee0cf1bf5d`
>   * Specify the busy threshold that must be met by all cores in a concurrency set to unpark an extra core.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Processor performance level decrease threshold for Processor Power Efficiency Class 1 processor count decrease
>   
>   * GUID: `f8861c27-95e7-475c-865b-13c0cb3f9d6b`
>   * Specifies the performance level decrease threshold at which the Processor Power Efficiency Class 1 processor count is decreased (in units of Processor Power Efficiency Class 0 processor performance).
> * A floor performance for Processor Power Efficiency Class 0 when there are Processor Power Efficiency Class 1 processors unparked
>   
>   * GUID: `fddc842b-8364-4edc-94cf-c17f60de1c80`
>   * Performance state floor for Processor Power Efficiency Class 0 when Processor Power Efficiency Class 1 is woken from a parked state.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> 
> # Display
> Specify power management settings for your display. Group GUID: `7516b95f-f776-4464-8c53-06167f40cc99`
> 
> * Dim display after
>   
>   * GUID: `17aaa29b-8b43-4b94-aafe-35f64daaf1ee`
>   * Specify how long your computer is inactive before your display dims.
>   * ValueMax: 4294967295 Seconds | ValueMin: 0 Seconds | ValueIncrement: 1 Seconds
> * Turn off display after
>   
>   * GUID: `3c0bc021-c8a8-4e07-a973-6b14cbcb2b7e`
>   * Specify how long your computer is inactive before your display turns off.
>   * ValueMax: 4294967295 Seconds | ValueMin: 0 Seconds | ValueIncrement: 1 Seconds
> * NVIDIA-näytön virransäästöteknologia
>   
>   * GUID: `89cc76a4-f226-4d4b-a040-6e9a1da9b882`
>   * Säätää virkistystaajuuden virransäästöä ja näyttölaadun säilyttämistä varten.
>   * Possible values: Ei käytössä, Käytössä
> * Console lock display off timeout
>   
>   * GUID: `8ec4b3a5-6868-48c2-be75-4f3044be88a7`
>   * Specifies console lock display off timeout
>   * ValueMax: 4294967295 Seconds | ValueMin: 0 Seconds | ValueIncrement: 1 Seconds
> * Adaptive display
>   
>   * GUID: `90959d22-d6a1-49b9-af93-bce885ad335b`
>   * Extends the time that Windows waits to turn off the display if you repeatedly turn on the display with the keyboard or mouse.
>   * Possible values: Off, On
> * Allow display required policy
>   
>   * GUID: `a9ceb8da-cd46-44fb-a98b-02af69de4623`
>   * Allow programs to prevent display from turning off automatically
>   * Possible values: No, Yes
> * Display brightness
>   
>   * GUID: `aded5e82-b909-4619-9949-f5d71dac0bcb`
>   * Specify the normal brightness level of your display.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Mukautuva taustavalo
>   
>   * GUID: `aded5e82-b909-4619-9949-f5d71dac0bcc`
>   * Optimoi värien ja taustavalon käytön siten, että akun kestoikä paranee eikä kuvan kirkkaus muutu.
>   * Possible values: Ei mitään, 1 (Pieni), 2, 3 (Keski), 4, 5 (Suuri)
> * Dimmed display brightness
>   
>   * GUID: `f1fbfde2-a960-4165-9f88-50667911ce96`
>   * Specify the brightness level for when your display is dimmed.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Enable adaptive brightness
>   
>   * GUID: `fbd9aa66-9553-4097-ba44-ed6e9d65eab8`
>   * Monitors ambient light sensors to detect changes in ambient light and adjust the display brightness.
>   * Possible values: Off, On
> 
> # Presence Aware Power Behavior
> Presence Aware Power Behavior Settings Group GUID: `8619b916-e004-4dd8-9b66-dae86f806698`
> 
> * Standby Reserve Time
>   
>   * GUID: `468fe7e5-1158-46ec-88bc-5b96c9e44fd0`
>   * Specifies the minimun active usage time that the battery charge level should allow before taking an adaptive action
>   * ValueMax: 4294967295 Seconds | ValueMin: 0 Seconds | ValueIncrement: 1 Seconds
> * Standby Reset Percentage
>   
>   * GUID: `49cb11a5-56e2-4afb-9d38-3df47872e21b`
>   * Specifies percentage of battery charge which resets the adaptive budget
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Non-sensor Input Presence Timeout
>   
>   * GUID: `5adbbfbc-074e-4da1-ba38-db8b36b2c8f3`
>   * Specifies Non-sensor Input Presence Timeout
>   * ValueMax: 4294967295 Seconds | ValueMin: 0 Seconds | ValueIncrement: 1 Seconds
> * Standby Budget Grace Period
>   
>   * GUID: `60c07fe1-0556-45cf-9903-d56e32210242`
>   * Specifies the grace period before taking an adaptive action when the system has exceeded its standby budget
>   * ValueMax: 4294967295 Seconds | ValueMin: 0 Seconds | ValueIncrement: 1 Seconds
> * User Presence Prediction mode
>   
>   * GUID: `82011705-fb95-4d46-8d35-4042b1d20def`
>   * Specify User Presence Prediction mode for your computer
>   * Possible values: Disabled, Enabled
> * Standby Budget Percent
>   
>   * GUID: `9fe527be-1b70-48da-930d-7bcf17b44990`
>   * Specifies percentage of battery per unit of time allowed to be consumed by the system while it is in standby
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Standby Reserve Grace Period
>   
>   * GUID: `c763ee92-71e8-4127-84eb-f6ed043a3e3d`
>   * Specifies the grace period before taking an adaptive action when the system is below the reserve battery charge level
>   * ValueMax: 4294967295 Seconds | ValueMin: 0 Seconds | ValueIncrement: 1 Seconds
> 
> # Multimedia settings
> Configure power settings for when devices and computers are playing media from your computer. Group GUID: `9596fb26-9850-41fd-ac3e-f7c3c00afd4b`
> 
> * When sharing media
>   
>   * GUID: `03680956-93bc-4294-bba6-4e0f09bb717f`
>   * Specify what your computer does when a device or computer is playing media from your computer.
>   * Possible values: Allow the computer to sleep, Prevent idling to sleep, Allow the computer to enter Away Mode
> * Video playback quality bias.
>   
>   * GUID: `10778347-1370-4ee0-8bbd-33bdacaade49`
>   * Specify the policy to bias video playback quality.
>   * Possible values: Video playback power-saving bias., Video playback performance bias.
> * When playing video
>   
>   * GUID: `34c7b99f-9a6d-4b3c-8dc7-b6693b78cef4`
>   * The power optimization mode used by your computer's video playback pipeline
>   * Possible values: Optimize video quality, Balanced, Optimize power savings
> 
> # Energy Saver settings
> Energy Saver settings. Group GUID: `de830923-a562-41af-a086-e3a2c6bad2da`
> 
> * Display brightness weight
>   
>   * GUID: `13d09884-f74e-474a-a852-b6bde8ad03a8`
>   * Specifies the percentage value to scale brightness when Energy Saver is on.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Energy Saver Policy
>   
>   * GUID: `5c5bb349-ad29-4ee2-9d0b-2b25270f7a81`
>   * Specifies the policy to control Energy Saver.
>   * Possible values: User, Aggressive
> * Charge level
>   
>   * GUID: `e69653ca-cf7f-4f05-aa73-cb833fa90ad4`
>   * Specifies battery charge level at which Energy Saver is turned on.
>   * ValueMax: 100 Percent battery charge | ValueMin: 0 Percent battery charge | ValueIncrement: 1 Percent battery charge
> 
> # Battery
> Configure notification and alarm settings for your battery. Group GUID: `e73a048d-bf27-4f12-9731-8b2076e8891f`
> 
> * Critical battery action
>   
>   * GUID: `637ea02f-bbcb-4015-8e2c-a1c7b9c0b546`
>   * Specify the action to take when the battery capacity reaches the critical level.
>   * Possible values: Do nothing, Sleep, Hibernate, Shut down
> * Low battery level
>   
>   * GUID: `8183ba9a-e910-48da-8769-14ae6dc1170a`
>   * Percentage of battery capacity remaining that initiates the low battery action.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Critical battery level
>   
>   * GUID: `9a66d8d7-4ff7-4ef9-b5a2-5a326ca2a469`
>   * Percentage of battery capacity remaining that initiates the critical battery action.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %
> * Low battery notification
>   
>   * GUID: `bcded951-187b-4d05-bccc-f7e51960c258`
>   * Specify whether a notification is shown when the battery capacity reaches the low level.
>   * Possible values: Off, On
> * Low battery action
>   
>   * GUID: `d8742dcb-3e6a-4b3c-b3fe-374623cdcf06`
>   * Specify the action that your computer takes when battery capacity reaches the low level.
>   * Possible values: Do nothing, Sleep, Hibernate, Shut down
> * Reserve battery level
>   
>   * GUID: `f3c5027d-cd16-4930-aa6b-90db844a8f00`
>   * Percentage of battery capacity remaining that initiates reserve power mode.
>   * ValueMax: 100 % | ValueMin: 0 % | ValueIncrement: 1 %

