# K2 Plus w12 Test Area

# w12 — still in testing ---------------------------------------------------------------

Use at your own risk. These settings are experimental and may not match your printer’s stock defaults. Make a backup of your config before trying anything.

What this is

A small, focused set of probing (PRTouch) and bed mesh tweaks aimed at:

Faster probing without stressing the sensor

More repeatable measurements (median of multiple samples)

A tighter tolerance for higher mesh precision

Smoother first layers by refining mesh interpolation and fade

## Backup first. Save your current printer.cfg and any included files

Configuration (printer.cfg)

# w12 — still in testing -----------------------------------------------------------------

```bash
[prtouch_v3]
#z_offset: 0
speed: 8
samples: 3
samples_result: median
samples_tolerance: 0.03
samples_tolerance_retries: 3
prth_clr_probe_pos: 150,355
step_swap_pin: !PC7
pres_swap_pin: nozzle_mcu:PA15

prth_msg_show: False
pres_cs0_pin: nozzle_mcu:PB13, nozzle_mcu:PB14
pres_tri_hold: 4000, 10000, 500

enable_not_linear_comp: False

pres_cfg_regs: 60
prth_max_chps: 8
prth_tri_zacc: 1000
prth_min_fans: 0.3

regional_prtouch_switch: True
regional_prtouch_percentage: 0.85
```

# w12 — still in testing -------------------------------------------------------------------

## Rationale: what changed & why

```bash
PRTouch ([prtouch_v3])

speed: 8 — ~60% faster than very conservative speeds (e.g. 5 mm/s). Saves time but remains gentle on the probe.

samples: 3 — Three measurements per point reduce random outliers.

samples_result: median — Median resists spikes better than average.

samples_tolerance: 0.02 — Tightens acceptable variance between samples to ±0.02 mm for a cleaner mesh.

samples_tolerance_retries: 2 — Fewer unnecessary re-probes while still re-checking when needed.

prth_msg_show: False — Keeps the console/log tidy during probing.

prth_min_fans: 0.3 — Slightly lower fan speed during probing to reduce airflow effects on the sensor.

enable_not_linear_comp: False — Disables temperature drift compensation unless you have a proper calibration curve. Avoids wrong Z-offsets from uncalibrated compensation.

regional_prtouch_switch: True / regional_prtouch_percentage: 0.8 — Enables regional probing. Intended to pair with your slicer-bounds workflow (e.g., w12_leveling.cfg) to probe only the relevant print area (here ~80% of the area), saving time without sacrificing quality where it matters.
```

# w12 modified

```bash
[bed_mesh]
speed: 120
mesh_min: 5,5
mesh_max: 345,345
probe_count: 5,5 # used by w12_leveling.cfg
mesh_pps: 3,3
fade_start: 0.25
fade_end: 8
bicubic_tension: 0.25
algorithm: bicubic
horizontal_move_z: 3
split_delta_z: 0.015
move_check_distance: 1.5
```

## Rationale: what changed & why

```bash
Bed mesh ([bed_mesh])

speed: 120 — Faster travel between probe points to reduce total probing time.

probe_count: 5,5 — A good balance of resolution vs. speed for most prints.

mesh_pps: 3,3 — Finer interpolation between points → smoother compensation.

fade_start: 0.3 / fade_end: 10 — Start fading bed compensation after the first 0.3 mm and fully fade by 10 mm height for cleaner walls and dimensions higher up.

algorithm: bicubic / bicubic_tension: 0.2 — Smooths the mesh surface; mild tension avoids over-smoothing.

horizontal_move_z: 3 — Lowers Z-hops between points to save time while staying safe.

split_delta_z: 0.02 / move_check_distance: 2 — Reasonable precision and move checks without overloading the CPU.
```

## Experimental. No warranty.

## You are responsible for any risk or damage. Always supervise your first runs after changing probing or mesh settings.

