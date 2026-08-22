Absolutely. Here's a **standalone README** you can paste directly into `README.md`. It doesn't depend on this conversation and is written as a reusable setup/reference guide.

````markdown
# iOS Location Spoofing on macOS

Free GPS location simulation for a physical iPhone using `pymobiledevice3` and Apple's DVT developer services.

This method uses the iOS 17+ DVT location-simulation functionality exposed by `pymobiledevice3`. It does **not** require paid software such as iTools.

## What This Does

Allows a Mac to set a simulated GPS location on a USB-connected iPhone.

You can:

- Teleport the iPhone to arbitrary GPS coordinates
- Clear the simulated location and return to normal GPS
- Replay GPX routes
- Use the functionality directly from Terminal
- Avoid paid GPS-spoofing applications

The underlying mechanism is Apple's developer tooling (DVT), accessed through `pymobiledevice3`.

---

# Tested Configuration

This setup has been confirmed working with:

| Component | Version |
|---|---|
| macOS | 15.5 Sequoia |
| Mac architecture | Intel `x86_64` |
| Xcode | 16.4 (Build 16F6) |
| iPhone | iOS 18.7.8 |
| Connection | USB |
| Python | 3.x |
| `pymobiledevice3` | Current installed version |

The important compatibility requirement is:

> **iOS 17 or newer**

`pymobiledevice3` provides DVT location simulation for iOS 17+.

---

# Requirements

## Mac

- macOS
- Python 3
- `pymobiledevice3`
- USB connection to the iPhone

Xcode is useful/required for Apple's developer tooling and related device services.

## iPhone

- Physical iPhone
- iOS 17 or newer
- USB connection
- Device paired/trusted with the Mac
- Developer Mode enabled when required by the device/developer tooling

---

# Installation

## 1. Install Python

Check whether Python 3 is installed:

```bash
python3 --version
````

If Python is not installed, install a current Python 3 release.

After installation:

```bash
python3 --version
```

---

# 2. Install pymobiledevice3

The recommended installation method is `pipx`:

```bash
pipx install pymobiledevice3
```

Alternatively:

```bash
pip3 install pymobiledevice3
```

Verify that the command is available:

```bash
pymobiledevice3 -h
```

> Note: `pymobiledevice3 --version` is not necessarily supported. To query the installed version, use:

```bash
pymobiledevice3 version
```

---

# 3. Connect the iPhone

Connect the iPhone to the Mac with USB.

Unlock the iPhone and accept the **Trust This Computer** prompt if it appears.

Verify that `pymobiledevice3` can communicate with the device:

```bash
pymobiledevice3 -h
```

The device should also be accessible through the relevant device commands.

---

# 4. Developer Mode

DVT developer services require the iPhone to be configured for development.

On the iPhone:

**Settings → Privacy & Security → Developer Mode**

Enable Developer Mode if it is not already enabled.

The iPhone may reboot after enabling it.

---

# 5. Check DVT

Run:

```bash
pymobiledevice3 developer dvt -h
```

You should see a command called:

```text
simulate-location
```

It should be described approximately as:

```text
Simulate device location through DVT on iOS 17 and newer.
```

---

# Location Simulation

## Set a Location

The basic command is:

```bash
pymobiledevice3 developer dvt simulate-location set LATITUDE LONGITUDE
```

For example, to simulate the Statue of Liberty:

```bash
pymobiledevice3 developer dvt simulate-location set 40.690008 "-74.045843"
```

The latitude is:

```text
40.690008
```

The longitude is:

```text
-74.045843
```

### Important: Negative Longitudes

On some versions of the CLI, a negative longitude can be interpreted as a command-line option.

Quoting the longitude avoids this:

```bash
pymobiledevice3 developer dvt simulate-location set 40.690008 "-74.045843"
```

Use the same approach for other negative coordinates.

---

# Examples

## Boston

```bash
pymobiledevice3 developer dvt simulate-location set 42.3601 "-71.0589"
```

## New York City

```bash
pymobiledevice3 developer dvt simulate-location set 40.7128 "-74.0060"
```

## Los Angeles

```bash
pymobiledevice3 developer dvt simulate-location set 34.0522 "-118.2437"
```

## London

```bash
pymobiledevice3 developer dvt simulate-location set 51.5074 "-0.1278"
```

## Tokyo

```bash
pymobiledevice3 developer dvt simulate-location set 35.6762 139.6503
```

---

# Clear the Simulated Location

To stop location simulation:

```bash
pymobiledevice3 developer dvt simulate-location clear
```

This clears the simulated location and allows the device to return to its normal location behavior.

---

# GPX Route Simulation

The DVT interface also supports GPX route playback.

Check the available options:

```bash
pymobiledevice3 developer dvt simulate-location play -h
```

The `play` command can be used to replay a GPX route through the device's developer location-simulation system.

This is useful for simulating movement rather than simply teleporting to a single coordinate.

---

# Check Available Location Commands

Show the location simulator help:

```bash
pymobiledevice3 developer dvt simulate-location -h
```

Expected commands:

```text
clear    Clear the currently simulated location
set      Set a simulated location
play     Replay a GPX route
```

Show the syntax for setting a location:

```bash
pymobiledevice3 developer dvt simulate-location set -h
```

Expected syntax:

```text
set {latitude} {longitude}
```

---

# Device Selection

By default, `pymobiledevice3` can use the first USB-connected device.

If multiple devices are connected, the command supports specifying a UDID:

```bash
pymobiledevice3 developer dvt simulate-location set \
  --udid YOUR_DEVICE_UDID \
  LATITUDE LONGITUDE
```

The `set -h` command shows the currently supported device-selection options.

---

# RemoteXPC / Tunnel Options

iOS 17+ developer services use Apple's RemoteXPC infrastructure.

The location command supports options including:

```text
--tunnel
--rsd
--userspace
--udid
```

For a normal USB-connected device, start with the default configuration.

If a tunnel is required, inspect:

```bash
pymobiledevice3 developer dvt simulate-location set -h
```

and:

```bash
pymobiledevice3 remote -h
```

Recent versions of `pymobiledevice3` can establish the required iOS 17+ developer-service tunnels.

---

# Quick Reference

## Check pymobiledevice3

```bash
pymobiledevice3 -h
```

## Check version

```bash
pymobiledevice3 version
```

## Check DVT

```bash
pymobiledevice3 developer dvt -h
```

## Check location simulation

```bash
pymobiledevice3 developer dvt simulate-location -h
```

## Set location

```bash
pymobiledevice3 developer dvt simulate-location set LATITUDE "LONGITUDE"
```

## Example

```bash
pymobiledevice3 developer dvt simulate-location set 40.690008 "-74.045843"
```

## Clear location

```bash
pymobiledevice3 developer dvt simulate-location clear
```

## GPX playback help

```bash
pymobiledevice3 developer dvt simulate-location play -h
```

---

# Location Simulator GUI

There is also an open-source graphical application based on `pymobiledevice3`:

**Location Simulator**

[https://github.com/lvalen91/Location-Simulator](https://github.com/lvalen91/Location-Simulator)

It provides features such as:

* Map-based location selection
* Teleporting
* Route planning
* Walking/cycling/driving simulation
* GPX-style route functionality
* USB device management

However, the prebuilt release architecture may not match every Mac.

For example, an ARM64 release will not run natively on an Intel Mac.

The GUI is also **not required** for GPS simulation. Direct `pymobiledevice3` commands are sufficient.

---

# Building Location Simulator From Source

If building the GUI from source, check the Mac architecture:

```bash
uname -m
```

Intel Macs:

```text
x86_64
```

Apple Silicon Macs:

```text
arm64
```

Check Xcode:

```bash
xcodebuild -version
```

Example:

```text
Xcode 16.4
Build version 16F6
```

Clone the project:

```bash
git clone https://github.com/lvalen91/Location-Simulator.git
cd Location-Simulator
```

Check available releases/tags:

```bash
git tag
```

For example:

```bash
git checkout v2.1.0
```

A source build may require additional compatibility fixes depending on the macOS/Xcode SDK combination.

If the GUI does not build, use the direct `pymobiledevice3` method described above.

---

# Troubleshooting

## `No such option: -7`

If you run:

```bash
pymobiledevice3 developer dvt simulate-location set 40.690008 -74.045843
```

and receive an error such as:

```text
No such option: -7
```

quote the negative longitude:

```bash
pymobiledevice3 developer dvt simulate-location set 40.690008 "-74.045843"
```

---

## `simulate-location` is missing

Run:

```bash
pymobiledevice3 developer dvt -h
```

If `simulate-location` is not listed, check your installed `pymobiledevice3` version:

```bash
pymobiledevice3 version
```

Then update it:

```bash
pipx upgrade pymobiledevice3
```

or, if installed through pip:

```bash
python3 -m pip install --upgrade pymobiledevice3
```

---

## Developer services are unavailable

Check:

```bash
pymobiledevice3 developer dvt -h
```

Make sure:

* iPhone is connected over USB
* iPhone is unlocked
* Mac is trusted
* Developer Mode is enabled
* Required developer services/tunnel are available

For iOS 17+, inspect:

```bash
pymobiledevice3 remote -h
```

---

## Multiple iPhones are connected

Specify the target UDID:

```bash
pymobiledevice3 developer dvt simulate-location set \
  --udid YOUR_DEVICE_UDID \
  LATITUDE "LONGITUDE"
```

---

# Important Limitations

This is **developer location simulation**, not a modification of the iPhone's physical GPS hardware.

Some applications may be able to determine that a location is being simulated.

Therefore:

* Location simulation may not behave identically to real GPS.
* Some apps may ignore simulated locations.
* Some apps may detect developer/simulated location.
* Behavior can vary between iOS versions and applications.
* Clearing the simulation restores normal location behavior.

---

# Minimal Setup

Once everything is installed, the entire workflow can be reduced to:

```bash
# Connect iPhone via USB

# Set location
pymobiledevice3 developer dvt simulate-location set 42.3601 "-71.0589"

# ...use the simulated location...

# Return to normal GPS
pymobiledevice3 developer dvt simulate-location clear
```

That's it.

---

# Tested Working Example

The following command successfully sets a simulated location on a physical iPhone running iOS 18.7.8:

```bash
pymobiledevice3 developer dvt simulate-location set 40.690008 "-74.045843"
```

The corresponding command to remove the simulation is:

```bash
pymobiledevice3 developer dvt simulate-location clear
```

Yes — and based on what you just discovered, I would **update the README to make the tunnel method the recommended method**, rather than leaving it as an optional/advanced path.

The important new working setup is:

### Recommended workflow

**Terminal 1 — keep this running:**

```bash
sudo pymobiledevice3 remote tunneld
```

You should see something like:

```text
Created tunnel --rsd fd0f:1079:eb80::1 58990
```

**Terminal 2 — set the location:**

```bash
pymobiledevice3 developer dvt simulate-location set --tunnel '' -- 42.341368 -71.104866
```

The `--` is important because your CLI otherwise interprets a negative longitude such as `-71.104866` as an option.

### I'd add this section to your README

````markdown
# Recommended iOS 17+ Tunnel Setup

For reliable location simulation on iOS 17+, the most reliable setup is to run
`pymobiledevice3`'s tunneld service in one Terminal window and use the tunnel
from a second Terminal window.

## Terminal 1 — Start tunneld

Run:

```bash
sudo pymobiledevice3 remote tunneld
````

Leave this Terminal window running.

A successful startup will look similar to:

```text
INFO: Uvicorn running on http://127.0.0.1:49151
INFO: Created tunnel --rsd fd0f:1079:eb80::1 58990
```

The exact RSD address and port will vary.

Do not close this Terminal while using location simulation.

## Terminal 2 — Set the Location

In a second Terminal window, run:

```bash
pymobiledevice3 developer dvt simulate-location set --tunnel '' -- LATITUDE LONGITUDE
```

For example:

```bash
pymobiledevice3 developer dvt simulate-location set --tunnel '' -- 42.341368 -71.104866
```

The `--` before the coordinates is important.

### Why is `--` necessary?

`pymobiledevice3` uses a command-line parser that can interpret a negative
longitude as an option.

For example:

```text
-71.104866
```

may incorrectly be interpreted as an option beginning with `-`.

Using:

```bash
-- 42.341368 -71.104866
```

tells the command-line parser that everything after `--` is positional
argument data.

## Keep Terminal 1 Running

The tunneld process is the persistent connection to the iPhone's developer
services.

Therefore:

```text
Terminal 1
sudo pymobiledevice3 remote tunneld
        │
        │
        ▼
   iPhone tunnel
        │
        ▲
        │
Terminal 2
pymobiledevice3 developer dvt simulate-location ...
```

If Terminal 1 is terminated, the tunnel will eventually disappear and
location simulation may stop working.

## Reset the Location

To clear the simulated location:

```bash
pymobiledevice3 developer dvt simulate-location clear --tunnel ''
```

If the installed version requires the device selection to be specified
differently, check:

```bash
pymobiledevice3 developer dvt simulate-location clear -h
```

## Useful Commands

Check the available DVT commands:

```bash
pymobiledevice3 developer dvt -h
```

Check location simulation:

```bash
pymobiledevice3 developer dvt simulate-location -h
```

Check the tunnel options:

```bash
pymobiledevice3 developer dvt simulate-location set -h
```

Check RemoteXPC/tunneld:

```bash
pymobiledevice3 remote -h
```

## If the Tunnel Gets Confused

If commands start behaving strangely, the simplest recovery is to restart
the Terminal processes.

1. Stop the existing `pymobiledevice3` processes with `Ctrl+C`.
2. Close/reopen the Terminal windows if necessary.
3. Start tunneld again:

```bash
sudo pymobiledevice3 remote tunneld
```

4. Open a second Terminal.
5. Set the location again:

```bash
pymobiledevice3 developer dvt simulate-location set --tunnel '' -- LATITUDE LONGITUDE
```

Example:

```bash
pymobiledevice3 developer dvt simulate-location set --tunnel '' -- 42.341368 -71.104866
```

## Important

The tunneld Terminal must remain running while using the tunnel.

The location command itself does not need to remain running indefinitely;
the tunnel/developer-service infrastructure is what maintains communication
with the device.

````

### One other change I'd make

Your current README has the direct command presented as the primary workflow:

```bash
pymobiledevice3 developer dvt simulate-location set LATITUDE LONGITUDE
````

I'd change that to:

```bash
# Terminal 1
sudo pymobiledevice3 remote tunneld

# Terminal 2
pymobiledevice3 developer dvt simulate-location set --tunnel '' -- LATITUDE LONGITUDE
```

and label the old direct method **"Simple/Legacy Method"**.

That way, when you come back to this repo months from now, the README immediately tells you **exactly what you actually got working on your Mac/iPhone**, instead of making you rediscover the tunnel setup.






---

## License / Attribution

This repository documents the use of `pymobiledevice3` and Apple's developer services.

`pymobiledevice3`:

[https://github.com/doronz88/pymobiledevice3](https://github.com/doronz88/pymobiledevice3)

Location Simulator:

[https://github.com/lvalen91/Location-Simulator](https://github.com/lvalen91/Location-Simulator)

```

**One recommendation:** put exactly that in `README.md` and keep it as the stable reference. Your repo then becomes useful in a future chat because you can simply give the README and say *“this is my working iOS spoofing setup; continue from here.”*
```
