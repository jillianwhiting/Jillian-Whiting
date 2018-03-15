
Jillian Whiting

jpw236

CEE4530 Lab 1

# Introduction:
This lab was two parts each of which tested experimental data against theoretical models. The first part was using two wave gauges to match wave data. If the wave data was overlapping from the two gauges there were a wavelength apart(or at least an integer multiple of the wavelength).The second part of the lab was using the ADV to measure the difference in velocity at different depths beneath a wave. A thing that I think was helpful from the lab was getting a physical feeling for the theoretical relationships.
# Experimental Setup:
A brief description of the wave tank facility, wave gauges, ADV
accompanied by a diagram showing the experimental setup.
There were two parts of the experimental set-up, the first being the wave gauges. The wave gauges send out signals which bounce off the water surface and therefore can measure distance to the surface. There was one stationary wave gauge and one on a slide as shown in the schematic below:
![experimental_apparatus_1](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/experimental_apparatus_1.png?raw=true)
The wave gauge could be slid along the top of the tank until the distance between represented one wavelength because the wave gauge data lined up.

The second part of the apparatus was using the ADV.The ADV measures velocity in all directions. It uses acoustics to measure velocity. Extra seeding particles needed to be added to the tank, which is not surprising as we were the first group. The ADV unlike the wave gauges is submerged in water. During the test it was submerged at four elevations below water level, 7.5 cm, 10.5 cm, 12.5 cm, 17.5 cm.
![experimental_apparatus_2](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/experimental_apparatus_2.png?raw=true)

# Dispersion relation:

$k = \frac{2\pi}{\lambda}$
$\sigma = \frac{2\pi}{T}$

$\frac{\sigma^{2}h}{g} = \frac{kh}{tanh^{-1}(kh)}$

Stroke = 5
![amplitude 5](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/dispersion_5.png?raw=true)
Stroke = 10
![amplitude 10](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/dispersion_10.png?raw=true)


# Dispersion relation-celerity:

$c_{p} = \sqrt{\frac{tanh(kh)}{kh}} \sqrt{gh}$

$c_{p} = \frac{\sigma}{k}$

Stroke = 5
![celerity_5 5](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/celerity_5.png?raw=true)
Stroke = 10
![celerity_10](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/celerity_10.png?raw=true)

# Water particle velocity (u):
The data for 17.5 cm was lost, so the other 3 points were used.

The method for calculating the experimental u max's was to use python to calculate all of the local minimum and maxima. There were a few outliers, but otherwise to the fourth decimal place there was a minimum and a maximum for each depth that was very consistent.The amplitude of the wave was calculated using the equation for u_max shown below and averaged to find an overall amplitude of 0.0136 m.

$u = a\sigma\frac{cosh(k(h+z))}{sinh(kh)}cos(kx-\sigma t)$

$u_{max} = a\sigma\frac{cosh(k(h+z))}{sinh(kh)}$

| Depth (cm) | $u_{max} \frac{m}{s}$ |
|:---------- |:--------------------- |
| 7.5        | .123                  |
| 10.2       | .0668                 |
| 12.5       | .0565                 |


![u_max](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/u_max.png?raw=true)

# Water particle velocity (w):
The same analysis was used for w velocity as u velocity, as it was the same wave the amplitude was assumed to be the same.

$w = a\sigma\frac{sinh(k(h+z))}{sinh(kh)}sin(kx-\sigma t)$
$w_{max} = a\sigma\frac{sinh(k(h+z))}{sinh(kh)}$

| Depth (cm) | $w_{max} \frac{m}{s}$ |
|:---------- |:--------------------- |
| 7.5        | .0281                 |
| 10.2       | .0122                 |
| 12.5       | .0110                 |

![w_max](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/w_max.png?raw=true)

# Discussion of experiments and results, and comparison to theory from lectures.

For the first dispersion relation part, the points are expected to be relativity close to the predicted line. Theses are not close, but do follow the same trends. The error could be caused by measurement error from our ruler. It could also be caused by the fact that waves in the tank are reflected which can interfere with the wave gauge data. There is also a possibility that when the team measured the wavelength, different multiples of wavelength were measured. The data also did not fit well to the phase velocity prediction.

For the w and u velocities graphs the general trend is consistent with the data, however the points do not line up. A significant factor that linear wave theory neglects is friction from the bottom of the bed. There is friction from the bed in the real world and for the u velocity this is a reason that the predicted velocity is much higher than the measured velocity. Another factor that could effect the ADV data is harmonics and reflections causing velocity to be lower or higher than the velocity of just the wave.
