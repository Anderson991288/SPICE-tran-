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

# 訊號傳輸

## NMOS CMOS

### NMOS switch:
![Screenshot from 2022-07-07 21-27-12](https://user-images.githubusercontent.com/68816726/177785374-b1e73c87-4276-4362-b483-c72ba3c64857.png)
netlist:
```
Signal Transmission through a NMOS Switch
* circuit description
Vg 2 0 DC 5V
Vb 3 0 DC -5V
Vi 1 0 SIN(0V 5V 100Hz)
Rl 6 0 100k
* MOSFET model description
M1 1 2 6 3 e_nmosfet L=10u W=400u
.model e_nmosfet nmos (KP=20u Vto=2V lambda=0.02 gamma=0.5)
.end
```
ngspice:
```
ngspice TransmissionNMOS.cir

tran 0.01ms 20ms 0ms 0.01ms
plot v(1) v(6)
```

![Screenshot from 2022-07-07 21-28-39](https://user-images.githubusercontent.com/68816726/177785776-2de4b612-6726-4820-8ce5-b6fcf1b82bba.png)

*紅線為輸入波形,藍線為輸出波形*

### CMOS switch:

![Screenshot from 2022-07-07 21-31-34](https://user-images.githubusercontent.com/68816726/177785964-9a6d4867-730d-40ac-b996-91f4199bd285.png)

netlist:
```
On-Resistance of an CMOS Switch
* circuit description
Vgn 2 0 DC 5V
Vbn 3 0 DC -5V
Vgp 5 0 DC -5V
Vbp 4 0 DC 5V
Vi 1 0 SIN(0V 5V 100Hz)
Rl 6 0 1k
* MOSFET model description
M1 1 2 6 3 e_nmosfet L=10u W=400u
M2 1 5 6 4 e_pmosfet L=10u W=400u
.model e_nmosfet nmos (KP=20u Vto=2V lambda=0.02 gamma=0.5)
.model e_pmosfet pmos (KP=20u Vto=-2V lambda=0.02 gamma=0.5)
.end
```
ngspice:
```
ngspice TransmissionCMOS.cir

tran 0.1ms 20ms 0ms 0.1ms
plot v(6)
```

![Screenshot from 2022-07-07 22-01-31](https://user-images.githubusercontent.com/68816726/177792700-131d4ba4-b943-4b46-bd99-807fbe95a274.png)

*可見CMOS輸出波形與輸入正弦波形幾乎一致，代表訊號傳輸幾乎無失真*

# 步階響應
  將 MOS 開關之接收端連接負載電阻和電容，為觀察CMOS開關步階響應輸入步階波形
## CMOS:
![Screenshot from 2022-07-07 22-26-26](https://user-images.githubusercontent.com/68816726/177797891-4a204560-86df-4d7f-aa84-ab11ff25b5bd.png)

netlist:
```
Step Response of a CMOS Switch
* circuit description (RL=100k)
Vgn 2 0 DC 5V
Vbn 3 0 DC -5V
Vgp 5 0 DC -5V
Vbp 4 0 DC 5V
Vi 1 0 PWL (0 0V 10ns 0V 20ns 5V 50ns 5V)
Rl 6 0 100k
Cl 6 0 1pF
* MOSFET model description
M1 1 2 6 3 e_nmosfet L=10u W=400u
M2 1 5 6 4 e_pmosfet L=10u W=400u
.model e_nmosfet nmos (KP=20u Vto=2V lambda=0.02 gamma=0.5)
.model e_pmosfet pmos (KP=20u Vto=-2V lambda=0.02 gamma=0.5)
.end
```
ngspice:
```
ngspice StepResponseCMOSSW.cir

tran 0.01ns 50ns 0ms 0.01ns
plot v(6)+v(1)
```

![Screenshot from 2022-07-07 22-25-56](https://user-images.githubusercontent.com/68816726/177798194-34ce468b-1aee-4ffa-a138-b9467a003566.png)

# 米勒積分器
![Screenshot from 2022-07-08 22-29-23](https://user-images.githubusercontent.com/68816726/178012485-842fa508-e079-4bf4-aad3-9465608149e1.png)

netlist:
```
The Miller Integrator
* circuit description *
Vi 1 0 PWL (0 0V 1ms 0V 1.001ms 1V 10ms 1V)
R1 1 2 1k
C1 2 3 20uF
Eopamp 3 0 0 2 1e7
```

ngspice:
```
ngspice MillerIntegrator.cir

tran 10us 5ms 0ms 10us
plot v(1)
plot v(3)
```
### input:
![Screenshot from 2022-07-08 22-28-36](https://user-images.githubusercontent.com/68816726/178012814-f8176b43-8b31-4950-aa6c-7f97b3d85dfe.png)

### output:
![Screenshot from 2022-07-08 22-28-44](https://user-images.githubusercontent.com/68816726/178012930-19fc0fee-3963-463c-bc9f-0d403d7eecba.png)



