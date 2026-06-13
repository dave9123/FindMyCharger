Powerbank with Tracker
* BLE (for https://github.com/dchristl/macless-haystack and https://github.com/leonboe1/GoogleFindMyTools)
* Built-in USB C cable for output
* Battery indicator (LED or screen)
* 3x 18650 Li-on batteries
* nRF54L15 SoC -- overkill, could pick lower memory version from https://www.rutronik.com/electronic-components/nordic-semiconductor/nordic-nrf54-series or even the very small version (WLCSP)
* BOM https://docs.google.com/spreadsheets/d/1hIzFk7VpzFUyE0Nlcrc1xWnVTtqv2BH0UQ99vOO1Ge8/view
* Breakout programming pins to flash and debug
* Make certain traces thicker
* If I'm going for a small approach, might as well do 1.2 MHz, although I believe that more battery would be worth it but having the tracker approach, make it as small as possible, maybe?
* Include battery level transmission onto Macless Haystack
* https://github.com/smaeul/fhn-beacon-tools https://github.com/smaeul/fhn-beacon-tools https://arxiv.org/pdf/2210.14702
* buck boost switching https://www.ti.com/lit/an/slva535b/slva535b.pdf?ts=1781330527146

Goals
- Able to stay alive for at least 3 months idle (while transmitting)
- Moisture detection for USB C receptacles https://www.infineon.com/assets/row/public/documents/24/42/infineon-liquid-corrosion-mitigation-on-usb-type-c-connector-using-ez-pd-pmg1-devices-applicationnotes-en.pdf

Thermal foldback default config TPS25762-Q1 *R1 10k thermistor 3950 NTC - R2 3.3k

| Phase | Enter Threshold (V) | NTC Enter (Ohm) | Temp Enter (°C) | Exit Threshold (V) | NTC Exit (Ohm) | Temp Exit (°C) | Max Power (%) |
| :---: | :-----------------: | :-------------: | :-------------: | :----------------: | :------------: | :------------: | :-----------: |
|   1   |       2.044V        |     2.027k      |     65.84°C     |       1.988V       |     2.177k     |    63.77°C     |      60%      |
|   2   |        2.1V         |     1.885k      |     67.96°C     |       2.044V       |     2.027k     |    65.84°C     |      30%      |
|   3   |       2.142V        |     1.784k      |     69.59°C     |       2.086V       |     1.920k     |    67.42°C     |      0%       |

To do
- Resources
	- PCB design guidelines for reduced EMI https://www.ti.com/lit/an/szza009/szza009.pdf
	- Prevent ESD https://www.ti.com/lit/an/slva680a/slva680a.pdf?ts=1776544226092&ref_url=https%253A%252F%252Fwww.google.com%252F https://electronics.stackexchange.com/questions/233600/differences-between-tvs-diode-and-zener-diodes-in-diagrams-and-in-practice, maybe tvs beside zener?
	- layout guidelines 
	- https://www.ti.com/document-viewer/BQ25720/datasheet#GUID-713033B3-1727-4037-B279-6185E4201636/TITLE-SLUSDU3X7800
	- https://www.ti.com/lit/an/slva232/slva232.pdf?ts=1781099885492&ref_url=https%253A%252F%252Fwww.google.com%252F spark gap
	- inductor calculation http://pigeonsnest.co.uk/stuff/core-saturation.html
	- inductor calculator spreadsheet https://www.eevblog.com/forum/projects/softwarespreadsheet-for-transformer-calculation/msg5080441/#msg5080441
	- bq25713 evm https://www.ti.com/lit/ug/sluubt8b/sluubt8b.pdf?ts=1781161466690
	- bq25713 design checklist https://e2e.ti.com/cfs-file/__key/communityserver-discussions-components-files/196/0552.BQ2570X_5F00_BQ2571X_5F00_BQ2572X_5F00_BQ2573X_5F00_SchematicChecklist.pdf
- Design PCB
	- Add spark gap PACK+ PACK-
	- add polyfuse
		- calculate ripple current inductor
	- calculate ripple current for capacitors
	- verify 0402 resistors aren't extended jlcpcb pcba
	- add thermistor for thermal foldback tps25762-q1 https://www.tokopedia.com/rajacell/sensor-suhu-ntc-10k-thermistor-temperature-sensor-b3950-probe-5x25mm-1m
	- verify items exist in jlcpcb
	- replace extended with non ones
	- Include power rating to components
		- BQ4050
		- TPS25730
		- pinout debug
	- Assign footprints
	- Double check schematics
	- Double check footprints
	- Route component traces
	- Double check PCB
- Casing
	- through hole NTC beside battery d:5mm p:25mm
- Firmware
	- Macless Haystack port
	- Google Find Hub port

TPS25762-Q1 EN/UVLO setup
(12.6-12)/(16.8-12) = 12.5% left before shutdown

LED brightness
VBAT = VLEDCNTLx + 2.5 V
If battery 16.8, LEDCNTLx = 16.8-2.5 = 14.3V


agnd and pgnd connected together even tho datasheet says to isolate them (?)

nrf54 does internal pull up https://devzone.nordicsemi.com/f/nordic-q-a/31152/nrf52840-swdio-internal-pullup-and-swdclk-internal-pulldown-values

Future considerations
* Update thermistor to be able to be used by both BQ4050 and TPS25762-Q1
* Handle communications without external IC (https://github.com/Infineon/pdstack https://github.com/MicrochipTech/usb-pd-software-framework https://github.com/pdsink/pdsink PD DRP handlers)
* Ultra Wide Band (UWB) for precise finding
* Support PD2.0 / PD3.1 / PPS, QC2/3/4/5, FCP / SCP / SFCP, AFC, MTK PE, Apple / BC1.2, UFCS (new universal Chinese standard)
* Opt for WLCSP (ultra small) version
* instead of direct BMS LED, do it from MCU for more customizable things (especially how the BMS has 2 pins for control (shutdown and disp) which can be turned into short press and long press

![[Pasted image 20260324112012.png]]

ceramic capacitor weird by volt stuff https://www.analog.com/en/resources/technical-articles/temperature-and-voltage-variation-ceramic-capacitor.html
>As a result of this lesson, I no longer just specify an X7R or X5R capacitor to colleagues or customers. Instead, I specify specific parts from specific vendors whose data I have checked. I also warn customers to check data when considering alternative vendors in production to ensure that they do not run into these problems.
>
>The larger lesson here, as you may have surmised, is "read the data sheet," every time, no exceptions. Ask for detailed data when the data sheet does not contain sufficient information. Remember too that the ceramic capacitor type designations, such as X7R, X5R, and Y5V, imply nothing about voltage coefficients. Engineers must check the data to know, really know, how a specific capacitor will perform under voltage.
>
>Finally, keep in mind that, as we continue to drive madly to smaller and smaller sizes, this is becoming more of an issue every day.

human esd https://www.egr.msu.edu/classes/ece480/capstone/fall08/group03/appnotes/zener_shunt_regulator.html available up to 5W only
> The best method of routing from the ESD Source to the TVS is using straight paths which are as short as possible. Beyond lowering the impedance in the path to ground for IESD, shortening the length of this path also reduces the EMI being radiated inside the system. If corners are necessary, they should be curved with the largest radii possible, with 45° corners being the maximum angle if the PCB technology does not allow curved traces.
https://www.ti.com/lit/an/slva680a/slva680a.pdf?ts=1776544226092&ref_url=https%253A%252F%252Fwww.google.com%252F (add curves to traces)

short to vbus figure 8-3
![[Pasted image 20260419185610.png]]
https://www.ti.com/lit/an/slvaf82b/slvaf82b.pdf?ts=1776591067117

> zener diode breakdown voltage when reverse-biased would connect it to gnd, but slow response times
https://www.flywing-tech.com/blog/overvoltage-protection-using-zener-vs-tvs-diode/

![[Pasted image 20260529212459.png]]
![[firefox_of7rFwYYZ2.jpg]]
![[Pasted image 20260530112122.png]]
weird values for easier future changes instead of directly shoving it to GND and 3V3 and 1V5

![[Pasted image 20260530115305.png]]
charger voltage through battery detection

normal sleep shutdown mode![[Pasted image 20260530214131.png]]

![[Pasted image 20260531142250.png]]
die at ~12.5%

charge pump capacitor uses mosfets to charge for use in usage peak

SW's SNB cap and BOOT's BOOT cap different (50V and 10V) while being side-by-side
![[Pasted image 20260601162110.png]]
![[Pasted image 20260601162113.png]]
![[Pasted image 20260601162115.png]]

![[Pasted image 20260609111633.png]] series diode to prevent return current on battery

![[Pasted image 20260609184912.png]]
components degrade -> no inf lifespan https://www.google.com/url?sa=t&source=web&rct=j&url=https%3A%2F%2Fwww.dyc-electronic.com%2Fx5r-vs-x7r-comprehensive-comparison-engineering-design-guide%2F&ved=0CBkQjhxqFwoTCICFjrWL-pQDFQAAAAAdAAAAABAJ&opi=89978449

![[Pasted image 20260609193347.png]]tantalum polrarity

battery charger bq25713 n-mos sot-23 30V![[Pasted image 20260609194908.png]] 
![[Pasted image 20260610152507.png]]class 1 ceramic cap

bq* schematic checklist https://e2e.ti.com/cfs-file/__key/communityserver-discussions-components-files/196/0552.BQ2570X_5F00_BQ2571X_5F00_BQ2572X_5F00_BQ2573X_5F00_SchematicChecklist.pdf

altogether skip the insane power rating resistor and directly connect bq25713
![[Pasted image 20260610192307.png]]![[Pasted image 20260610192309.png]]

different setup bq4050
![[Pasted image 20260610193129.png]]

how did I miss current sense DNP
![[Pasted image 20260610220321.png]]![[Pasted image 20260610220322.png]]

![[Pasted image 20260612194439.png]]ref design and evm diff fets

what![[Pasted image 20260612200559.png]]

![[Pasted image 20260613114032.png]]

Battery (18650)
* 3000 mAh 3.7V @ Rp 14.250 (sells at 2) https://www.tokopedia.com/nayfastore/baterai-cas-ulang-charger-li-ion-18650-3000mah-3-7v-ungu-isi-2-pcs-1731453774692058659 
* 1200 mAh @ Rp 5.738 https://www.tokopedia.com/lbagstore/baterai-recharge-18650-3-7v-flat-top-button-datar-senter-kipas-lampu
* !! (BAD, reported fail on 2nd charge) 6800-8000 mAh @ Rp 16.500 https://www.tokopedia.com/gold-mart/baterai-18650-8000-6800-mah-3-7v-mitsuyama-battery-li-ion-rechargeable-1730909238634514094
* !! (REPORTED 800 mAh) 3000 mAh 3.7V @ Rp 8.800 https://www.tokopedia.com/harry-howard/baterai-18650-3000mah-3-7v-grade-a-rechargeable-lithium-li-ion-ori-datar-cac90
* 2600 mAh @ Rp 23.300 https://www.tokopedia.com/ebikejakartacom/li-ion-18650-dmegc-inr18650-26e-2600mah-discharg-5c-15a-3-7v-flat-head
* 2900 mAh @ Rp 32.050 https://www.tokopedia.com/ebikejakartacom/li-ion-18650-dmegc-inr18650-29-2900mah-discharg-3c-10a-3-7v-flat-head

Great place to find symbols and footprints
- https://www.ultralibrarian.com/
- https://www.snapeda.com/
- https://componentsearchengine.com/ through Mouser
- https://github.com/uPesy/easyeda2kicad.py

Places to source things
- Mouser Indonesia, DigiKey Indonesia, RS Components, Future Electronics, Arrow Electronics, TTI Asia, Chipdistribution Indonesia, Tokopedia