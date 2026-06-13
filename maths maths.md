## calc tps25762-q1

### inductor
```
vin -> 12-16.8V
vout -> 5-21V
iout -> 3.25A
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
### mosfet