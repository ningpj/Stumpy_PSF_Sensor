# "Stumpy" PSF Sync-Feedback Sensor for Multi Material Printing

During extended periods of synchronised printing without intervening tool changes, alignment between extruder and gear steppers can gradually drift. Even with precise calibration, factors such as under extrusion during purging, printing at high speed near upper flow limits, friction and drag and the inertia of heavy spools can conspire to cause slippage in either the extruder or more commonly—the downstream gear stepper. Over time, this mismatch accumulates and can lead to print artefacts caused by missed steps or suboptimal extrusion rates. Although not always apparent on well-tuned systems, it's a legitimate and demonstrable issue.

Proportional sync-feedback sensors address this using linear Hall Effect sensors (or similar), providing analog positional telemetry to Happy Hare allowing it to continuously adjust and fine tune gear stepper Rotational Distance (RD).

This redesign is based around the PSF v1.1 or later proportional sensor kit from **Kashine6** and intended to be used with the **Happy Hare** MMU ecosystem and Flowguard / Sync-feedback controller.

## References & Acknowledgements

- This design is based on the original works of [Tshine's Filament Sync Sensor](https://makerworld.com/en/models/507573) and PSF adaption by [Kashine6](https://github.com/kashine6/Proportional-Sync-Feedback-Sensor?tab=readme-ov-file).

## Assembly

![Stumpy PSF Sensor](<Assets/Stumpy PSF Sensor.jpeg>)

> [!NOTE] 
> Step files (rather than STLs) are included for printing the sensor with [Multi-colour](Multi_Colour_Step) tick marks or  [Single-colour](Single_Colour_Step) with recessed tick marks.


Assembly is reasonably straight forward.

- Install ECAS fittings without rubber boots
- Install D4 x 15mm magnet in PSF shuttle until it's flush with the end of the shuttle (magnetic orientation doesn't matter)
- Straighten the PTFE you will be inserting into the Stumpy PSF body
- Insert the PTFE tube from the right, threading through the spring and PSF shuttle until it bottoms out in recess on the body. Install the ECSA clip to secure
- Hold the shuttle up against the stop on the left of the body and using a small screw driver or hex key, push the magnet until it bottoms out in the pocket on the body. Once in position, it should sit proud of the PSF shuttle by 4mm
- Use 2 M2 x 6mm SHCS screws to install the PSF module
- Use a M2 x 6mm SHCS screw to secure the cover
- Verify the shuttle moves freely across its full range of movement

![alt text](<Assets/Assembly showing magnet recess.jpeg>)

## Bill of Materials (BOM)

| Item                            | Specification                                                | Quantity |
| ------------------------------- | ------------------------------------------------------------ | -------- |
| **PSF v1.1+ Board**             | Kit from [Aliexpress](https://www.aliexpress.com/item/1005010470743517.html)     | 1        |
| **Spring**                      | 0.4 mm × 6 mm × 20 mm, spring steel                          | 1        |
| **Magnet**                      | D4 mm × 15 mm N35                                            | 1        |
| **ECAS04 Bowden connector & clips**|                                                           | 2        |
| **M2×6 mm SHCS screw**          |                                                              | 3        |

## Prerequsites

This needs to be used with `Happy Hare` Flowguard and **integrated** support for Proportional Feedback Sensors.
Please refer to https://github.com/moggieuk/Happy-Hare/wiki/Synchronized-Gear-Extruder2 for detailed setup and configuration instructions.

> ![IMPORTANT]
> You will need to switch to the Happy Hare `FLOWGUARD` branch before configuring the sensor until the `beta` is complete and merged with the main Happy Hare release.  
>
> ```text
> cd Happy-Hare
> ./install.sh -b FLOWGUARD
> ```

## Happy Hare Configuration

`MMU_PARAMETERS.CFG`

The "Stumpy" PSF Sync-Feedback Sensor has 14mm of movement.

```yaml
sync_feedback_buffer_range: 14 		# Travel in "buffer" between compression/tension or one sensor and end (see above)
sync_feedback_buffer_maxrange: 14 	# Absolute maximum end-to-end travel (mm) provided by buffer (see above)
```

Use `MMU_CALIBRATE_PSENSOR` to determine the min and max ADC raw values returned by the sensor to set the following parameters in `MMU_HARDWARE.CFG`. Your sensor may return different values than the example below e.g.:

```yaml
sync_feedback_analog_pin: mmu:<ADC GPIO>          # The ADC pin where the proportional filament pressure sensor is installed
sync_feedback_analog_max_compression: 0.0038      # Raw sensor reading at max filament compression (buffer squeezed)
sync_feedback_analog_max_tension: 0.9919          # Raw sensor reading at max filament tension (buffer expanded)
sync_feedback_analog_neutral_point: 0.4979        # Neutral point
```
