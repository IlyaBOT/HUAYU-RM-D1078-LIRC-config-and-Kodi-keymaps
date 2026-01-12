# HUAYU RM-D1078+ LIRC config and Kodi keymaps
**"HUAYU RM-D1078+"** Remote control configuration files for Kodi using LIRC.
<img src="https://github.com/IlyaBOT/HUAYU-RM-D1078-LIRC-config-and-Kodi-keymaps/blob/main/HUAYU_remote.png" width="48" align="left">

<br><br><br><br><br><br><br>

## Contents

- `lircd.conf` — LIRC remote configuration for your IR remote.
- `Lircmap.xml` — mapping from LIRC `KEY_*` codes into Kodi button names.
- `keymaps/remote.xml` — Kodi keymap definitions for mapped remote buttons.

## Usage

### 1. LIRC Config
Before using the provided `lircd.conf`, make sure your LIRC daemon is configured to use the correct driver and IR device. If your LIRC daemon is configured correctly, skip to [step 1.2](#12-place-huayu-remote-config-at-etclirc-folder). If not, read [step 1.1](#11-fix-lirc-daemon-config-file-for-using-the-correct-driver-and-device) and follow the steps there.

#### 1.1 Fix LIRC daemon config file for using the correct driver and device:

```bash
sudo nano /etc/lirc/lirc_options.conf
```

Change the `[lircd]` section to:

```
[lircd]
driver = default
device = /dev/lirc0
```

**Don't change any other values, except `driver` and `device`!**
This ensures LIRC daemon uses the **raw IR device** at `/dev/lirc0` rather than the `devinput` driver.
After saving the file, restart LIRC:

```bash
sudo systemctl restart lircd
```

#### 1.2 Place HUAYU remote config at /etc/lirc folder:

```bash
sudo cp HUAYU.lircd.conf /etc/lirc/lircd.conf
sudo systemctl restart lircd
```

#### 1.3 Testing LIRC config and remote

To check if lirc is configured correctly and the configuration file is applied, use the command **`irw`** and press the buttons on the remote. If nothing is displayed in the terminal when you press the buttons, then either lirc/lirc daemon is configured incorrectly, or the config file is not applied, or it is not suitable for your remote control.

You also can check whether lirc is configured correctly and accepting codes from your ir remote using the command:

```bash
mode2 -d /dev/lirc0
```

If, when pressing buttons on the remote, instead of the **`pulse`** and **`space`** strings, you get something like this:
```bash
Using driver devinput on device /dev/lirc0
Trying device: /dev/lirc0
Using device: /dev/lirc0
code: 0xb2c2c800c011000188110000300200019006000030020001
code: 0x900600003002000190060000300200014002000028020001
code: 0x400200002802000140020000300200014002000028020001
Partial read 8 bytes on /dev/lirc0%
```
this means your lirc daemon is configured incorrectly and is using the wrong driver or ir device. Proceed to [step 1.1](#11-fix-lirc-daemon-config-file-for-using-the-correct-driver-and-device) to fix the lirc daemon config.

### 2. Kodi Keymap

Copy the files into your Kodi userdata directory:

```bash
mkdir -p ~/.kodi/userdata
mkdir -p ~/.kodi/userdata/keymaps

cp Lircmap.xml ~/.kodi/userdata/Lircmap.xml
cp keymaps/remote.xml ~/.kodi/userdata/keymaps/
```

Then restart Kodi.
