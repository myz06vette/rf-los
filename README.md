# Mike's Ham Radio Mapping Tools

**A free, browser-based toolkit for amateur radio operators — no account, no install, no subscription.**

---

This tool was built by **N4MXB** for the ham radio community. It combines two purpose-built utilities in a single application: a **VHF/UHF RF Line-of-Sight calculator** and an **HF contact logger with mapping**. Both tabs are designed around real operating scenarios — whether you're planning a repeater link, scouting a portable operating site, chasing POTA and SOTA activations, or simply keeping a visual log of your HF contacts.

All computation runs locally in your browser. No data is sent to any server except for optional callsign, repeater, POTA, and SOTA lookups, which are proxied through a Cloudflare Worker to handle cross-origin restrictions.

---

## 📡 VHF / UHF Tab — RF Line-of-Sight Calculator

The VHF/UHF tab calculates whether two stations can communicate directly over a given terrain path. It models earth curvature, real terrain elevation, Fresnel zone obstruction, and knife-edge diffraction loss to give you a link quality assessment along with a visual elevation profile.

### How to Use It

1. **Set Site A — Your Station.** Enter your address or place name in the Site A field and press Enter, or click *Use My GPS* to use your device's location. You can also drag the marker directly on the map or click *Pick on Map*. Set your antenna height above ground level (AGL) in feet.

2. **Set Site B — The Remote Station.** Choose how you want to locate the remote site using the mode buttons:

   - 📍 **Address** — Type any address, landmark, or place name and press Enter.
   - 👤 **Licensee** — Enter a ham callsign (e.g. W4ABC). The tool looks up the FCC license record and places the marker at the operator's registered address. Works for amateur licensees; GMRS callsigns will return a link to the FCC ULS search instead.
   - 🔲 **Grid Sq.** — Enter a 4 or 6 character Maidenhead grid square (e.g. EM74 or EM74rj). The marker is placed at the center of that grid square.
   - 📡 **Repeater** — Enter a callsign and look up repeater listings from RepeaterBook. Select a specific repeater from the results to place the marker at its coordinates.
   - ⛰️ **SOTA** — Enter a SOTA summit reference (e.g. W4G/NG-001) or a partial code like NG-001. If a full reference is found, the marker is placed at the summit's exact coordinates. If the input is partial or ambiguous, a *Did You Mean?* list appears with the closest matching summits based on your current location and the text entered.

3. **Select Your RF Band.** Choose the frequency band that matches your operating frequency: VHF Low (30–88 MHz), VHF High (136–174 MHz), UHF Low (400–512 MHz), or UHF High (700–900 MHz). This determines the wavelength used in Fresnel zone calculations.

4. **Calculate.** Press the *Calculate LOS* button. The tool fetches terrain elevation data, plots the path profile, and returns a full link assessment.

5. **Read the Results.** The elevation profile shows your path with a terrain cross-section. The result panel shows distance, path clearance, Fresnel zone radius at the most obstructed point, estimated diffraction loss (if any), and a qualitative link quality rating. Contacts are saved to a history log at the bottom of the panel.

### Optional Fields and Tips

- Antenna heights default to 33 ft AGL. Adjust both sites to reflect actual mast or tower heights for accurate results.
- The Licensee lookup places the marker at the operator's *registered address*, which may differ from their actual antenna location. Use the map drag or manual coordinate entry to fine-tune.
- The SOTA lookup places the marker at the summit's GPS coordinates — useful for evaluating whether a hilltop or mountain activation is likely to reach your QTH on VHF/UHF.
- History entries are saved in your browser's local storage. They persist across sessions on the same device.

---

## 📻 HF Tab — Contact Logger and Mapping

The HF tab is a visual contact logging tool designed around chasing POTA (Parks on the Air) and SOTA (Summits on the Air) activations, as well as general HF operating. It plots contacts on a satellite map with great-circle lines between your station and each heard station, calculates distances, and exports logs in standard formats.

### How to Use It

1. **Set Up My Station — Site A.** Enter your callsign and click *Home QTH* to look up your registered address from the FCC database. Click *Use My GPS* to use your current device location instead — useful when operating portable. If you're activating a POTA park, enter the park reference in the *Activating Park* field; the map marker will move to the park's coordinates and your park reference will be included in log exports.

2. **Enter the Heard Station — Site B.**

   - 👤 **Callsign** — Always required. Enter the callsign of the station you heard and press *Look Up*. The tool retrieves the operator's name, license class, and registered QTH from the FCC database via callook.info.
   - 🌲 **POTA Park** *(optional — overrides callsign QTH)* — If the station is a POTA activator, enter their park reference (e.g. US-0001 or just 0001). The tool looks up the park from the POTA API and moves the contact marker to the park's coordinates. The park name and location appear on the log card.
   - ⛰️ **SOTA Summit** *(optional — overrides callsign QTH)* — If the station is a SOTA activator, enter the summit reference (e.g. W4G/NG-001 or just NG-001). The tool looks up the summit from the SOTA database and moves the contact marker to the summit's coordinates. Elevation and points are shown on the log card. Partial codes trigger a *Did You Mean?* list using your station location as a proximity clue.
   - 🔲 **Grid Square** *(optional — overrides callsign QTH)* — Enter a Maidenhead grid square if you know the operator's grid but not their exact address.

3. **All three optional fields run in parallel.** When you press *Look Up*, the callsign lookup, POTA lookup, and SOTA lookup all run simultaneously. Priority order for location: SOTA overrides POTA overrides Grid Square overrides callsign QTH. The operator's name always comes from the callsign lookup regardless of which location source wins.

4. **Fill in Log Entry details.** Select band, frequency, mode, and RST sent. Add notes if needed. Press *Log Contact* to save.

5. **Review the Map and Log.** Each contact is plotted with a green dot at the remote station and a great-circle line back to your QTH. The log panel on the right shows all contacts with callsign, name, license class, POTA/SOTA details, distance, band, and timestamp. Click any card to reload that contact.

6. **Export.** Use *Export Log — ADIF* for import into QRZ logbook, LoTW, HAMRS, or any standard logging software. Use *Export Log — CSV* for spreadsheets. ADIF includes standard `SOTA_REF` and `SIG`/`SIG_INFO` fields for POTA and SOTA chaser credits.

### Show All Toggle

The *Show All* toggle in the log header controls whether all contacts are plotted on the map simultaneously or only the currently selected one. With Show All on, every contact in your log is visible as a green dot with a line to your QTH — useful for visualizing your operating session geographically.

---

## ⚙️ Models, Methodology, and Data Sources

### Elevation Data

Terrain elevation is sampled along the path between Site A and Site B using the **Open-Elevation API**, which sources data from the SRTM (Shuttle Radar Topography Mission) dataset at approximately 90-meter horizontal resolution. A fallback to **Open-Meteo's elevation API** is used if Open-Elevation is unavailable. Elevation samples are taken at uniform intervals along the great-circle path — approximately one sample per 100–200 meters depending on path length.

### Earth Curvature and Effective Earth Radius

A flat-earth approximation is not used. Each terrain sample is corrected for earth curvature using the standard effective earth radius model. The effective earth radius factor k is set to **4/3 (1.333)**, which is the widely accepted standard for average atmospheric refraction conditions in temperate climates at VHF and UHF frequencies. This k-factor accounts for the slight bending of radio waves downward through the atmosphere, effectively increasing the radio horizon beyond the optical horizon.

The curvature correction applied to each terrain sample at distance d from Site A along a total path of length D is:

```
h_correction = (d × (D − d)) / (2 × k × R_e)
```

where R_e is the mean radius of the Earth (6,371 km). This correction is added to the raw terrain elevation before comparing against the line-of-sight ray between the two antenna heights.

### Fresnel Zone Clearance

Line-of-sight in the geometric sense (unobstructed ray between antennas) is necessary but not sufficient for reliable VHF/UHF communication. The first Fresnel zone — an ellipsoid of revolution around the direct path — must be at least 60% clear of terrain obstructions to avoid significant diffraction loss.

The radius of the first Fresnel zone at any point along the path is:

```
F1 = √(λ × d1 × d2 / (d1 + d2))
```

where λ is the wavelength in meters, d1 is the distance from Site A to the point, and d2 is the distance from that point to Site B. The tool evaluates this at every terrain sample and reports the worst-case clearance as a percentage of F1.

### Knife-Edge Diffraction Loss

When terrain obstructs the path, the tool estimates the additional signal loss using the **knife-edge diffraction model** per ITU-R Recommendation P.526. This model treats the obstruction as a perfectly absorbing half-plane and calculates the diffraction loss as a function of the Fresnel-Kirchhoff diffraction parameter ν (nu):

```
ν = h × √(2(d1 + d2) / (λ × d1 × d2))
```

where h is the height of the obstruction above the line of sight (negative if the path clears the obstacle). The diffraction loss J(ν) is then computed using the standard ITU-R P.526 approximation. This provides a conservative estimate — real terrain is rarely a perfect knife-edge, and the actual loss may be lower. For multiple terrain obstructions, the Deygout method is applied, which identifies the dominant obstacle and adds secondary corrections for each additional ridge.

### Link Quality Rating

The qualitative link quality indicator (Excellent / Good / Marginal / Obstructed) is derived from a combination of Fresnel zone clearance percentage and estimated diffraction loss. A path with full Fresnel zone clearance and no diffraction loss is rated Excellent. Paths with greater than 60% Fresnel clearance but minor obstructions are Good. Significant Fresnel intrusion with measurable diffraction loss is Marginal. Paths with severe obstruction and high predicted loss are Obstructed.

### Mapping

Both the VHF/UHF and HF maps use **Leaflet.js** as the mapping engine. The VHF/UHF map uses OpenStreetMap tiles for terrain context. The HF map uses **Esri World Imagery** (satellite) with a boundaries overlay, which is better suited to visualizing the geographic spread of HF contacts across state and national borders.

### Callsign Lookups

Ham radio callsign lookups are performed against **callook.info**, which provides real-time access to FCC ULS license data including operator name, license class, registered address, and Maidenhead grid square. GMRS callsigns (format WXXX###) do not have a public API equivalent and return a direct link to the FCC ULS search for manual lookup.

### POTA Integration

Parks on the Air park data is sourced from the official **POTA API** (api.pota.app), which provides park name, location, state/country, grid square, and activity status for all POTA reference numbers worldwide. The tool accepts US-format references (US-0001) as well as numeric shorthand (0001 or K-1).

### SOTA Integration

Summit on the Air data is sourced from the official **SOTA API** (api2.sota.org.uk), which provides summit name, elevation, point value, association, and GPS coordinates. Lookups are routed through the Cloudflare Worker proxy and cached at the edge for 30 days — meaning frequently looked-up summits are served from Cloudflare's global network with near-zero latency rather than hitting the SOTA servers on every request. The Did You Mean? feature fetches the full summit list for nearby regions and scores candidates by code match and geographic proximity to your Site A location.

### Repeater Data

Repeater lookups use the **RepeaterBook API**, which provides coverage for amateur and GMRS repeaters across the United States and Canada. Results include output frequency, offset, CTCSS/DCS tones, mode, and the repeater's geographic coordinates.

### Privacy and Data

No user data is stored server-side. Contact logs are stored exclusively in your browser's local storage and never leave your device. Callsign, park, and summit lookups are the only outbound requests made, and those are proxied through a Cloudflare Worker that does not log query parameters. The tool uses Google Analytics (GA4) to collect anonymous aggregate usage data (page views, session counts) to help guide future development.

---

Built by **N4MXB** · [n4mxb.com](https://n4mxb.com) · [Find me on QRZ](https://www.qrz.com/db/N4MXB)
