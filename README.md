# "Stumpy" PSF Sync-Feedback Sensor for Multi Material Printing

During extended periods of synchronised printing without intervening tool changes, alignment between extruder and gear steppers can gradually drift. Even with precise calibration, factors such as under extrusion during purging, 
printing at high speed near upper flow limits, friction and drag and the inertia of heavy spools can conspire to cause slippage in either the extruder or more commonly—the downstream gear stepper. Over time, this mismatch accumulates
and can lead to print artefacts caused by missed steps or suboptimal extrusion rates. Although not always apparent on well tuned systems, it is a legitimate and demonstrable issue.

Proportional sync-feedback sensors address this using linear Hall Effect sensors (or similar) to provide analog positional data to Happy Hare allowing it to continuously adjust and fine tune gear stepper Rotational Distance (RD).
This design is based on the PSF v1.1 or later proportional sensor kit from Kashine6.



## References & Acknowledgements

- This design is based on the orginal works of [Tshine's Filament Sync Sensor](https://makerworld.com/en/models/507573) and PSF adaption by [Kashine6](https://github.com/kashine6/Proportional-Sync-Feedback-Sensor?tab=readme-ov-file).

## Bill of Materials (BOM)

| Item                            | Specification                                                | Quantity |
| ------------------------------- | ------------------------------------------------------------ | -------- |
| **PSF v1.1+ Board**                   | Kit from [Aliexpress](https://www.aliexpress.com/item/1005010470743517.html)     | 1        |
| **Spring**                      | 0.4 mm × 6 mm × 20 mm, spring steel                          | 1        |
| **Magnet**                      | D4 mm × 15 mm N35                                            | 1        |
| **ECAS04 Bowden connector & clips**     | —                                                            | 2        |
| **M2×6 mm SHCS screw**          |                                                              | 3        |

## 🔧 Configuration & Calibration

At the moment, `Happy-Hare` has **integrated** support for Proportional Feedback Sensors, and the relevant Wiki documentation has already been published. 

👉➡️:  https://github.com/moggieuk/Happy-Hare/wiki/Synchronized-Gear-Extruder2 

For more detailed configuration, please refer to the HappyHare Wiki.


