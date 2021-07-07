# ghn-tweak

A simple command line tool for configuring [G.hn](https://en.wikipedia.org/wiki/G.hn) [powerline network adapters](https://homegridforum.org/certified-systems/) that use the [common MaxLinear chipsets](https://www.maxlinear.com/products/connectivity/wired/g-hn), including G.hn devices from [Comtrend](https://us.comtrend.com/g-hn-powerline/), [Zyxel](https://www.zyxel.com/us/en/products_services/home-powerline_and_adapters.shtml)*, [NexusLink](https://nexuslinkusa.com/discover-nexuslink/g-hn-powerline-everything-you-need-to-know/), and others.

**Zyxel's G.hn products are uncertified.*

## Usage

```
ghn-tweak <host> <password> [<key>[=<value>]]
```

**host** is the IP address or hostname of your G.hn device, e.g. `192.168.0.5`

**password** is the admin password for the device, e.g. `admin`

**key** refers to the setting name.  If the key is omitted, a list of all settings is dumped.

**value** is the new value to save for the setting.  If value is omitted, the current value of the setting is returned.

### Examples

```bash
# List the whole configuration and status
ghn-tweak 192.168.0.5 admin

# Get the device's Ethernet MAC address
ghn-tweak 192.168.0.5 admin SYSTEM.PRODUCTION.MAC_ADDR

# Enable power-saving on Ethernet inactivity
ghn-tweak 192.168.0.5 admin POWERSAVING.GENERAL.MODE=2
```

### Installation

`ghn-tweak` is just a shell script with minimal dependencies: bash v4 or later, curl, and POSIX standard utilities including grep, sed, and tr.  Download it, make it executable, and drop it in your path.

## Compatibility

Tested on:

* Comtrend PG-9182PT
* Comtrend PG-9182PoE

Should work on most MaxLinear-based G.hn adapters from Comtrend, Zyxel, NexusLink, etc.  
Please share your experiences.

#### CAUTION

* This is an **unofficial tool** that accesses **unsupported and undocumented** settings, including settings that were never meant to be changed.
* This tool allows you to modify a device that interacts with **high-voltage, high-current *electricity***.  This tool **will not warn you** or prevent you from doing **dangerous things**.
* Changing some of these settings will lead to unhappy outcomes like permanently damaging your device or blowing your circuit breaker. This will definitely **void your device's warranty**.
* Do not integrate this tool into another end-user product or utility.  It'll definitely break.

## Q&A

### What settings can be changed?

This tool uses the same API as the web UI to read and write settings.  All or most settings shown the web UI can be changed, as well as dozens of hidden settings.

Some settings include:

* changing the device's IP address (DHCP, IPv6)
* turning on/off power saving
* pairing devices with shared encryption keys
* modifying MAC address allow/deny lists
* modifying VLAN configuration
* modifying QoS configuration
* a lot of deep and dangerous stuff channel gain, coexistence, flow control, boost thresholds, and master selection 

### What statistics or states can be read?

In addition to settings, some info available for reading includes:

* CPU, memory, temperature
* physical interface stats like rx/tx collisions & errors
* throughput to each neighboring G.hn device
* uptime

### Is there a guide to these settings?

No. These are proprietary settings from the MaxLinear firmware. Zero of these are documented.

### If G.hn is an open standard, why rely on a clunky private API?

G.hn can supposedly be managed with LCMP, the Layer 2 Configuration Management Protocol, but open source tooling for this doesn't appear to exist.  While some tools exist for HomePlug/AV2 devices, G.hn hasn't caught up yet.
