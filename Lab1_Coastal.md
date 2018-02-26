```python
from aide_design.play import*
```
####Jillian Whiting
####jpw236

##CEE4530 Lab 1

##Introduction:
This purpose of this lab was to generate waves in the wave maker and use the wave gauge to measure the distance from the gauge to the wave.

##Experimental Setup:

##Calibration
The gauge reading was the average reading over the 10-20 seconds logging period.
| Distance  (ft) | Volts |
|:-------------- |:----- |
| 0.4            | 4.11  |
| 0.3            | 2.72  |
| 0.2            | 1.69  |
| 0.1            | 0.60  |


```python
import scipy.stats
Gauge_height = np.array([.4,.3,.2,.1])*u.ft
Gauge_height = Gauge_height.to(u.mm)/u.mm
Gauge_data = np.array([4.11,2.72,1.69,0.60])
trend = scipy.stats.linregress(Gauge_data,Gauge_height)
plt.plot(Gauge_data,Gauge_height,'*')
plt.plot(Gauge_data, Gauge_data * trend[0] + trend[1])
plt.title('Wave Gauge Calibration Results')
plt.xlabel('Distance(mm)')
plt.ylabel('Gauge Reading (Volts)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/gauge_calibration')
plt.show()
```
![calibration](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/gauge_calibration.png?raw=true)

The calibration coefficient is 26.24 mm/V.This is very close to the calibration coefficients of the other wave gauges, so it is a reasonable answer.

##Wave Generation Results
```python
file = pd.read_csv('Monday2-1.csv')
wave_1_upstream = file["upstream"].values  * 22.4
wave_1_time = file["Time"].values
wave_1_time = wave_1_time - wave_1_time[0]
plt.plot(wave_1_time,wave_1_upstream)
plt.title('Wave Gauge Stroke = 3mm Frequency = 2 Hz')
plt.xlabel('Time(s)')
plt.ylabel('Eta (mm)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/wave1_2')
plt.show()
file = pd.read_csv('Monday2-2.csv')
wave_2_upstream = file["upstream"].values * 22.4
wave_2_time = file["Time"].values
wave_2_time = wave_2_time - wave_2_time[0]
plt.plot(wave_2_time,wave_2_upstream)
plt.title('Wave Gauge Stroke = 3 mm Frequency = 2.5 Hz')
plt.xlabel('Time(s)')
plt.ylabel('Eta (mm)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/wave2')
plt.show()
file = pd.read_csv('Monday2-3.csv')
wave_3_upstream = file["upstream"].values * 22.4
wave_3_time = file["Time"].values
wave_3_time = wave_3_time - wave_3_time[0]
plt.plot(wave_3_time,wave_3_upstream)
plt.title('Wave Gauge Stroke = 3 mm Frequency = 3 Hz')
plt.xlabel('Time(s)')
plt.ylabel('Eta (mm)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/wave3')
plt.show()
file = pd.read_csv('Monday2-4.csv')
wave_4_upstream = file["upstream"].values * 22.4
wave_4_time = file["Time"].values
wave_4_time = wave_4_time - wave_4_time[0]
plt.plot(wave_4_time,wave_4_upstream)
plt.title('Wave Gauge Stroke = 4 mm Frequency = 2 Hz')
plt.xlabel('Time(s)')
plt.ylabel('Eta (mm)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/wave4')
plt.show()
file = pd.read_csv('Monday2-5.csv')
wave_5_upstream = file["upstream"].values * 22.4
wave_5_time = file["Time"].values
wave_5_time = wave_5_time - wave_5_time[0]
plt.plot(wave_5_time,wave_5_upstream)
plt.title('Wave Gauge Stroke = 5 mm Frequency = 2 Hz')
plt.xlabel('Time(s)')
plt.ylabel('Eta (mm)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/wave5')
plt.show()
```
![wave1](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/wave1_2.png?raw=true)
![wave2](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/wave2.png?raw=true)
![wave3](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/wave3.png?raw=true)
![wave4](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/wave4.png?raw=true)
![wave5](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/wave5.png?raw=true)

Based on the graphed wave data, there is not much difference between the local minimum and maxima and the overall minimum and maximum. The error there is a fluctuation of about .5 mm in a 10 mm amplitude which is only a 5% error. Therefore using the difference between the overall minimum and maximum will be sufficient for our analysis of amplitude.
```python
from scipy.optimize import curve_fit
def func(time,a,k,q,w,c):
    return a * np.sin(k * q - w * time) + c
guesses = [5,-1,0,15,0]
wave_1_fit = np.zeros([56,5])
for k in range(56):
  popt, pcov = scipy.optimize.curve_fit(func, wave_1_time[k*50:k*50 + 50], wave_1_upstream[k*50:k*50 + 50],guesses)
  wave_1_fit[k,:] = popt
print(wave_1_fit)
average = np.zeros([5])
average[0] = np.average(abs(wave_1_fit[:,0]))
average[1] = np.average(wave_1_fit[:,1])
average[2] = np.average(wave_1_fit[:,2])
average[3] = np.average(wave_1_fit[:,3])
average[4] = np.average(wave_1_fit[:,4])
plt.plot(wave_1_time, func(wave_1_time, *average))
plt.plot(wave_1_time, wave_1_upstream)
plt.show()
print(average[0])

wave_2_fit = np.zeros([56,5])
guesses = [5.25,-1,0,15,0]
for k in range(56):
  popt, pcov = scipy.optimize.curve_fit(func, wave_2_time[k*50:k*50 + 50], wave_2_upstream[k*50:k*50 + 50],guesses)
  wave_2_fit[k,:] = popt
average_2 = np.zeros([5])
average_2 [0] = np.average(abs(wave_2_fit[:,0]))
average_2[1] = np.average(wave_2_fit[:,1])
average_2[2] = np.average(wave_2_fit[:,2])
average_2[3] = np.average(wave_2_fit[:,3])
average_2[4] = np.average(wave_2_fit[:,4])
plt.plot(wave_2_time, func(wave_2_time, *average_2))
plt.plot(wave_2_time, wave_2_upstream)
plt.show()
print(average_2[0])

wave_3_fit = np.zeros([56,5])
guesses = [5.25,-1,0,15,0]
for k in range(56):
  popt, pcov = scipy.optimize.curve_fit(func, wave_3_time[k*50:k*50 + 50], wave_3_upstream[k*50:k*50 + 50],guesses)
  wave_3_fit[k,:] = popt
average_3 = np.zeros([5])
average_3 [0] = np.average(abs(wave_3_fit[:,0]))
average_3[1] = np.average(wave_3_fit[:,1])
average_3[2] = np.average(wave_3_fit[:,2])
average_3[3] = np.average(wave_3_fit[:,3])
average_3[4] = np.average(wave_3_fit[:,4])
plt.plot(wave_3_time, func(wave_3_time, *average_3))
plt.plot(wave_3_time, wave_3_upstream)
plt.show()
print(average_3[0])

wave_4_fit = np.zeros([56,5])
guesses = [5.25,-1,0,15,0]
for k in range(56):
  popt, pcov = scipy.optimize.curve_fit(func, wave_4_time[k*50:k*50 + 50], wave_4_upstream[k*50:k*50 + 50],guesses)
  wave_4_fit[k,:] = popt
average_4 = np.zeros([5])
average_4 [0] = np.average(abs(wave_4_fit[:,0]))
average_4[1] = np.average(wave_4_fit[:,1])
average_4[2] = np.average(wave_4_fit[:,2])
average_4[3] = np.average(wave_4_fit[:,3])
average_4[4] = np.average(wave_4_fit[:,4])
plt.plot(wave_4_time, func(wave_4_time, *average_4))
plt.plot(wave_4_time, wave_4_upstream)
plt.show()
print(average_4[0])

wave_5_fit = np.zeros([56,5])
guesses = [5.25,-1,0,15,0]
for k in range(56):
  popt, pcov = scipy.optimize.curve_fit(func, wave_5_time[k*50:k*50 + 50], wave_5_upstream[k*50:k*50 + 50],guesses)
  wave_5_fit[k,:] = popt
average_5 = np.zeros([5])
average_5 [0] = np.average(abs(wave_5_fit[:,0]))
average_5[1] = np.average(wave_5_fit[:,1])
average_5[2] = np.average(wave_5_fit[:,2])
average_5[3] = np.average(wave_5_fit[:,3])
average_5[4] = np.average(wave_5_fit[:,4])
plt.plot(wave_5_time, func(wave_5_time, *average_5))
plt.plot(wave_5_time, wave_5_upstream)
plt.show()
print(average_5[0])
```
| Stroke (mm) | Frequency (Hz) | Amplitude (mm) |
|:----------- |:-------------- | --------- |
| 3           | 2              | 5.3957    |
| 3           | 2.5            | 5.3404    |
| 3           | 3              | 5.0493    |
| 4           | 2              | 7.1341    |
| 5           | 2              | 8.5370    |


```python
strokes = np.array([3,3,3,4,5])
frequency = np.array([2,2.5,3,2,2])
amplitude = np.array([average[0],average_2[0],average_3[0],average_4[0],average_5[0]])
array_2 = np.array([amplitude[0],amplitude[3],amplitude[4]])
## stroke 3 graph
plt.plot(frequency[0:3],amplitude[0:3])
plt.show()
## frequency 2 graph
plt.plot(strokes[2:5],array_2)
plt.show()
```


##Discussion of experiments

##Conclusion
