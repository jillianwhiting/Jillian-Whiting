```python
from aide_design.play import*
x = 0.2
X = 0.15
K = 2
delta_t = 1
def Co(X,delta_t,K):
  Co = (-2 * X + delta_t / K)/(2 * (1 - X) + delta_t / K)
  return Co

def C1(X,delta_t,K):
  C1 = (2 * X + delta_t / K)/(2 * (1 - X) + delta_t / K)
  return C1

def C2(X,delta_t,K):
  C2 = (2 * (1 - X) - delta_t / K)/(2 * (1 - X) + delta_t / K)
  return C2

C_0 = Co(X,delta_t,K)
C_1 = C1(X,delta_t,K)
C_2 = C2(X,delta_t,K)

if C_0 + C_1 + C_2 == 1:
  print('Valid')
```

$Q_{e2} = c_{0}Q_{i2} + c_{1}Q_{i1} +c_{2}Q_{e1}$

```python
time = np.array([1:21])
Q_i = np.array([])
```
