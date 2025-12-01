Powerbank with Tracker
* BLE (for https://github.com/dchristl/macless-haystack and https://github.com/leonboe1/GoogleFindMyTools)
* Power Delivery and Quick Charge (for high speed charging): MAX77958EWV+T (EWV is lead free, T is sealed (?))
* Built-in USB C cable for output
* Battery indicator (LED or screen)
* 3x 18650 Li-on batteries
* nRF54L15 SoC -- overkill, could pick lower memory version from https://www.rutronik.com/electronic-components/nordic-semiconductor/nordic-nrf54-series or even the very small version (WLCSP)
* Battery Management System (BMS): Maxim MAX17320G20+T (replace with H20 for smaller version)
* Light (in weight)
* Thermistor (stop on overheat) 10k/100k (supported as stated on https://www.analog.com/media/en/technical-documentation/data-sheets/max17320.pdf page 79 on "THType" as long as it is specified)
* BOM https://docs.google.com/spreadsheets/d/1hIzFk7VpzFUyE0Nlcrc1xWnVTtqv2BH0UQ99vOO1Ge8/view
* Maybe opt for WLCSP (ultra small) version of components for production (?) going to be harder to hand solder for sure
* Breakout programming pins to flash and debug
* Wouldn't opt for 20V because of the inefficiency, set it to MAX77958's non-volatile memory (PDO list) to make it persistent

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
- https://componentsearchengine.com/

Places to source things
- Mouser Indonesia, DigiKey Indonesia, RS Components, Future Electronics, Arrow Electronics, TTI Asia, Chipdistribution Indonesia, Tokopedia