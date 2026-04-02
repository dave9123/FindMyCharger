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

Goals
- Able to stay alive for at least 3 months idle (while transmitting)

To do
- Resources
	- PCB design guidelines for reduced EMI https://www.ti.com/lit/an/szza009/szza009.pdf
- Design PCB
	- Confirm LED strength to prevent it from being too bright	
	- Include power rating to components
	- Add more test pads
	- Setup firmware update pin out for TPS25762-Q1 (TVSP)
	- Include pad comms for ICs
	- Isolate GND for BQ4050
	- Calculate voltage divider on TPS25762-Q1 thermistor
	- Figure out either to connect ICs comms nRF54 or debug pad
	- Figure out PD in's ADCIN
	- Assign footprints
		- Capacitors
		- Resistors
		- Inductors
		- Others
	- Double check schematics
	- Double check footprints
	- Figure out impedance, frequency and allat
	- Route component traces
	- Double check PCB
- Casing
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
* Handle communications without external IC (https://github.com/Infineon/pdstack https://github.com/MicrochipTech/usb-pd-software-framework https://github.com/pdsink/pdsink PD DRP handlers)
* Ultra Wide Band (UWB) for precise finding
* Support PD2.0 / PD3.1 / PPS, QC2/3/4/5, FCP / SCP / SFCP, AFC, MTK PE, Apple / BC1.2, UFCS (new universal Chinese standard)
* Opt for WLCSP (ultra small) version

![[Pasted image 20260324112012.png]]

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