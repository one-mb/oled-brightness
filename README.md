# OLED laptop display brightness changer

This script handles OLED display brightness settings.

## the issue
With many modern laptops you're able to change the internal display brightness, by changing the "background light" via FN-key combinations. But OLED displays do not have a traditional "Background-LED" to change. 
That given, without proper hardware support, the FN-Keys won't change the brightness, but rather trigger some error messages like:
```
Precision-5540 kernel: [22999.402040] ACPI Error: [LCD_] Namespace lookup failure, AE_NOT_FOUND (20170831/psargs-364)
Precision-5540 kernel: [22999.402072] No Local Variables are initialized for Method [BRT6]
Precision-5540 kernel: [22999.402077] Initialized Arguments for Method [BRT6]:  (2 arguments defined for method invocation)
Precision-5540 kernel: [22999.402079]   Arg0:   00000000424112d7 <Obj>           Integer 0000000000000001
Precision-5540 kernel: [22999.402092]   Arg1:   0000000097f471df <Obj>           Integer 0000000000000000
Precision-5540 kernel: [22999.402107] ACPI Error: Method parse/execution failed \_SB.PCI0.PEG0.PEGP.BRT6, AE_NOT_FOUND (20170831/psparse-550)
Precision-5540 kernel: [22999.402313] ACPI Error: Method parse/execution failed \EV5, AE_NOT_FOUND (20170831/psparse-550)
Precision-5540 kernel: [22999.402510] ACPI Error: Method parse/execution failed \SMEE, AE_NOT_FOUND (20170831/psparse-550)
Precision-5540 kernel: [22999.402706] ACPI Error: Method parse/execution failed \SMIE, AE_NOT_FOUND (20170831/psparse-550)
Precision-5540 kernel: [22999.402900] ACPI Error: Method parse/execution failed \NEVT, AE_NOT_FOUND (20170831/psparse-550)
Precision-5540 kernel: [22999.403103] ACPI Error: Method parse/execution failed \_SB.PCI0.LPCB.ECDV._Q66, AE_NOT_FOUND (20170831/psparse-550)
```

# possible workaround for OLED displays 

Given that, the brightness can also be changed via xrandr: 
```
xrandr --output eDP1 --brightness 0.3
```
The useful brightness values range from 0.1 to 1.0. You'll propably not use 0.0, as that might turn the screen completely black.

## attaching to the FN-Keys / usage

The `oled-brightness` script replicates the original FN-Key behaviour. 
It accepts a change value as parameter, e.g. `./oled-brightness -0.1` to reduce brightness by 10% or `./oled-brightness 0.3` to raise it by 30%.
 
Furthermore it persists the new brightness value in the user's home directory in `~/.brightness`

A call without a change value, will read the users value from `~/.brightness` and set the display's brightness value accordingly. 

This way you may set up your X session's key binding to call this script with a change value. And additionally call this script, without change value, at session start to reset it to the last value. (Unfortunately the X session will likely restart at 1.0 again otherwise.)


## requirements - or - why PHP?

* any available `php-cli` interpreter
* xrandr

Well mostly, because 
* Bash can't handle float values, without jumping some "dirty" loops.
* Zsh might be a hurdle to the unexperienced users.
* php was ... nah, that hammer was in reach.

I'll propably supply a zsh version later on.
