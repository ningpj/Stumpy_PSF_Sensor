# "_Stumpy_" Proportional Sync-Feedback (PSF) Sensor for Multi Material Printing

During extended periods of synchronised printing without intervening tool changes, alignment between extruder and gear steppers can gradually drift. Even with precise calibration, factors such as under extrusion during purging, printing at high speed near upper flow limits, friction and drag and the inertia of heavy spools can conspire to cause slippage in either the extruder or more commonly—the downstream gear stepper. Over time, this mismatch accumulates and can lead to print artefacts caused by missed steps or suboptimal extrusion rates. Although not always apparent on well-tuned systems, it's a legitimate and demonstrable issue.

Proportional sync-feedback sensors address this using linear Hall Effect sensors (or similar), providing real-time analog positional telemetry to Happy-Hare allowing it to continuously adjust and fine tune the following gear stepper's Rotational Distance (RD).

This is built around the excellent `PSF v1.0 / 1.1` or later Proportional Sync-Feedback sensor kit from **Kashine6** (discord @jacksky6) and is intended to be used with the **Happy-Hare** MMU ecosystem and new Flowguard / Sync-feedback controller facilities (Happy-Hare `3.4.2` or later (`3.4.2` is currently in open beta). It has a total of `14.5 mm` of buffer range and will work with `20 mm` or `25 mm` springs from the kit (`20 mm` prefered).

## References & Acknowledgements

This design has been completely reworked from the ground up, incorporating improvements informed by field testing of MK I and the earlier adaptations of [Tshine's original dual micro-switch based Sync-feedback Sensor](https://makerworld.com/en/models/507573) by [Kashine6](https://github.com/kashine6/Proportional-Sync-Feedback-Sensor?tab=readme-ov-file) to support Proportional Sync-Feedback sensors.

The primary goals have been to further reduce the overall form factor, optimise sensor–magnet alignment and constrain shuttle lateral movement to maximise Hall Affect ADC signal fidelity, particularly for top‑mounted MMUs and central Bowden entry ports. The assembly now measures just `63 mm` in length — `9.2 mm` shorter than Kashine6’s version and `3 mm` shorter than MK I revision. The `ECAS04` captive retention design has also been refined to make it easier to install them, hopefully without splitting the base. Printable `ECAS04` clips have also been included to colour coordinate ;-)  

## Assembly

![Stumpy PSF Sensor](<Assets/Stumpy PSF Sensor.png>)

> [!NOTE] 
> Step files (rather than STLs) are included for printing the sensor with [Multi-colour](MK%20II%20(Full%20redesign)/Multi_Colour_Step) tick marks or [Single-colour](MK%20II%20(Full%20redesign)/Single_Colour_Step) with recessed tick marks. Also consider lowering your 1st layer speed as the coloured elements are reasonably narrow to ensure tick marks are crisp and clean. 
> Depending on MMU / printer orientation, you may also consider inverting all parts (mirror along x axis in your slicer) to relocate the connector to the bottom of the unit to optimise wiring for your particular mmu setup.


Assembly is reasonably straight forward:

- Install the `ECAS04` fittings without the optional rubber boot. You still need the hard plastic clasp cover on the base though.
- Press the `D4 × 15 mm` magnet into the PSF shuttle until it sits flush with the end opposite the `ECAS04` fitting. Magnet orientation doesn’t matter; Happy-Hare will calibrate it.
- Straighten the PTFE tube you’ll use between the MMU and the “Stumpy” PSF sensor. Avoid PTFE with an `ID > 2.5 mm` when fitting close to the MMU if it has to bend to accomodate the full range of gates / lanes as kinks can jam the shuttle, which must move freely for any selected gate.
- Insert the  `2.0 – 2.5 mm ID` PTFE feeder tube from the left, feeding it through the `20 mm` spring (or `25 mm` if this is all you have) and shuttle until it bottoms out in the body recess. Adjust so it neither binds nor pulls out when held against the opposite stop. Install the colour coordinated `ECAS04` clip with the 90° top facing the shuttle so it’s retained and prevented from rotating by the nubs provided.
- Insert the `PSF 1.0 / v1.1` PCB module into the provided slot.
- Slide the lid / cover on from the back over the shuttle and onto the base, aligning the parts and snapping them gently onto the locating nubs. Do not force it. Secure with a single `M2 × 6 mm SHCS`.
- Confirm the shuttle moves freely across its full travel before installing.

![Assembly showing magnet ECSA clip positions](<Assets/Assembly showing magnet, spring, and ECAS clip orientation.png>)

## Bill of Materials (BOM)

| Item                            | Specification                                                | Quantity |
| ------------------------------- | ------------------------------------------------------------ | -------- |
| **PSF v1.0/1.1+ Board**         | Inexpensive kit from [Aliexpress](https://www.aliexpress.com/item/1005010470743517.html) that includes all the required parts below    | 1        |
| **Spring**                      | 0.4 mm × 6 mm × 20 mm, spring steel ( If longer, e,g 25mm, cut down and shorten it, tiding up the cut end with needle nose pliers   | 1        |
| **Magnet**                      | D4 mm × 15 mm N35                                            | 1        |
| **ECAS04 Bowden connector & clips**|                                                           | 2        |
| **M2×6 mm SHCS screw**          |                                                              | 1        |
| **PTFE feeder tube**                | Short PTFE feeder tube installed between the “Stumpy” PSF Sensor and the MMU (2.5 mm ID recommended) - 3 mm ID PTFE tube is too soft and will deform or jam if its bent too much. |          |

## Prerequsites

Stumpy PSF needs to be used with `Happy-Hare` Flowguard and **integrated** support for Proportional Feedback Sensors (Release `3.4.2` or later).
Please refer to https://github.com/moggieuk/Happy-Hare/wiki/Synchronized-Gear-Extruder2 for proportional sensor setup, configuration, and usage instructions.

> [!NOTE] 
> You will need to manually switch across to the Happy-Hare `FLOWGUARD` branch before configuring the sensor until the `beta` concludes and is merged with the main Happy-Hare release.  
>
> ```text
> cd ~/Happy-Hare
> ./install.sh -b flowguard
> ```
> You can switch back at any time by running `./install.sh -b main` but will need to manually undo some of the new Flowguard configuration options & renamed Happy-Hare parameters (It's not difficult, just a reminder).

## Happy-Hare Configuration

`MMU_PARAMETERS.CFG`

The "Stumpy" PSF Sync-Feedback Sensor has 14.5mm of buffer / sensor range.

```yaml
sync_feedback_buffer_range: 14.5 		# Travel in "buffer" between compression/tension or one sensor and end (see above)
sync_feedback_buffer_maxrange: 14.5 	# Absolute maximum end-to-end travel (mm) provided by buffer (see above)
```

In `MMU_HARDWARE.CFG` set `sync_feedback_analog_pin: mmu:<ADC GPIO>` to a valid analog-capable GPIO port on your MCU you have connected the "Stumpy" PSF sensor to. [Kashine6](https://github.com/kashine6/Proportional-Sync-Feedback-Sensor?tab=readme-ov-file) provides a breakdown on their GITHUB of valid ADC GPIO ports for many popular MMU MCU's.  Note that while many boards offer multiple ADC-capable GPIO's, only one port has been fully verified and documented for each board.

Restart Klipper, load a gate with filament and run `MMU_CALIBRATE_PSENSOR` to determine the minimum and maximum ADC values returned by your PSF v1.0 / v1.1 Sensor module. Use these values to configure the corresponding parameters in `MMU_HARDWARE.CFG`. Your sensor may produce different—or inverted—readings compared to the example below. This is expected, and Happy-Hare can accommodate it.

```yaml
sync_feedback_analog_pin: mmu:<Your analog-capable GPIO> # The ADC pin where the proportional filament pressure sensor is installed
sync_feedback_analog_max_compression: 0.9964             # Raw sensor reading at max filament compression (buffer squeezed)
sync_feedback_analog_max_tension: 0.0046                 # Raw sensor reading at max filament tension (buffer expanded)
sync_feedback_analog_neutral_point: 0.5005               # Sensor neutral point (tunable to apply positive tension on the filament to unburden the Bowden and extruder)
```

## Installation Options

![Normal Orientation, wired from top](<Assets/Normal orientation.png>)
