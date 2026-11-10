# DVB Configurations for ISDB
DVBV5 channel config files like recpt1 for ISDB.

## Usage
```
dvbv5-zap -a <adapter#> -c dvbv5_channels_isdbs.conf -r -P <channel>
dvbv5-zap -a <adapter#> -c dvbv5_channels_isdbt.conf -r -P <channel>
```

## Files per reception route
`dvbv5_channels_<route>.conf` holds the channels of one reception route.
The route names are those of Mirakurun (`TER`, `SAT`, `SKY`, `CATV`, `FTTH`).

| File | Route | Channels |
| --- | --- | --- |
| `dvbv5_channels_ter.conf` | Terrestrial antenna | `1`-`12`, `13`-`62`, `C13`-`C63` (ISDB-T) |
| `dvbv5_channels_sat.conf` | BS / 110CS antenna | `BSxx_y`, `CSx` (ISDB-S), BS 4K / 8K (ISDB-S3) |
| `dvbv5_channels_sky.conf` | 124 / 128 CS antenna | `JCSAT3A-TPxx`, `JCSAT4B-TPxx` (DVB-S2) |
| `dvbv5_channels_catv.conf` | Cable TV (trans-modulation) | `1`-`12`, `C13`-`C63`, `13`-`62` (J.83 Annex C, tuned as `DVBC/ANNEX_A`) |
| `dvbv5_channels_ftth.conf` | Optical TV | `13`-`62`, `BSxx_y`, `CSx` (pass-through), SKY PerfecTV! Premium Service Hikari (J.83 Annex B) |

`dvbv5_channels_isdbs.conf` and `dvbv5_channels_isdbt.conf` are kept as they
were for existing setups. New entries are only added to the files above.

### Channels with a shifted frequency
Some cable and optical systems carry a few channels on a shifted frequency.
These are listed at the end of each file under a separate name, so that the
default channel names keep their frequency.

- `dvbv5_channels_ter.conf`, `dvbv5_channels_catv.conf`:
  `4-` - `7-` and `C22-` are 2 MHz lower than `4` - `7` and `C22`.
  `C24+` - `C27+` are 2 MHz higher than `C24` - `C27`.
- `dvbv5_channels_ftth.conf`: `159250`, `165750` (1 MHz higher than `158250`,
  `164750`) and `259750`, `266250`, `272750` (4 MHz higher than `255750`,
  `262250`, `268750`).

### `dvbv5_channels_sat.conf`
- `BSxx_y` and `CSx` are the channel names of recpt1, the same as in
  `dvbv5_channels_isdbs.conf`. Slots that are no longer on air are not listed.
- BS 4K / 8K entries (`BS07_*`, `BS11_*`, `BS17_*`, `BS14_0`) carry an ISDB-S3
  stream ID (`0xbXXX`) with `DELIVERY_SYSTEM = ISDBS`. The Linux DVB API has no
  delivery system for ISDB-S3, so this only works with drivers that follow
  this convention, such as
  [tbs6812_drv](https://github.com/otya128/tbs6812_drv) (TBS6812 / PT4K).
  The output is a TLV/MMT stream, not MPEG-TS.
- `BS14_0` (8K) is left-hand circular polarized and needs an antenna and
  cabling that support it.
- The second stream of BS11 is not listed, because its stream ID could not be
  confirmed.

### `dvbv5_channels_sky.conf`
This file does **not** work with stock `dvbv5-zap`. libdvbv5 has no LNBf
definition for this service and does not drive its LNB control signals:

- LO frequency: 11200 MHz
- Polarization: selected by LNB voltage
- Satellite: selected by 22 kHz tone (off = JCSAT-3A, on = JCSAT-4B)

All transponders are listed with the DVB-S2 parameters of the service
(8PSK, 23.303 Msym/s, FEC 3/5); not every transponder is necessarily on air.

### `dvbv5_channels_ftth.conf`
- The left-hand circular polarized `BS14_0` is not listed.
- The SKY PerfecTV! Premium Service Hikari entries (256QAM, 5.6 Msym/s) are
  named after their frequency in kHz (`73750`-`769250`).
