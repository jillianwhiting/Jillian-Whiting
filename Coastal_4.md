```python
# function that we need
from aide_design.play import*
import scipy as scipy
from scipy import optimize

# importing the data
file = pd.read_csv('hw4.csv')
time = file["Time"].values[0:101]
free_surface = file["WaveGauge"].values[0:101]
# data frequency
f_s = 0.1
def zero_crossings(vector):
  # find the locations of the zero crossings in a vector recording the lower bound of the the crossing
  N = len(vector)
  zero_crossings = []
  for i in range(N-1):
    if vector[i] < 0 and vector[i+1]>=0:
      # This is a zero crossing, record location
      zero_crossings = zero_crossings +[i]
  return zero_crossings

record_length = len(free_surface)
zero_crossings = zero_crossings(free_surface)
n_zero_cross = len(zero_crossings)
n_crests = n_zero_cross - 1

# Create Period vector
T = []
for i in range(n_crests):
  # records period of each crossing
  T = T + [time[zero_crossings[i+1]-zero_crossings[i]]]

def wave_height(vector,zero_crossings):
  #records the height of each wave defined by the zeros crossing
  N = len(zero_crossings)
  wave_height = []
  for i in range(N-1):
    #calculates amplitude of the wave
    amplitude = (max(vector[zero_crossings[i]:zero_crossings[i+1]]) - min(vector[zero_crossings[i]:zero_crossings[i+1]]))
    wave_height = wave_height + [amplitude]
  return wave_height

wave_height = wave_height(free_surface,zero_crossings)
print(wave_height)
#plotting histogram
df = pd.DataFrame(wave_height)
df.hist()
plt.xlabel('Wave Height (cm)')
plt.ylabel('Number of occurrences')
plt.title('Random Wave Height Distribution')
plt.show()

#create sorted height vector
height_sort = np.sort(wave_height)
n = len(zero_crossings) -1
def h_rms(n,hi):
  #calculates hrms of the sorted vector with n-1 zero crossings
  x = 0
  for i in range(n):
    x = x + hi[i]**2
  h_rms = np.sqrt(1 / n * x)
  return h_rms

h_rms = h_rms(n,height_sort)
print(h_rms)

def h_significant(n,hi):
  #calculates the significant wave height (h1/3)
  h_significant = 3 / n *np.sum(hi[round(2*n/3):n])
  return h_significant
h_1_3 = h_significant(n,height_sort)
print(h_1_3)

#create sorted T vector
T_sort = np.sort(T)
#calculates Tz
T_z = record_length/n_zero_cross * f_s
print(T_z)
#calculates Tc
T_c = record_length/n_crests * f_s
print(T_c)
#calculates T 1/3
T_1_3 = h_significant(n,T_sort)
print(T_1_3)

#creates frequency vector from 0.1 to 4 Hz
freq = np.arange(.5,4,0.01)

#uses equation given in homework to calculate the spectra points
spectrum = 0.205 * (h_1_3**2) * (T_1_3**-4) * (freq**-5) * np.exp(-0.75 * ((T_1_3 * freq)**-4))

#creates spectra plot
plt.plot(freq,spectrum)
plt.xlabel('freq (1/s)')
plt.ylabel('energy (cm^2/s)')
plt.title('Wave Energy Spectrum')
plt.show()

#finding the frequency of the maximum energy of the spectrum and the area under the curve
sum = 0
for i in range(len(spectrum)):
  if spectrum[i] == max(spectrum):
    print(freq[i])
  sum = sum + spectrum[i]*0.01
print(sum)

np.var(wave_height)
a_rms = np.sqrt(2*sum)
```

The frequency of the maximum energy occurrence is 1.06 Hz.

$P =\rho g \eta\frac{cosh(k(h+z))}{cosh(kh)}$

In head form:
$S_{pp} =S_{\eta\eta} \frac{cosh(k(h+z))}{cosh(kh)}$

$z = h$
$S_{pp} =S_{\eta\eta} \frac{cosh(2kh)}{cosh(kh)}$

```python

h = 10
T = 0.92
k = 0.756

S_pp = spectrum * (1/np.cosh(k*h))**2

plt.plot(freq,S_pp)
```

```python
# Defined parameters
T = 10
sigma = 2 *np.pi / T
gamma_2 = -0.5
gamma_1 = 0.5
s = 1/50
xo = -1000
theta = np.pi/6
ao = 1
wavelength = 5000
#create x direction vector
x = np.arange(-1000, 0, 1)
#define bathymetry functions
h_1 =s*x
h_2 = s*(x + gamma_1 * xo *np.exp(-((x-xo)**2)/wavelength))
h_3 = s*(x + gamma_2 * xo *np.exp(-((x-xo)**2)/wavelength))

#Plot bathymetry
plt.plot(x,h_1)
plt.plot(x,h_2)
plt.plot(x,h_3)
plt.xlabel('Distance(x) (m)')
plt.ylabel('Bathymetry(h) (m)')
plt.legend(['Case 1','Case 2','Case 3'])
plt.show()
```
Wave Rays
They are uniform in the y-direction so Snell's Law is valid.

$\frac{dy}{dx} = \frac{\kappa}{\sqrt{k^{2}-\kappa^{2}}}$

$dy = \frac{\kappa  dx}{\sqrt{k^{2}-\kappa^{2}}}$

$\kappa = ksin\alpha$

$\overline{E}^{+}c_{go} = \overline{E}^{+}c_{g1}$

$a_{o}^{2}c_{go} = a_{1}^{2}c_{g1}$

$a_{1}= \sqrt{\frac{a_{0}^{2}c_{g0}}{c_{g1}}}$


```python
#Based on our x vector definition
dx =1
g = 9.81
def k(sigma,h):
  #function that uses a root finder to solve for k for any sigma and h
  def disp_fun(k):
    return sigma**2 - 9.81 * k *np.tanh(k*h)
  return scipy.optimize.root(disp_fun,0.2).x
def n(k,h):
  return(0.5 * (1 + (2*k*h)/(np.sinh(2*k*h))))
kappa_1 = k(sigma,20)*np.sin(theta)
# solving for kappa for case 1
y_1= np.zeros(len(x))
a_1= np.zeros(len(x))
a_1[0]=ao
#keeps track of y movement
y_1[0] = 0
cg_1 = sigma / k(sigma,20)
for i in range(len(x)-1):
  h = -1*h_1[i+1]
  k_val = k(sigma,h)
  #solves for y position based on equations above
  y_1[i+1] = y_1[i] + kappa_1 * dx / np.sqrt(k_val**2-kappa_1**2)
  a_1[i+1] = np.sqrt(ao**2 * cg_1 / (sigma / k_val * n(k_val,h)))
#repeated forother cases
kappa_2 = k(sigma,30)*np.sin(theta)
a_2= np.zeros(len(x))
a_2[0]=ao
y_2= np.zeros(len(x))
y_2[0] = 0
cg_2 = sigma / k(sigma,30)
for i in range(len(x)-1):
  h = -1*h_2[i+1]
  k_val = k(sigma,h)
  y_2[i+1] = y_2[i] + kappa_2 * dx / np.sqrt(k_val**2-kappa_2**2)
  a_2[i+1] = np.sqrt(ao**2 * cg_2 / (sigma / k_val * n(k_val,h)))
kappa_3 = k(sigma,10)*np.sin(theta)
a_3= np.zeros(len(x))
a_3[0]=ao
y_3= np.zeros(len(x))
y_3[0] = 0
cg_3 = sigma / k(sigma,10)
for i in range(len(x)-1):
  h = -1* h_3[i+1]
  k_val = k(sigma,h)
  y_3[i+1] = y_3[i] + kappa_3 * dx / np.sqrt(k_val**2-kappa_3**2)
  a_3[i+1] = np.sqrt(ao**2 * cg_3 / (sigma / k_val * n(k_val,h)))

#plotting the wave rays
plt.plot(y_1,x)
plt.plot(y_2,x)
plt.plot(y_3,x)
plt.xlabel('Along Shore Direction (m)')
plt.ylabel('Distance From Shore (m)')
plt.legend(['Case 1','Case 2','Case 3'])
plt.show()

plt.plot(x[1:],a_1[1:])
plt.plot(x[1:],a_2[1:])
plt.plot(x[1:],a_3[1:])
plt.xlabel('Along Shore Direction (m)')
plt.ylabel('Distance From Shore (m)')
plt.legend(['Case 1','Case 2','Case 3'])
plt.show()


```
