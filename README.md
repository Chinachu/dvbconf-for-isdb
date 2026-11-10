# DVB Configurations for ISDB
DVBV5 channel config files like recpt1 for ISDB.

## Usage
```
dvbv5-zap -a <adapter#> -c dvbv5_channels_isdbs.conf -r -P <channel>
dvbv5-zap -a <adapter#> -c dvbv5_channels_isdbt.conf -r -P <channel>
```

## Files
| File | Delivery system | Target |
| --- | --- | --- |
| `dvbv5_channels_isdbs.conf` | `ISDBS` | BS / 110CS |
| `dvbv5_channels_isdbt.conf` | `ISDBT` | Terrestrial (`13`-`62`) and CATV pass-through (`1`-`12`, `C13`-`C63`) |
| `dvbv5_channels_dvbc-a.conf` | `DVBC/ANNEX_A` | CATV trans-modulation (J.83 Annex C, 6 MHz raster, 5.274 Msym/s), for tuners that only offer Annex A |
| `dvbv5_channels_dvbc-c.conf` | `DVBC/ANNEX_C` | Same channels as `dvbc-a`, for tuners that support Annex C |
| `dvbv5_channels_dvbc-b_sky-hikari.conf` | `DVBC/ANNEX_B` | SKY PerfecTV! Premium Service Hikari (256QAM, 5.6 Msym/s) |
| `dvbv5_channels_dvbs2_sky.conf` | `DVBS2` | SKY PerfecTV! Premium Service (JCSAT-3A 128E / JCSAT-4B 124E) |

`sky` corresponds to `type: SKY` of Mirakurun.

### Notes on `dvbv5_channels_dvbs2_sky.conf`
This file does **not** work with stock `dvbv5-zap`. libdvbv5 has no LNBf
definition for this service and does not drive its LNB control signals:

- LO frequency: 11200 MHz
- Polarization: selected by LNB voltage
- Satellite: selected by 22 kHz tone (off = JCSAT-3A, on = JCSAT-4B)

It is provided as a transponder table for tools that handle the LNB
themselves. All transponders are listed with the DVB-S2 parameters of the
service (8PSK, 23.303 Msym/s, FEC 3/5); not every transponder is
necessarily on air.

Tools such as [szap-s2j](https://github.com/kanreisa/szap-s2j) can drive
this LNB, but they use their own channel file format and cannot read this
file directly.
