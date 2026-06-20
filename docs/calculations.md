*value rounded up for obvious safety reasons
## calc tps25762-q1

### output caps - 9.2.2.6

```
Icout(rms) = 3A*sqrt(21V/12V - 1) = 2.59A
Icout(rms) = 3A*sqrt(21V/16.8V - 1) = 1.5A

Icout(rms) = 3.25A*sqrt(20V/12V - 1) = 2.65A
Icout(rms) = 3.25A*sqrt(20V/16.8V - 1) = 1.42A


Vripple(Cout) = 3.25A*(1 - 12V/20V) / (144uF*10^-6 * 400kHz*10^3) ~= 0.0225V = 22.5mV (that's if MLCC don't decide to lower their capacitance)
```

### input caps - 9.2.2.7

```
D = Vout-Vin/Vout = 21-12/21 ~= 0.43
> from https://www.everythingpe.com/calculators/duty-cycle-calculator

Icin(rms) = 3.25A * sqrt(0.43*(1-0.43)) ~= 1.61A
```
### thermal foldback
default config w R1 10k thermistor 3950 NTC - R2 3.3k

|Phase|Enter Threshold (V)|NTC Enter (Ohm)|Temp Enter (°C)|Exit Threshold (V)|NTC Exit (Ohm)|Temp Exit (°C)|Max Power (%)|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|1|2.044 V|2.892 kΩ|56.88°C|1.988 V|3.101 kΩ|55.10°C|60%|
|2|2.100 V|2.686 kΩ|58.75°C|2.044 V|2.892 kΩ|56.88°C|30%|
|3|2.142 V|2.546 kΩ|60.08°C|2.086 V|2.731 kΩ|58.33°C|0%|

### inductor
```
vin -> 12-16.8V
vout -> 5-21V
iout -> 3-3.25A
fsw -> 400kHz


Dbuck = 5V/(16.8V*0.9) = 0.33
Il_buck(max) = (16.8V-5V)*0.33/(400KHz*(10^3) * 4.7uH*(10^-6)) = 2.07A
Isw_buck(max) = 2.07/2 + 3.25 = 4.29A
Imaxout(buck) = 6.2A - 2.07A/2 = 5.17A


Dboost = 1-(12*0.95)/21 = 0.46
Il_boost(max) = 12V*0.46/(400KHz*(10^3) * 4.7uH*(10^-6)) = 2.94A
Isw_boost(max) = 2.94A/2 + 3.25A/(1-0.45) = 7.38A
Imaxout(boost) = (8.2A-2.94A/2) * (1-0.46) = 3.63A
```
switching max is 7.38A, so pick higher
### LDO
external current limit 25 mA

nRF54 TX 10 mA
EEPROM 5 mA max (?, max dc output current @ absolute maximum rating)
pull up resistors, TODO: calculate power used

### i2c pull up

![[Pasted image 20260615203838.png]]

capacitive load seems to be worst case scenario, TODO: calculate capacitive load
```
-> min = (3.3V-0.4V) / (3mA*10^-3) = 966.666667

standard mode (max)
 -> max = 1000ns*10^-9 / (0.8473 * 400pF*10^-12) = 2950.54880208

fast mode (max)
 -> max 300ns*10^-9 / (0.8473 * 400pF*10^-12) = 885.16464062

fast mode plus (max)
 -> max = 120ns*10^-9 / (0.8473 * 550pF*10^-12) = 257.50244091
```
## calc bq25713
### mosfet
```
duty cycle (buck) -> D = Vout/Vin
= 16.8V/20V = **0.84**
= 12V/20V = 0.6

duty cycle (boost) -> D = 1 - Vin/Vout
= 1 - 5V/16.8V = 0.70
= 1 - 5V/12V = 0.42

Ptop = 0.84 * 3.2A^2 * Rds(on) * 1/2 * 21V * (ton + toff) * 800kHz*10^3

Pbottom = (1-0.42) * 3.2A^2 * Rds(on) = 5.939 * Rds(on)
```

### inductor - 10.2.2.2
```
D = duty cycle


Dbuck = Vout/Vin
= (12/20) = 0.6

Iripple_buck = Vin * D * (1 - D) / (fs * L)
= 20V * 0.6 * (1-0.6) / (800kHz*(10^3) * 2.2uH*(10^-6))
= 2.73A (peak buck)


Dboost = 1 - (Vin/Vbat)
= 1 - (5V/16.8V) = 1 - 0.2976.. = 0.71

Iripple_boost = (VIN * Dboost) / (fs * L)
= (5V * 0.71) / ((800kHz*(10^3) * 2.2uH*(10^-6))
  = 2.02A (peak boost)


Isat >= Ichg + 1/2*Iripple
  >= (60W/12V) + 1/2*2.73A
  >= 6.37A
```
inductor ripple range 20 – 40% as trade-off between size and efficiency

### input capacitor - 10.2.2.3
*duty cycle rounded down as worst case RMS seems to be half
> Input capacitor should have enough ripple current rating to absorb input switching ripple current. The **worst case RMS ripple current is half of the charging current** (plus system current there is any system load) when duty cycle is 0.5 in buck mode

*referencing duty cycle buck & boost values from inductor calc, 10.2.2.2

buck
Icin = 3A * sqrt(0.83*(1-0.83)) = 1.13A

boost
Icin = 3A * sqrt(0.70*(1-0.70)) = 1.38A

### power mosfet
gate drive -> 6V
input -> 19-20V -> derating +50% >= 30V

D = Vout/Vin
  = 16.8V/5V = 3.36
  = 16.8V/20V = 0.84


Qsw = Qgd + 1/2*Qgs

ton = 

toff = 

Ion = (6-)

Ioff = 

Ptop = 3.36 * (3A)^2 * Rds(on) + 1/2 * 5 * 3 * ()

MOSFET plateau voltage (Vplt) -> basically gate needs more voltage than threshold because of miller effect (when between in&out amplifier, Vout > Vin)
https://electronics.stackexchange.com/questions/392956/what-do-we-mean-by-gate-plateau-voltage
https://electronics.stackexchange.com/questions/141298/what-happpens-to-the-vgs-in-the-miller-plateau-region-during-mosfet-turn-on
ok now how am I supposed to calc Vplt, it's needed for figure-of-merit (FOM) calc

> max current non-sync -> 0.25A@10mΩ curent sensing or 0.5A if batt < 2.5V
> minimum duty cycle happens at lowest battery voltage
does this mean that less duty cycle = worst case scenario?

| Mode |   Buck    | Buck-Boost |   Boost   |
| :--: | :-------: | :--------: | :-------: |
|  Q1  | Switching | Switching  |    ON     |
|  Q2  | Switching | Switching  |    OFF    |
|  Q3  |    OFF    | Switching  | Switching |
|  Q4  |    ON     | Switching  | Switching |
### battery cells
cell_batpresz at 8.5 BQ25713 "ANALOG INPUT (CELL_BATPRESZ)" 4S 6*300000/(100000+300000) = 4.5V (75%)
    - on 10% tolerance up to lowest 6 * 270000/(110000+270000) = 4.26V (71%)
    - on 15% tolerance up to lowest 6 * 255000/(115000+255000) = 4.13V (68.8%)