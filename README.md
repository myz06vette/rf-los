How to Use Mike’s RF Line of Sight Tool
Live Link: https://myz06vette.github.io/rf-los/

Site A: The transmitting station. You can use the browser to fetch your current location, enter an address manually, or pick a location on the map using the mouse. You can also provide latitude / longitude coordinates. In addition, you can set your antenna height above ground. 

Site B: The receiving station. There are several options here for lookup, and where I think the real power in the tool lies. You may not always have lat/long like other LOS tools. You may just have a ham call sign. Or a grid square. Or, you may want to see the RF path between you and a local repeater to troubleshoot link quality. All of these options are available. In addition, you can also set the receiving antenna height above ground.

Manual: Provide lat / long or a street address of the station / repeater you’re trying to hit. 
Repeater Lookup: Provide the repeater’s call-sign. This does a lookup using Repeaterbook for that repeater’s lat/long coordinates. If the repeater owner has provided that info accurately in the Repeaterbook record, we’ll use those coordinates to plot Site B. In some instances, repeater owners use a shared call-sign among more than one repeater. In those cases, the tool will present a drop-down menu for you to select the desired repeater. In all cases where the data is available, the repeater’s input and output frequency, offset, and tone(s) will also be provided. We cannot guarantee the accuracy of the lat/long data for repeater location, since that is provided (or not provided) by the repeater owner. In some cases, they intentionally obscure the exact location, so you may need to pick a point manually to get more accurate data. 
Licensee Lookup: Provide the FCC amateur call-sign and we’ll lookup the registered address, lat/long, and maidenhead grid square of the licenseholder. Then, this data will be used to plot Site B. 
Grid Square: Useful for POTA / SOTA activations. Provide the 4 or 6-digit maidenhead grid square and we’ll plot the centerpoint of that grid square as Site B. 

Transmitter Power / Antenna Gain (Optional)

TX Output: Enter power output (in watts) of your transmitter. 
Line Loss: Enter the line loss in either dB or watts. You can test this with a simple, inexpensive meter like the Surecom SW-102 by placing it at the end of your coax run at the connection to the antenna, and transmitting on your desired power setting. 
TX Antenna: Select an antenna from the list of options, or enter a custom gain. This allows the tool to calculate your total ERP. 
Receive Station Sensitivity: If you know the type of radio of the other station you’re attempting to contact, you can select that here. This will attempt to calculate the likelihood of a successful link and the receiving station’s ability to hear your contact. 

RF Band: Select from 1 of 4 RF bands for your contact. 

VHF Lo: This is the radio spectrum between 30 and 88 Mhz. Useful for 6 and 10 meter HF, although in practice, this tool isn’t specifically designed with the nuances of HF in mind, so results and projections may vary. 
VHF Hi: This would be the 2-meter spectrum between 136 and 174mhz. This would also include business band LMR, marine VHF, and MURS.
UHF Lo: This would include 70-centimeter spectrum between 400 and 512 mhz. This would also include GMRS and business band LMR. 
UHF Hi: This is 700 - 900 mhz spectrum, useful for 7/800 mhz, and although slightly outside the stated range, could be marginally useful for predicting Meshtastic contacts on the 915 mhz ISM band. Your mileage may vary. 
 
How to Read the Results

Up front, the tool will give you a plain-English predicted outcome using all available RF path data between the plotted two points. Possible outcomes that can be displayed: 

Excellent: Full line of sight, first Fresnel zone completely clear (ν ≤ −0.78)
Good: LOS clear but with minor or partial Fresnel zone intrusion (ν ≤ 0.5)
Fair: Marginal LOS or shallow diffraction path — signal likely usable but reduced (ν ≤ 1.5)
Poor: Heavy diffraction — marginal or intermittent contact (ν ≤ 2.5)
Unlikely: Severe obstruction — contact improbable (ν > 2.5)

ν (nu) is the Fresnel-Kirchhoff diffraction parameter from ITU-R P.526. Values below zero mean terrain is actually below the line of sight; the more positive it gets, the deeper the obstruction.

A few additional nuances about how the tool presents the Results information:

GOOD covers two distinct sub-cases internally (minor vs. partial Fresnel intrusion) but both display the same label and green indicator.

FAIR similarly covers two sub-cases: one where LOS technically exists but is marginal, and one where it's a true diffraction path.

The terrain-corrected link budget (Deygout + roughness penalty) can show a failing budget even on a GOOD or EXCELLENT geometry. Those are independent assessments. 

Link Quality Estimate: The overall assessment bar and label (EXCELLENT / GOOD / FAIR / POOR / UNLIKELY) at the top of the results. The colored progress bar is a visual indicator of signal path quality; green for clear paths, sliding through yellow and orange to red for heavily obstructed ones. This is the tool’s “best estimate” at your likelihood of making contact.

ν (nu) · Est. diffraction loss
ν is a dimensionless number (the Fresnel-Kirchhoff diffraction parameter) that describes how severely terrain interrupts the signal path. Negative values mean terrain is safely below the line of sight. Values above zero mean terrain is intruding, and the higher the number, the worse the obstruction. The diffraction loss figure (in dB) is how much signal strength is estimated to be lost due to that terrain intrusion, calculated using the ITU-R P.526 knife-edge model. 

Band · Worst obstacle
The frequency band you've selected (e.g. UHF High), which affects how diffraction loss is calculated since lower frequencies bend around obstacles better than higher ones. "Worst obstacle" is how many feet the highest terrain point sits above or below the straight line-of-sight between your two antennas.

Path Distance: 
The straight-line over-ground distance between Site A and Site B, shown in both miles and kilometers.

Bearing: 
The compass direction from Site A to Site B, in degrees (0° = North, 90° = East, 180° = South, 270° = West).

Elev at A / Elev at B
The ground elevation at each site as reported by the terrain dataset. This is the elevation of the ground itself, not including your antenna height above ground (AGL).

Max Terrain: 
The highest ground elevation found anywhere along the path between the two sites. This is the peak that matters most for obstruction analysis.

Earth Bulge: 
Because the Earth is curved, the midpoint of a long path is physically higher relative to a straight line drawn between the two endpoints. This value is how much the Earth's curvature "bulges up" into your signal path at its worst point. The tool uses a 4/3 Earth radius factor (the standard RF engineering approximation that accounts for typical atmospheric refraction bending the signal slightly downward, effectively making the Earth appear less curved than it really is).

Min Clearance: 
The tightest margin between your line of sight and the terrain (including Earth bulge) at any point along the path. Positive means your signal clears that point; negative (shown in orange/red with "BELOW LOS") means terrain is physically above your line of sight at that point, causing diffraction or blockage.

Diffraction Loss (Est):
The estimated signal loss in decibels caused by terrain obstructing the path, calculated using the single knife-edge diffraction model. This is a theoretical lower bound; real-world loss is often higher, especially over broad ridges or multiple successive hills. This figure feeds directly into the link budget's terrain loss row.

Elevation Profile Chart 

This interactive chart shows the elevation of Site A and Site B on opposing ends of the graph, with the terrain between those two points visualized along the direct path. Put simply: this is how the terrain levels vary along the entire “as the crow flies” path. You can slide your mouse along the path to see specific elevation data. As you do so, you’ll notice an indicator moves along the RF path in the map view above, which is useful to show you exactly where obstructions are. 
