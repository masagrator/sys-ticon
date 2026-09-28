sys-ticon
================

Stripped version of [switch-sys-tweak](https://github.com/p-sam/switch-sys-tweak) to only replace icons, titles, publishers and display versions in qlaunch, upgraded to be compatible with 21.0.0+ and ported to use newest libstratosphere.

Tested on 17.0.0, 20.5.0, 21.1.0, 22.0.0, 22.5.0 and 23.0.0. Should work with every FW. At this point latest available FW is 23.0.0

# How to use

Latest release you can find [HERE](https://github.com/masagrator/sys-ticon/releases), download "sys-ticon.zip", put "atmosphere" folder to sdcard. Restart Switch.

"sys-ticon-log.zip" produces log file "sys-ticon.txt" on root of sdcard. Use it only if you have some issues with original sysmodule, reproduce the issue and attach this file to your issue report. I will close your issue immediately if you won't include this file. 

> [!IMPORTANT]
> Any change requires console reboot to see results immediately in home menu because those entries are cached upon boot, only "Options" menu and "All Software" show immediately changes because they are not cached.

### Title, publisher, display version
If you want to replace title + publisher and/or display version, use this template:
```ini
[override_nacp]
name=Some Title
author=Some Publisher
display_version=21.37
```

  System limitations:
  - File must be saved with encoding UTF-8 without BOM (Byte-Order Mark)
  - `name` cannot take more than 512 bytes
  - `author` cannot take more than 256 bytes
  - `display_version` cannot take more than 16 bytes

If name or author is longer than requested, both are ignored. If display_version is longer than requested, it is ignored.

Save it in "config.ini" and put it to `atmosphere/contents/*titleid*/`. If you already have such file with different things, make sure that our data are at the beginning of file.

`name` + `author` must come always in pair, one of the missing will mean that another one will be ignored. This is to avoid work with compressed NACPs.<br>
If you don't want to replace display version, just remove `display_version=` line. If you want to change only `display_version`, remove lines with `name=` and `author=`.

### Icon 
If you want to replace icon:
-  Create 256x256 JPG (must be baseline, aka non-progressive/non-interlaced) with max size 131072 B for FWs before 19.0.0, for 19.0.0+ it cannot be bigger than 102400 B. Name it "icon.jpg"
-  For 20.0.0+ create additional 174x174 JPG (must be baseline) with max size 65536 B. Name it "icon174.jpg"
-  Put them to `atmosphere/contents/*titleid*/`.

Since 20.0.0 "Options" menu and "All Software" are using 174x174 icons instead of 256x256, that's why we need 174x174 icon separately. If you won't put this icon, original one will show up.

If any detected JPG will be too big, progressive or malformed, it will be ignored. Proper information will be printed when using sys-ticon-log.

## How to compile

Standard compilation:
First (we are doing this only once, `make clean` won't remove this)
```
make libstrato
```
and then without logger enabled:
```
make
```
or with logger enabled
```
make TOGL_LOGGING=1
```
