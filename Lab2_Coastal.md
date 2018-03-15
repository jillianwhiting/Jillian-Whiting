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

The points are expected to be relativity close to the predicted line. Theses are not close, but do follow the same trends. The error could be caused by measurement error from our ruler. It could also be caused by the fact that waves in the tank are reflected which can interfere with the wave gauge data.

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
file = pd.read_csv('10_2.csv')
u_vel = file["u_vel"].values
w_vel = file["w_vel"].values
v_vel = file["v_vel"].values
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

[local_max_u,local_min_u] = local(u_vel[500:])
local_max_u_val = u_vel[local_max_u]
print(local_max_u_val)
local_min_u_val = u_vel[local_min_u]
plt.plot(u_vel)
a = np.mean(local_max_u_val)
print(a)

```
#Water particle velocity:
a vertical profile of maximum vertical velocity (wmax on the
horizontal axis, z on the vertical axis). Plot the theoretical curve on the same plot.
Explanation of your methods of analysis.
#Discussion of experiments and results, and comparison to theory from lectures.
