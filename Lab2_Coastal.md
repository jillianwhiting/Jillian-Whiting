```python
from aide_design.play import*
```
Jillian Whiting

jpw236

CEE4530 Lab 1

#Introduction:
A brief introduction to the lab and the purpose of the experiments.
#Experimental Setup:
A brief description of the wave tank facility, wave gauges, ADV
accompanied by a diagram showing the experimental setup.
#Dispersion relation:
Plots of $\frac{\sigma^{2}h}{g}$ as a function of $kh$. Two separate plots for the two different sets of amplitudes. On each plot, plot your experimental results and also the theoretical curve (as in HW1).
$\frac{\sigma^{2}h}{g} = \frac{kh}{tanh^{-1}(kh)}$


```python
wavelength = np.array([41,47,24,35.5,15,14.25]) *u.inches
k = ((2 * np.pi) / wavelength).to(1/u.m)
k = (k * u.m) .to(u.dimensionless)
k_5 = np.array([k[0],k[2],k[4]])
k_10 = np.array([k[1],k[3],k[5]])
h = 19.5 * u.cm
kh = (k * h).to(u.dimensionless)
time = np.array([1.82,1.82,1,1,0.645,0.645])
sigma = (2 * np.pi) / time
sigma_5 = np.array([sigma[0],sigma[2],sigma[4]]) / u.s
sigma_10 = np.array([sigma[1],sigma[3],sigma[5]]) / u.s
sigma2hg = ((sigma**2) * h / g).to(u.dimensionless)
kh_5 = np.array([kh[0],kh[2],kh[4]])


kh_5_1 = kh_5 * np.tanh(kh_5)
kh_10 = np.array([kh[1],kh[3],kh[5]])
kh_10_1 = kh_10 * np.tanh(kh_10)
sigma2hg_5 = np.array([sigma2hg[0],sigma2hg[2],sigma2hg[4]])
sigma2hg_10 = np.array([sigma2hg[1],sigma2hg[3],sigma2hg[5]])
plt.plot(kh_5,kh_5_1)
plt.plot(kh_5,sigma2hg_5,'*')
plt.show()

plt.plot(kh_10,kh_10_1)
plt.plot(kh_10,sigma2hg_10,'*')
plt.show()
```

#Dispersion relation:
Calculation of wave celerity, cp. Plots of cp/gh as a function of kh. Two separate plots for the two different sets of amplitudes. On each plot, plot your experimental results and also the theoretical curve (as in HW1).
$c_{p} = \frac{\sigma}{k}$
```python
cp_5 = sigma_5 / k_5
cp_10 = sigma_10/k_10
cpgh_5 = cp_5 / (g * h)
cpgh_10 = cp_10 / (g * h)
plt.plot(kh_5,cpgh_5,'*')
plt.show()

plt.plot(kh_10,cpgh_10,'*')
plt.show()

```
#Water particle velocity:
a vertical profile of maximum horizontal velocity (umax on the
horizontal axis, z on the vertical axis). Plot the theoretical curve on the same plot.
Explanation of your methods of analysis.
#Water particle velocity:
a vertical profile of maximum vertical velocity (wmax on the
horizontal axis, z on the vertical axis). Plot the theoretical curve on the same plot.
Explanation of your methods of analysis.
#Discussion of experiments and results, and comparison to theory from lectures.
