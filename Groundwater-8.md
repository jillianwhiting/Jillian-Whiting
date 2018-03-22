```python
def AWC(root_depth,field_capacity,wilting_point):
  AWC = root_depth * (field_capacity - wilting_point)
  return AWC

root_depth = 2
wilting_point = .15
field_capacity = .30

AWC = AWC(root_depth,field_capacity,wilting_point)

PET = np.array([1.0,2.0,4.0,6.0,8.0,10.0,10.0,8.0,6.0,4.0,2.0,1.0,1.0,2.0])/100
Precip = np.array([109.2,190.5,76.5,39.1,158.0,68.8,61.7,115.3,129.8,65.8,95.3,179.6,79.5,37.1])/1000
AWt = AWC
recharge = np.zeros(len(PET))
P_E = Precip - PET
print(P_E)
for i in range(len(PET)):
  if P_E[i]<0:
    AWt = AWt * np.exp(-PET[i]/AWC)
    recharge[i] = 0
  else:
    if AWt+P_E[i]<AWC:
      AWt = AWt + P_E[i]
      recharge[i] = 0
    else:
      recharge[i] = AWt + P_E[i] - AWC
      AWt= AWC

print(recharge*100)
```
| Month | Precipitation (cm) | PET (cm) | Recharge (cm) |
|:----- | ------------------ | -------- |:------------- |
| 1/17  | 10.92              | 1        | 9.92          |
| 2/17  | 19.05              | 2        | 17.05         |
| 3/17  | 7.65               | 4        | 3.65          |
| 4/17  | 3.91               | 6        | 0             |
| 5/17  | 15.80              | 8        | 2.36          |
| 6/17  | 6.88               | 10       | 0             |
| 7/17  | 6.17               | 10       | 0             |
| 8/17  | 11.53              | 8        | 0             |
| 9/17  | 12.98              | 6        | 0             |
| 10/17 | 6.58               | 4        | 0             |
| 11/17 | 9.53               | 2        | 6.02          |
| 12/17 | 17.96              | 1        | 16.96         |
| 1/18  | 7.95               | 1        | 6.95          |
| 2/18  | 3.711              | 2        | 1.71          |


Streams are gaining water mostly in the winter and losing water in the summer months.

```python
root_depth = 4.0
wilting_point = 0.15
field_capacity = 0.30

AWC = 4* (0.30-.15)

PET = np.array([4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0, 4.0])/100
precip_2 =np.array([150.2,128.8,177.2,9.6,0.0,0.0,0.0,0.0,0.8,0.2,0.2,12.6,419.0,297.8])/1000
AWt = AWC
recharge = np.zeros(len(PET))
len(recharge)
P_E = precip_2 - PET
for i in range(len(PET)):
  if P_E[i]<0:
    AWt = AWt * np.exp(-PET[i]/AWC)
    recharge[i] = 0
  else:
    if AWt+P_E[i]<AWC:
      AWt = AWt + P_E[i]
      recharge[i] = 0
    else:
      recharge[i] = AWt + P_E[i] - AWC
      AWt= AWC

print(recharge*100)

```
| Month | Precipitation (cm) | PET (cm) | Recharge (cm) |
|:----- | ------------------ | -------- |:------------- |
| 1/17  | 15.02              | 4        | 11.01         |
| 2/17  | 12.88              | 4        | 8.88          |
| 3/17  | 17.72              | 4        | 13.72         |
| 4/17  | 0.96               | 4        | 0             |
| 5/17  | 0                  | 4        | 0             |
| 6/17  | 0                  | 4        | 0             |
| 7/17  | 0                  | 4        | 0             |
| 8/17  | 0                  | 4        | 0             |
| 9/17  | 0.8                | 4        | 0             |
| 10/17 | 0.2                | 4        | 0             |
| 11/17 | 0.2                | 4        | 0             |
| 12/17 | 1.26               | 4        | 0             |
| 1/18  | 41.90              | 4        | 10.38         |
| 2/18  | 29.78              | 4        | 25.78         |

In the Canning aquifer, the basin is gaining water in the winter which is also the monsoon season and then the rest of the year they are not gaining any water because it is not raining.
