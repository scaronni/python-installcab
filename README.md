# install-mf-wmv.sh
Installer for Windows Media Foundation and Windows Media DLLs for Steam Windows games running under Proton.

## Known working games (including cut scenes):

- Resident Evil 2
- Resident Evil 7
- Darksiders Warmastered Edition
- Devil May Cry 5

Just set WINEPREFIX and the PROTON version you use for that particular game and run the script:

`WINEPREFIX="/home/slaanesh/.local/share/Steam/steamapps/compatdata/462780/pfx" PROTON=4.11 ./install-mf-wmv.sh`

The script will download all necessary files and leave them in the folder where the script is.

`installcab.py` is exactly the same as upstream (https://github.com/tonix64/python-installcab/blob/master/installcab.py)

## Dependencies
- python2
- cabextract
- wine

# python-installcab

Extract and install components from cab based installers

This tool is intended to be used with [wine](https://winehq.org)

## Experimental

The software is  **experimental** so you should try it in a fresh wineprefix or make a backup of your wineprefix just in case (at least windows folder and .reg files at the root of your wineprefix).

## Usage:

```
python installcab.py [options] cabfile filter [wineprefix_path]
```

- cabfile: an exe installer or cab file that can be extracted with cabextract
- filter: a filter for the components inside the cab file (will match anywhere in available files inside the cabinet)
- wineprefix_path: you can set this, otherwise it will try to get from your WINEPREFIX environment variable

### Options

-  --nocleanup: do not perform cleanup after running
-  --noreg: do not import registry entries
-  --nodll: do not install dlls into system dir
-  --register: register dlls with regsrv32
-  --stripdllpath: strip full path for dlls in registry so wine can find them through it's own resolver
   
## Examples

```
python installcab.py ~/.cache/winetricks/win7sp1/windows6.1-KB976932-X86.exe x86_microsoft-windows-mediafoundation"
```

will extract and install any manifest files and dlls with 'x86_microsoft-windows-mediafoundation' in their path.

```
python installcab.py ~/.cache/winetricks/win7sp1/windows6.1-KB976932-X64.exe wmadmod
```

will extract and install any manifest files and dlls with 'wmadmod' in their path.

### install-mf examples

You can also try the [install-mf.sh](install-mf.sh) file that installs WMF components from win7sp1 installer. There is also [install-mf-64.sh](install-mf-64.sh) for installing into a 64 bit wineprefix.

Note you will need to run `winetricks mf` first so the installer will be cached (for now install-mf.sh does not download the installer, but relies on winetricks to have done it beforehand).

## What exactly it does

1. Extracts from the cab file using cabextract with 'component' as filter
2. Finds out what dll files and manifest files where extracted
3. Will place dll files in $WINEPREFIX/windows/system32/ or syswow64 depending on arch
4. Will convert manifest data to .reg file and import it through wine/wine64

The software doesn't try to find out dependencies or be smart about where to install dlls, it will just install dlls in the system32 or syswow64 directory, and import stuff into wine's registry from manifest files.

It does not aim to fully support cabinet installers, but is capable of installing just parts from an installer, going a bit further than what is actually possible with 'proper' support.

## License

This software is released into the Public Domain by use of the Unlicense, see the [LICENSE](LICENSE) file
for more details.
