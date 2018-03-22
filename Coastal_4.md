```python
from aide_design.play import*
file = pd.read_csv('hw4.csv')
time = file["Time"].values[1:101]
free_surface = file["WaveGauge"].values[1:101]

def zero_crossings(vector):
  N = len(vector)
  zero_crossings = []
  for i in range(N-1):
    if vector[i] < 0 and vector[i+1]>=0:
      zero_crossings = zero_crossings +[i]
  return zero_crossings
t_r = len(free_surface)
zero_crossings = zero_crossings(free_surface)
n_z = len(zero_crossings)
n_c = n_z - 1
T = []
for i in range(n_c):
  T = T + [time[zero_crossings[i+1]-zero_crossings[i]]]
def wave_height(vector,zero_crossings):
  N = len(zero_crossings)
  wave_height = []
  for i in range(N-1):
    amplitude = max(vector[zero_crossings[i]:zero_crossings[i+1]]) - min(vector[zero_crossings[i]:zero_crossings[i+1]])
    wave_height = wave_height + [amplitude]
  return wave_height

wave_height = wave_height(free_surface,zero_crossings)
print(wave_height)
df = pd.DataFrame(wave_height)
df.hist()

hi = np.sort(wave_height)
n = len(zero_crossings) -1

def h_rms(n,hi):
  x = 0
  for i in range(n):
    x = x + hi[i]**2
  h_rms = np.sqrt(1 / n * x)
  return h_rms

h_rms = h_rms(n,hi)

def h_significant(n,hi):
  h_significant = 3 / n * sum(hi[round(2*n/3):n])
  return h_significant

h_1_3 = h_significant(n,hi)

T_z = t_r/n_z

T_c = t_r/n_c

T_1_3 = h_significant(n,T)

freq = np.linspace(.01,4,100)

spectrum = 0.205 * h_1_3**2 * T_1_3**-4 * freq**-5 * np.exp(-0.75 * (T_1_3 * freq)**-4)

plt.plot(freq,spectrum)
plt.xlabel('freq (1/s)')
plt.ylabel('energy (m^2/s)')
plt.title('Wave Energy Spectrum')
plt.show()

## The max wave energy is 1.47
## It occurs at the frequency of 1.05 1/s
```


```python
T = 10
gamma = -0.5
s = 1/50
xo = -10000
yo = 0
theta = 30
ao = 1
wavelength = 5000
x = np.arange(0, 10000, 100)
h_1 = s * (x + gamma*xo *np.exp(-((x-xo)**2)/wavelength))
plt.plot(x,h_1)
 ```
