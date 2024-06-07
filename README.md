# Major Haemorrhage Protocol

Major haemorrhage is defined as blood loss requiring ≥4 units of Red Blood Cells (RBC) in the past hour, or blood loss with an anticipated need for ≥4 units RBC in the next hour. During treatment of major haemorrhage, 3 main blood components need to be administered: RBC, Fresh Frozen Plasma (FFP) and Platelets. The majority of evidence from clinical literature indicates that transfusion of these 3 blood components, specifically in a 1:1:1 ratio, maximizes survival outcomes for patients. Hospitals will generally institute their own major haemorrhage protocols to ensure the appropriate blood components are administered such that this ideal ratio is met.

Data has been collected for major haemorrhage cases from 2017 - present. This data includes Date, Age, Gender, Reason for protocol activation, Time of activation, 30 Day survival as well as the number of units of each blood component that were ordered, transfused and wasted. The objectives of this project were to:

- Assess whether transfusion ratios have improved since 2017
- Assess whether any changes in transfusion ratios can be correlated with changes in survival rate
- Provide a means of easily monitoring the ratios going forward
- Provide a means of monitoring blood component wastage

Data for the individual cases were entered into an excel spreadsheet and transfusion ratios for each case were calculated. Average ratios and 30 day survival rates for each year were then calculated along with total units ordered, transfused, wasted and the percentage waste for each year. This information was then moved to a database (template shown in Images - Database Template.png) and displayed on a dashboard created in powerBI.

![MTP Overview](Images/MTP%20Overview.png)

Individual reports for each year were also created, with an example shown below.

![MTP Overview](Images/MTP%202022%20Report.png)

Given that there is substantial evidence to support the use of a 1:1:1 ratio, the research hypothesis is that increased disparity between administered and optimal ratios will be negatively correlated with 30 day survival rates. The percentage disparity from optimal ratio was calculated for each year and linear regression analysis was performed.

```python
import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import linregress

data = pd.read_csv(file_path)
df = data.sort_values(by='Ratio Disparity')

x = df['Ratio Disparity']
y = df['Survival Rate']

slope, intercept, r_value, p_value, std_err = linregress(x,y)
linear_fit = slope*x + intercept

resid = y - linear_fit

plt.figure()
plt.scatter(x, y, s=20)
plt.plot(x, linear_fit, color='r', linewidth=1)
plt.title('Ratio Disparity vs Survival Rate')
plt.ylim(bottom=0)
plt.ylim(top=100)
plt.xlabel('Ratio Disparity (%)')
plt.ylabel('Survival Rate (%)')

plt.figure()
plt.scatter(x, resid, s=20)
plt.axhline(0, color='black', linestyle='--', linewidth=1)
plt.title('Ratio Disparity vs Residuals')
plt.xlabel('Ratio Disparity (%)')
plt.ylabel('Residuals')
```

![MTP Overview](Images/Regression%20Analysis.png)




