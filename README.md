# FindMyCharger

A smart powerbank with locating capabilities-ish ;p

## Libraries
- NRF54L15-QFAA-R7 from https://github.com/hlord2000/nordic-lib-kicad
- MAX77958EWV+T from https://app.ultralibrarian.com/details/026be800-c091-11ea-b5d0-0aebb021a1ea/Analog-Devices-Maxim-Integrated/MAX77958EWV-T?uid=124226368
- MAX77961BEFV06+ from https://app.ultralibrarian.com/details/4d4052eb-77fa-11ec-9033-0a34d6323d74/Analog-Devices-Maxim-Integrated/MAX77961BEFV06-?uid=133653286
- NCP81599MNTXG from Component Search Engine on https://www.mouser.co.id/ProductDetail/onsemi/NCP81599MNTXG?qs=GedFDFLaBXFswcxUQ6RZzA%3D%3D

## To Do
- Figure out how to do PFAIL and put three terminal fuse... maybe https://www.reddit.com/r/AskElectronics/comments/1ac7jp2/questions_about_chemical_fuses_threeterminal/?
- Connect schematics
	- PD
	- Battery management
	- nRF54 (main brain)
	- Step down (NCP81599)
- Research for best wiring
- Design PCB
- Write firmware
- Go shopping for components
- Pick desired battery capacity
- Make casing design