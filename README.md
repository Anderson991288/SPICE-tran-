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

