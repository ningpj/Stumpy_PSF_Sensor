# "_Stumpy_" Proportional Sync-Feedback (PSF) Sensor for Multi Material Printing

During extended periods of synchronised printing without intervening tool changes, alignment between extruder and gear steppers can gradually drift. Even with precise calibration, factors such as under extrusion during purging, printing at high speed near upper flow limits, friction and drag and the inertia of heavy spools can conspire to cause slippage in either the extruder or more commonly—the downstream gear stepper. Over time, this mismatch accumulates and can lead to print artefacts caused by missed steps or suboptimal extrusion rates. Although not always apparent on well-tuned systems, it's a legitimate and demonstrable issue.

Proportional sync-feedback sensors address this using linear Hall Effect sensors (or similar), providing real-time analog positional telemetry to Happy Hare allowing it to continuously adjust and fine tune the following gear stepper's Rotational Distance (RD).

This is built around the PSF v1.0/1.1 or later Proportional Sync-Feedback sensor kit from **Kashine6** and is intended to be used with the **Happy Hare** MMU ecosystem and new Flowguard / Sync-feedback controller facilities (Happy-Hare `3.4.2` or later (`3.4.2` is currently in open beta).

> [!WARNING] 
> This is a **W**ork **I**n **P**rogress: This is a full redesign and isn't fully baked or "*print*" verified and expect it will need further tweaks and changes. Please continue to use steps from the `main` Stumpy FSF Sensor repo branch.

## References & Acknowledgements

This design has been completely reworked from the ground up, incorporating improvements informed by field testing of MK I and earlier adaptations of [Tshine's original dual micro-switch based Sync-feedback Sensor](https://makerworld.com/en/models/507573) by Kashine6.

The primary goals were to reduce the overall form factor, optimise sensor–magnet alignment and restrict shuttle lateral movement to maximise signal fidelity, particularly for top‑mounted MMUs and central Bowden entry ports. The assembly now measures just 63 mm in length — 9.2 mm shorter than Kashine6’s version and 3 mm shorter than MK I revision. ECAS04 captive retension design has also been improved to make it easier to install, hopefully without splitting the base.  

## Assembly

![Stumpy PSF Sensor](<Assets/Stumpy PSF Sensor.png>)

> [!NOTE] 
> Step files (rather than STLs) are included for printing the sensor with [Multi-colour](MK%20%II%20%(Full%20%redesign)/Multi_Colour_Step) tick marks or  [Single-colour](MK_II_(Full_redesign)/Single_Colour_Step) with recessed tick marks.
> Depending on MMU / Printer orientation, you may elect to invert all the parts (mirror along x axis in your slicer) to move the connector to the bottom to streamline wiring for your particular mmu.


Assembly is reasonably straight forward.

- Install ECAS fittings without optional, rubber boots
- Install D4 x 15mm magnet in PSF shuttle until it's flush with the end of the shuttle opposite the ECAS fitting (magnetic orientation doesn't matter)
- Straighten the PTFE tube you will be inserting into the "Stumpy" PSF body
- Insert the PTFE bowden tube from the left, threading it through the `20mm` spring and PSF shuttle until it bottoms out on the recess of the body. Adjust the position so it doesnt jam or pull out of the channel when held against the opposite stop. Install the ECAS clip to secure the PTFE, inserted with the 90° top towards the shuttle so it's retained, and prevented from rotating by the nub's on the shuttle
- Insert PSF 1.0/v1.1 circuit board into slot provided
- Gently slide the lid / cover from the back over the shuttle and onto the base, locating and press the parts until they snap together on the locating nubs. Be gentle, and dont force it. Secure the lid / cover with a single M2 x 6mm SHCS screw
- Verify the shuttle moves freely across its full range of movement before installing

![Assembly showing magnet ECSA clip positions](<Assets/Assembly showing magnet, spring, and ECAS clip orientation.png>)

## Bill of Materials (BOM)

| Item                            | Specification                                                | Quantity |
| ------------------------------- | ------------------------------------------------------------ | -------- |
| **PSF v1.0/1.1+ Board**         | Kit from [Aliexpress](https://www.aliexpress.com/item/1005010470743517.html) includes all the required parts below    | 1        |
| **Spring**                      | 0.4 mm × 6 mm × 20 mm, spring steel ( If longer, e,g 25mm, cut down and shorten it, tiding up the cut end with needle nose pliers   | 1        |
| **Magnet**                      | D4 mm × 15 mm N35                                            | 1        |
| **ECAS04 Bowden connector & clips**|                                                           | 2        |
| **M2×6 mm SHCS screw**          |                                                              | 1        |

## Prerequsites

Stumpy PSF needs to be used with `Happy Hare` Flowguard and **integrated** support for Proportional Feedback Sensors.
Please refer to https://github.com/moggieuk/Happy-Hare/wiki/Synchronized-Gear-Extruder2 for detailed setup and configuration instructions.

> [!NOTE] 
> You will need to switch to the Happy Hare `FLOWGUARD` branch before configuring the sensor until the `beta` concludes and is merged with the main Happy Hare release.  
>
> ```text
> cd ~/Happy-Hare
> ./install.sh -b flowguard
>
> You can siwtch back to the main Happy-Hare release by running `./install.sh -b main` but may need to back out some of the new Flowguard configuration changes manually.
> ```

## Happy Hare Configuration

`MMU_PARAMETERS.CFG`

The "Stumpy" PSF Sync-Feedback Sensor has 14mm of movement.

```yaml
sync_feedback_buffer_range: 14.5 		# Travel in "buffer" between compression/tension or one sensor and end (see above)
sync_feedback_buffer_maxrange: 14.5 	# Absolute maximum end-to-end travel (mm) provided by buffer (see above)
```

Use `MMU_CALIBRATE_PSENSOR` to determine the min and max ADC raw values returned by the sensor to set the following parameters in `MMU_HARDWARE.CFG`. Your sensor may return different values than the example below e.g.:

```yaml
sync_feedback_analog_pin: mmu:<ADC GPIO>          # The ADC pin where the proportional filament pressure sensor is installed
sync_feedback_analog_max_compression: 0.9964      # Raw sensor reading at max filament compression (buffer squeezed)
sync_feedback_analog_max_tension: 0.0046          # Raw sensor reading at max filament tension (buffer expanded)
sync_feedback_analog_neutral_point: 0.5005        # Neutral point
```

## Installation Options

![Normal Orientation, wired from top](<Assets/Normal orientation.png>)
