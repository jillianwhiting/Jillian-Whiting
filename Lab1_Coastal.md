```python
from aide_design.play import*
```
####Jillian Whiting
####jpw236

##CEE4530 Lab 1

##Introduction:
This purpose of this lab was to generate waves in the wave maker and use the wave gauge to measure the distance from the gauge to the wave. Another purpose was to apply what we have learned about linear wave theory and see how it applies to waves in the laboratory. Another learning outcome from this lab was how to analyze wave data especially curve fitting to a sine/cosine wave and connecting data to derived equations.

##Experimental Setup:
There were two experimental set ups. The first was a wave gauge set up over a column of water. The height of the wave gauge above the water column was varied and data from the gauge was recorded using Easy Data. This was used for the calibration coefficient step.
The second experimental setup was in a wave flume. There was a wave make that generated waves within the flume. There were two wave gauges along the flume that measured wave height at different points in time. The wave maker was set to make waves with different strokes and frequencies. The data from the wave gauges was recorded for all the waves and this data was analyzed for fit to a wave equation.

##Calibration
The calibration coefficient was determined by averaging the gauge reading over the 10-20 seconds that data was logged and then using a linear regression to fit the line. The slope of that line is the calibration coefficient.

| Distance above water column (ft) | Volts |
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

From the wave gauge data we can create curve fits based on linear wave theory. We will use a curve fit function to fit the data to the following equation:
$\eta(x,t) = a\sin(kx-\sigma t)$.
There is a lot of waves in the data from the recording so in order to minimize noise in the curve fitting function. the curve fit was done in sections of 50 points. Based on visual inspection this was the number of points where the amplitude of the wave was most similar to curve fitting amplitude. The amplitude of each of the 50 point curve fits was then averaged to generate the overall amplitude of the wave.  
```python
from scipy.optimize import curve_fit
def func(time,a,k,q,w,c):
    return a * np.sin(k * q - w * time) + c
file = pd.read_csv('Monday2-1.csv')
wave_1_upstream = file["upstream"].values  * 22.4
wave_1_time = file["Time"].values
wave_1_time = wave_1_time - wave_1_time[0]  
guesses = [5,-1,0,15,0]
wave_1_fit = np.zeros([56,5])
for k in range(56):
  popt, pcov = scipy.optimize.curve_fit(func, wave_1_time[k*50:k*50 + 50], wave_1_upstream[k*50:k*50 + 50],guesses)
  wave_1_fit[k,:] = popt
average = np.zeros([5])
average[0] = np.average(abs(wave_1_fit[:,0]))
average[1] = np.average(wave_1_fit[:,1])
average[2] = np.average(wave_1_fit[:,2])
average[3] = np.average(wave_1_fit[:,3])
average[4] = np.average(wave_1_fit[:,4])
plt.plot(wave_1_time, func(wave_1_time, *average))
plt.plot(wave_1_time, wave_1_upstream)
plt.title('Wave Gauge Stroke = 3mm Frequency = 2 Hz')
plt.xlabel('Time(s)')
plt.ylabel('Eta (mm)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/wave1_2')
plt.legend(['Curve Fit', 'Data'],loc='best')
plt.show()

file = pd.read_csv('Monday2-2.csv')
wave_2_upstream = file["upstream"].values * 22.4
wave_2_time = file["Time"].values
wave_2_time = wave_2_time - wave_2_time[0]
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
plt.title('Wave Gauge Stroke = 3 mm Frequency = 2.5 Hz')
plt.xlabel('Time(s)')
plt.ylabel('Eta (mm)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/wave2')
plt.legend(['Curve Fit', 'Data'],loc='best')
plt.show()

file = pd.read_csv('Monday2-3.csv')
wave_3_upstream = file["upstream"].values * 22.4
wave_3_time = file["Time"].values
wave_3_time = wave_3_time - wave_3_time[0]
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
plt.title('Wave Gauge Stroke = 3 mm Frequency = 3 Hz')
plt.xlabel('Time(s)')
plt.ylabel('Eta (mm)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/wave3')
plt.legend(['Curve Fit', 'Data'],loc='best')
plt.show()


file = pd.read_csv('Monday2-4.csv')
wave_4_upstream = file["upstream"].values * 22.4
wave_4_time = file["Time"].values
wave_4_time = wave_4_time - wave_4_time[0]
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
plt.title('Wave Gauge Stroke = 4 mm Frequency = 2 Hz')
plt.xlabel('Time(s)')
plt.ylabel('Eta (mm)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/wave4')
plt.legend(['Curve Fit', 'Data'],loc='best')
plt.show()


file = pd.read_csv('Monday2-5.csv')
wave_5_upstream = file["upstream"].values * 22.4
wave_5_time = file["Time"].values
wave_5_time = wave_5_time - wave_5_time[0]
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
plt.title('Wave Gauge Stroke = 5 mm Frequency = 2 Hz')
plt.xlabel('Time(s)')
plt.ylabel('Eta (mm)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/wave5')
plt.legend(['Curve Fit', 'Data'],loc='best')
plt.show()
```
![wave1](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/wave1_2.png?raw=true)
![wave2](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/wave2.png?raw=true)
![wave3](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/wave3.png?raw=true)
![wave4](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/wave4.png?raw=true)
![wave5](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/wave5.png?raw=true)

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
trend_freq = scipy.stats.linregress(frequency[0:3],amplitude[0:3])
trend_stroke = scipy.stats.linregress(strokes[2:5],array_2)
print(trend_stroke)

## stroke 3 graph
plt.plot(frequency[0:3],amplitude[0:3],'*')
plt.plot(frequency[0:3],frequency[0:3]*trend_freq[0]+trend_freq[1])
plt.title('Wave Stroke = 3 mm')
plt.xlabel('Frequency (Hz)')
plt.ylabel('Amplitude (mm)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/stroke_3')
plt.show()
## frequency 2 graph
plt.plot(strokes[2:5],array_2,'*')
plt.plot(strokes[2:5],strokes[2:5]*trend_stroke[0]+trend_stroke[1])
plt.title('Wave Frequency = 2 Hz')
plt.xlabel('Stroke (mm)')
plt.ylabel('Amplitude (mm)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/frequency_2')
plt.show()
```

![stroke](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/stroke_3.png?raw=tru)
![freq](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/frequency_2.png?raw=tru)

##Discussion of Results
From linear wave theory we derived the equation for the free surface as:
$\eta(x,t) = a\sin(kx-\sigma t)$.
From this equation we see that a change in the frequency ($\sigma$) of the wave should only shift the phase of the wave and not affect the amplitude. The results show a small, slope -0.34 mm/Hz downward trend in amplitude with frequency. The $r^{2}$ value for this correlation is 0.86. This could be due to mechanical error that with high frequencies the wave generator does not achieve its whole stroke because it is moving very fast.
From linear wave theory, we have also derived energy equations which resulted in:
$\overline{Flux} = \overline{E^{+}}c_{p}n$
Assuming the stroke of the wavemaker is proportional to Force exerted by the wavemaker, then the flux created by the wave maker is proportional to the force times the the velocity. Velocity is proportional to amplitude. the $\overline{E^{+}}$ is proportional to $a^{2}$. Therefore:
$F*a\sim a^2$ or $Stroke\sim a$
This is supported by the data which shows a strong linear correlation, $r^2$ 0.996, between stroke and amplitude. Errors in the data for this could be mechanical errors or measurement errors by the gauge, but they are very slight.
