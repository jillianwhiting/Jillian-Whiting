```python
from aide_design.play import*
```
Jillian Whiting

jpw236

CEE4530 Lab 1

#Introduction:
This lab was two parts each of which tested experimental data against theoretical models. The first part was using two wave gauges to match wave data. If the wave data was overlapping from the two gauges there were a wavelength apart(or at least an integer multiple of the wavelength).The second part of the lab was using the ADV to measure the difference in velocity at different depths beneath a wave. A thing that I think was helpful from the lab was getting a physical feeling for the theoretical relationships.
#Experimental Setup:
A brief description of the wave tank facility, wave gauges, ADV
accompanied by a diagram showing the experimental setup.
There were two parts of the experimental set-up, the first being the wave gauges. The wave gauges send out signals which bounce off the water surface and therefore can measure distance to the surface. There was one stationary wave gauge and one on a slide as shown in the schematic below:
![experimental_apparatus_1](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/experimental_apparatus_1.png?raw=true)
The wave gauge could be slid along the top of the tank until the distance between represented one wavelength because the wave gauge data lined up.

The second part of the apparatus was using the ADV.The ADV measures velocity in all directions. It uses acoustics to measure velocity. Extra seeding particles needed to be added to the tank, which is not surprising as we were the first group. The ADV unlike the wave gauges is submerged in water. During the test it was submerged at four elevations below water level, 7.5 cm, 10.5 cm, 12.5 cm, 17.5 cm.
![experimental_apparatus_2](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/experimental_apparatus_2.png?raw=true)

#Dispersion relation:
Plots of $\frac{\sigma^{2}h}{g}$ as a function of $kh$. Two separate plots for the two different sets of amplitudes. On each plot, plot your experimental results and also the theoretical curve (as in HW1).

$k = \frac{2\pi}{\lambda}$
$\sigma = \frac{2\pi}{T}$

$\frac{\sigma^{2}h}{g} = \frac{kh}{tanh^{-1}(kh)}$


```python
wavelength = np.array([41,47,24,35.5,15,14.25]) *u.inches
wavelength = wavelength.to(u.m)
k = ((2 * np.pi) / wavelength).to(1/u.m)
k_dim = (k * u.m) .to(u.dimensionless)
k_5 = np.array([k_dim[0],k_dim[2],k_dim[4]])
k_10 = np.array([k_dim[1],k_dim[3],k_dim[5]])
h = 19.5 * u.cm
kh = (k * h).to(u.dimensionless)
time = np.array([1.82,1.82,1,1,0.645,0.645])
sigma = (2 * np.pi) / time
sigma_5 = np.array([sigma[0],sigma[2],sigma[4]]) / u.s
sigma_10 = np.array([sigma[1],sigma[3],sigma[5]]) / u.s
sigma2hg = (((sigma/u.s)**2) * h / pc.gravity).to(u.dimensionless)
kh_5 = np.array([kh[0],kh[2],kh[4]])
kh_5_1 = kh_5 * np.tanh(kh_5)
kh_10 = np.array([kh[1],kh[3],kh[5]])
kh_10_1 = kh_10 * np.tanh(kh_10)
sigma2hg_5 = np.array([sigma2hg[0],sigma2hg[2],sigma2hg[4]])
sigma2hg_10 = np.array([sigma2hg[1],sigma2hg[3],sigma2hg[5]])
plt.plot(kh_5,kh_5_1)
plt.plot(kh_5,sigma2hg_5,'*')
plt.title('Dispersion Relation of different wavelengths')
plt.xlabel('kh')
plt.ylabel('sigma squared / hg')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/dispersion_5')
plt.show()

plt.plot(kh_10,kh_10_1)
plt.plot(kh_10,sigma2hg_10,'*')
plt.title('Dispersion Relation of different wavelengths')
plt.xlabel('kh')
plt.ylabel('sigma squared / hg')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/dispersion_10')
plt.show()
```
![amplitude 5](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/dispersion_5.png?raw=true)
![amplitude 10](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/dispersion_10.png?raw=true)


#Dispersion relation:
Calculation of wave celerity, cp. Plots of cp/gh as a function of kh. Two separate plots for the two different sets of amplitudes. On each plot, plot your experimental results and also the theoretical curve (as in HW1).
$c_{p} = \sqrt{\frac{tanh(kh)}{kh}} \sqrt{gh}$

$c_{p} = \frac{\sigma}{k}$

```python
cp_5 = (sigma_5 / k_5)
cp_10 = sigma_10 / k_10
cpgh_5 = cp_5 / (pc.gravity * h.to(u.m))

cpgh_10 = cp_10 / (pc.gravity * h.to(u.m))
predict_5 = np.sqrt(np.tanh(kh_5)/kh_5)/np.sqrt(pc.gravity * h.to(u.m))
predict_10 = np.sqrt(np.tanh(kh_10)/kh_10)/np.sqrt(pc.gravity * h.to(u.m))

plt.plot(kh_5, predict_5)
plt.plot(kh_5,cpgh_5,'*')
plt.title('Wave Celerity of different wavelengths')
plt.xlabel('kh')
plt.ylabel('cp/gh')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/celerity_5')
plt.show()

plt.plot(kh_10, predict_10)
plt.plot(kh_10,cpgh_10,'*')
plt.title('Wave Celerity of different wavelengths')
plt.xlabel('kh')
plt.ylabel('cp/gh')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/celerity_10')
plt.show()

```
![celerity_5 5](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/celerity_5.png?raw=true)
![celerity_10](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/celerity_10.png?raw=true)

#Water particle velocity:
a vertical profile of maximum horizontal velocity (umax on the
horizontal axis, z on the vertical axis). Plot the theoretical curve on the same plot.
Explanation of your methods of analysis.

```python
sigma = 2*np.pi
h = 0.195
k = 5.194
z1 = 0.102
z2 = 0.125
z3 = 0.175
file1 = pd.read_csv('10_2.csv')
u_vel_10_2 = file1["u_vel"].values
w_vel_10_2 = file1["w_vel"].values
v_vel_10_2 = file1["v_vel"].values
file2 = pd.read_csv('12_5.csv')
u_vel_12_5 = file2["u_vel"].values
w_vel_12_5 = file2["w_vel"].values
v_vel_12_5 = file2["v_vel"].values
file3 = pd.read_csv('17_5.csv')
u_vel_17_5 = file3["u_vel"].values
w_vel_17_5 = file3["w_vel"].values
v_vel_17_5 = file3["v_vel"].values
def local(vector):
  N = len(vector)
  local_min = []
  local_max = []
  for i in range(N-50):
    if vector[i+25] == min(vector[i:i+50]):
      local_min = local_min+[i+25]
    elif vector[i+25]==max(vector[i:i+50]):
      local_max = local_max+[i+25]
  return local_max,local_min
def amp(u,z,sigma,k,h):
  a = u/sigma * np.sinh(k*h)/np.cosh(k*(h-z))
  return a
[local_max_u,local_min_u] = local(u_vel_10_2[500:])
local_max_u_val_10_2 = u_vel_10_2[local_max_u]
local_min_u_val_10_2 = u_vel_10_2[local_min_u]
u_max_10_2 = (local_max_u_val_10_2 - local_min_u_val_10_2) / 2
a_10_2 = amp(u_max_10_2,z1,sigma,k,h)
[local_max_u,local_min_u] = local(u_vel_12_5[500:])
local_max_u_val_12_5 = u_vel_12_5[local_max_u]
local_min_u_val_12_5 = u_vel_12_5[local_min_u[0:94]]
u_max_12_5 = (local_max_u_val_12_5 - local_min_u_val_12_5) / 2
a_12_5 = amp(u_max_12_5,z2,sigma,k,h)
print(a_12_5)
[local_max_u,local_min_u] = local(u_vel_17_5[500:])
local_max_u_val_17_5 = u_vel_17_5[local_max_u]
local_min_u_val_17_5 = u_vel_17_5[local_min_u]
print(local_max_u_val_17_5)
plt.plot(u_vel_17_5)
a_12_5 = amp(.123,.075,sigma,k,h)
print(a_12_5)

```
By inspection of the local minimums and maximums, the amplitude of the maximum u velocity was:
| Depth (cm) | $u_{max} \frac{m}{s}$ | amplitude |
|:---------- |:--------------------- |:--------- |
| 7.5        | .123                  | .0195     |
| 10.2       | .0668                 | 0.0113    |
| 12.5       | .0565                 | 0.0100    |

The average amplitude is 0.0136 m.

$u = a\sigma\frac{cosh(k(h+z))}{sinh(kh)}cos(kx-\sigma t)$
$u_{max} = a\sigma\frac{cosh(k(h+z))}{sinh(kh)}$

```python
a = 0.0136
z = np.array([0.075,0.102,0.125])*-1
u_max = a * sigma * np.cosh(k*(h+z))/np.sinh(k*h)
print(u_max)
u_max_exp = np.array([0.123,0.0668,0.0565])
plt.plot(u_max,z)
plt.plot(u_max_exp,z,'*')
plt.title('Maximum Horizontal Velocity')
plt.xlabel('u max (m/s)')
plt.ylabel('z(m)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/u_max')
plt.show()
```
![u_max](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/u_max.png?raw=true)
#Water particle velocity:
a vertical profile of maximum vertical velocity (wmax on the
horizontal axis, z on the vertical axis). Plot the theoretical curve on the same plot.
Explanation of your methods of analysis.

$w = a\sigma\frac{sinh(k(h+z))}{sinh(kh)}sin(kx-\sigma t)$
$w_{max} = a\sigma\frac{sinh(k(h+z))}{sinh(kh)}$

```python
[local_max_w,local_min_w] = local(w_vel_10_2[500:])
local_max_w_val_10_2 = w_vel_10_2[local_max_w]
local_min_w_val_10_2 = w_vel_10_2[local_min_w]
w_max_10_2 = (local_max_w_val_10_2 - local_min_w_val_10_2) / 2
[local_max_w,local_min_w] = local(w_vel_12_5[500:])
local_max_w_val_12_5 = w_vel_12_5[local_max_w]
local_min_w_val_12_5 = w_vel_12_5[local_min_w]
w_max_12_5 = (local_max_w_val_12_5 - local_min_w_val_12_5) / 2
print(w_max_12_5)
[local_max_w,local_min_w] = local(w_vel_17_5[500:])
local_max_w_val_17_5 = w_vel_17_5[local_max_w]
local_min_w_val_17_5 = w_vel_17_5[local_min_w]
print(local_min_w_val_17_5)

```
By inspection of the local minimums and maximums, the amplitude of the maximum u velocity was:
| Depth (cm) | $w_{max} \frac{m}{s}$ |
|:---------- |:--------------------- |
| 7.5        | .0281                 |
| 10.2       | .0122                 |
| 12.5       | .0110                 |

```python
w_max = a * sigma * np.sinh(k*(h+z))/np.sinh(k*h)

w_max_exp = np.array([0.2881,0.0112,0.0110])
plt.plot(w_max,z)
plt.plot(w_max_exp,z,'*')
plt.title('Maximum vertical velocity')
plt.xlabel('w max (m/s)')
plt.ylabel('z(m)')
plt.savefig('/Users/jillianwhiting/github/Jillian-Whiting/Images/w_max')
plt.show()
```
![w_max](https://github.com/jillianwhiting/Jillian-Whiting/blob/master/Images/w_max.png?raw=true)
#Discussion of experiments and results, and comparison to theory from lectures.

For the first dispersion relation part, the points are expected to be relativity close to the predicted line. Theses are not close, but do follow the same trends. The error could be caused by measurement error from our ruler. It could also be caused by the fact that waves in the tank are reflected which can interfere with the wave gauge data. There is also a possibility that when the team measured the wavelength, different multiples of wavelength were measured. The data also did not fit well to the phase velocity prediction.
