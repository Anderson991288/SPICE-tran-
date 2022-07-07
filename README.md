# SPICE 交流與暫態分析

## 二極體限制器(tran)

![Screenshot from 2022-07-07 16-14-50](https://user-images.githubusercontent.com/68816726/177725636-dc2b7f53-39d5-43a1-a0cf-d7758d071d55.png)

netlist:
```
A Diode Limiting Circuit
* circuit description
Vi 1 0 SIN (0V 10V 60Hz)
VR1 4 0 DC 5V
VR2 6 0 DC -5V
R1 1 2 10k
R2 2 3 10k
R3 2 5 10k
* diode model descriotion
D1 3 4 1mA_diode
D2 6 5 1mA_diode
.model 1mA_diode D (Is=0.01pA n=1.0675)
.end

```
ngspice:
```
ngspice 'Diode Limitingtran.cir'
tran 0.01ms 40ms 0ms 0.01ms
plot v(1) v(2)
```

![Screenshot from 2022-07-07 16-21-33](https://user-images.githubusercontent.com/68816726/177727378-f7367e15-e713-4a7f-8938-cebaea5915fc.png)

# 具有濾波電容的峰值整流器電路

![Screenshot from 2022-07-07 16-29-07](https://user-images.githubusercontent.com/68816726/177728688-17b8b541-430a-46bd-9922-422644fab609.png)

netlist:
```
A Peak Rectifier

* circuit description (1mA diode)
Vi 1 0 SIN (0V 10 60Hz)
R1 2 0 1k
C1 2 0 1mF
*diode model descriotion
D1 1 2 1mA_diode
.model 1mA_diode D (Is=0.01pA n=1.0675)
.end
```

ngspice:
```
ngspice PeakRectifier.cir

tran 0.01ms 60ms 0ms 0.01ms
plot v(2) v(1)
plot v(2)
```

![Screenshot from 2022-07-07 16-28-48](https://user-images.githubusercontent.com/68816726/177729007-68d78097-58df-45f7-9b1b-26b3824067c6.png)

# BJT 放大器單級放大器

![Screenshot from 2022-07-07 16-46-15](https://user-images.githubusercontent.com/68816726/177732174-a30fc6d2-0da2-4934-896b-61ae976ce2ac.png)

netlist:
```
A Single-Stage Amplifier Circuit
* circuit description
Vcc 1 0 DC +10V
Vbb 5 0 DC +3V
Vi 4 5 SIN (0V 1 60Hz)
Rc 1 2 2k
Rbb 4 3 100k
* model description *
Q1 2 3 0 npn_transistor
.model npn_transistor npn (Is=1.8e-15 Bf=100)
.end
```
ngspice:
```
ngspice BJTSingle-StageAmplifier.cir

tran 0.01ms 60ms 0ms 0.01ms
plot v(4)-v(5) v(2)
```

![Screenshot from 2022-07-07 16-47-06](https://user-images.githubusercontent.com/68816726/177732417-01a86104-3289-4891-81a0-da0f43d46b40.png)


# CMOS 開關

![Screenshot from 2022-07-07 21-16-26](https://user-images.githubusercontent.com/68816726/177782933-1d65eeb4-c8e7-42c5-b1f4-7976a025cb41.png)

netlist:
```
On-Resistance of an CMOS Switch
* circuit description
Vgn 2 0 DC 5V
Vbn 3 0 DC -5V
Vgp 5 0 DC -5V
Vbp 4 0 DC 5V
Vi 1 0 DC 0V
* MOSFET model description
M1 1 2 0 3 e_nmosfet L=10u W=400u
M2 1 5 0 4 e_pmosfet L=10u W=400u
.model e_nmosfet nmos (KP=20u Vto=2V lambda=0.02 gamma=0.5)
.model e_pmosfet pmos (KP=20u Vto=-2V lambda=0.02 gamma=0.5)
.end
```
ngspice:
```
ngspice CMOSSwitch.cir

dc vi -5 5 10.001m
plot -v(1)/i(vi)
```

![Screenshot from 2022-07-07 21-15-58](https://user-images.githubusercontent.com/68816726/177783243-da9056ca-b806-4cc5-90ea-48bd7254c737.png)



