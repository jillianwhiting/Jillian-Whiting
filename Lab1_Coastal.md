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
a_1 = max(wave_1_upstream) - min(wave_1_upstream)
print(a_1)
a_2 = max(wave_2_upstream) - min(wave_2_upstream)
print(a_2)
a_3 = max(wave_3_upstream) - min(wave_3_upstream)
print(a_3)
a_4 = max(wave_4_upstream) - min(wave_4_upstream)
print(a_4)
a_5 = max(wave_5_upstream) - min(wave_5_upstream)
print(a_5)

```
| Stroke (mm) | Frequency (Hz) | Amplitude |
|:----------- |:-------------- | --------- |
| 3           | 2              | 10.89     |
| 3           | 2.5            | 11.18     |
| 3           | 3              | 11.44     |
| 4           | 2              | 14.98     |
| 5           | 2              | 18.13     |

```python
# graphing stuff to do
```


##Discussion of experiments

##Conclusion
