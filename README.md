# PSK
## Aim:
Write a simple Python program for the modulation and demodulation of PSK and QPSK.
## Tools required:
# Program:
### PSK:
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter,lfilter

fs,fc,br,T=1500,75,10,1
t=np.linspace(0,T,fs,endpoint=False)

bits=np.random.randint(0,2,br)
bd=fs//br
msg=np.repeat(bits,bd)

car=np.sin(2*np.pi*fc*t)
psk=np.sin(2*np.pi*fc*t+np.pi*msg)

b,a=butter(5,fc/(0.5*fs),'low')
demod=lfilter(b,a,psk*car)

dec=(demod[::bd]<0).astype(int)

signals=[msg,car,psk]
titles=['Message Signal','Carrier Signal','PSK Signal']
colors=['blue','green','red']

plt.figure(figsize=(12,8))

for i in range(3):
    plt.subplot(4,1,i+1)
    plt.plot(t,signals[i],color=colors[i])
    plt.title(titles[i])
    plt.grid()

plt.subplot(4,1,4)
plt.step(range(len(dec)),dec,where='mid',
         color='blue',marker='x')
plt.title('Decoded Bits')
plt.grid()

plt.tight_layout()
plt.show()
```
### Output Waveform:
<img width="1190" height="790" alt="dc5-1" src="https://github.com/user-attachments/assets/86b52e6a-d109-463d-a0e5-2d8da01ee824" />

### QPSK:
```
import numpy as np
import matplotlib.pyplot as plt

x=['10','11','11','10']
t=np.arange(-np.pi,np.pi,0.1)

waves={
'00':np.sin(t+np.pi/4)*2,
'01':np.sin(t+3*np.pi/4)*2,
'10':np.sin(t+5*np.pi/4)*2,
'11':np.sin(t+7*np.pi/4)*2
}

mod=np.concatenate([waves[i] for i in x])
inp=np.array([int(j) for i in x for j in i])

demod=[]
for i in range(len(x)):
    v=mod[i*len(t)+2]
    if v<=-0.77: demod+=[0,0]
    elif v<=-0.63: demod+=[0,1]
    elif v>=0.77: demod+=[1,0]
    else: demod+=[1,1]

plt.figure(figsize=(10,6))

plt.subplot(3,1,1)
plt.plot(np.repeat(range(len(inp)),2),np.repeat(inp,2),
         drawstyle='steps-post',color='blue')
plt.title('Input Binary Data')
plt.ylim(-0.5,1.5)
plt.grid()

plt.subplot(3,1,2)
plt.plot(mod,color='red')
plt.title('QPSK Modulated Signal')
plt.grid()

plt.subplot(3,1,3)
plt.plot(np.repeat(range(len(demod)),2),np.repeat(demod,2),
         drawstyle='steps-post',color='green')
plt.title('Demodulated Signal')
plt.ylim(-0.5,1.5)
plt.grid()

plt.tight_layout()
plt.show()
```

### OUTPUT:
<img width="989" height="590" alt="dc5-2" src="https://github.com/user-attachments/assets/c3f98a81-457f-497f-8584-ee619970eeb3" />


# Results
Thus PSK and QPSK were performed and the waveform is verified using Google Colab

