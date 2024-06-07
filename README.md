# Major Haemorrhage Protocol

Major haemorrhage is defined as blood loss requiring ≥4 units of Red Blood Cells (RBC) in the past hour, or blood loss with an anticipated need for ≥4 units RBC in the next hour. During treatment of major haemorrhage, 3 main blood components need to be administered: RBC, Fresh Frozen Plasma (FFP) and Platelets. The majority of evidence from clinical literature indicates that transfusion of these 3 blood components, specifically in a 1:1:1 ratio, maximizes survival outcomes for patients. Hospitals will generally institute their own major haemorrhage protocols to ensure the appropriate blood components are administered such that this ideal ratio is met.

Data has been collected for major haemorrhage cases from 2017 - present. This data includes Date, Age, Gender, Reason for protocol activation, Time of activation, 30 Day survival as well as the number of units of each blood component that were ordered, transfused and wasted. The objectives of this project were to:

- Provide a means of easily monitoring the ratios going forward
- Provide a means of monitoring blood component wastage
- Assess whether transfusion ratios have improved since 2017
- Assess whether any changes in transfusion ratios can be correlated with changes in survival rate
- Assess any changes in blood component wastage from 2017

## Objectives 1 & 2
Data for the individual cases were entered into an excel spreadsheet and transfusion ratios for each case were calculated. Average ratios and 30 day survival rates for each year were then calculated along with total units ordered, transfused, wasted and the percentage waste for each year. This information was then moved to a database using the following template and displayed on a dashboard created in powerBI.

![MTP Overview](Images/Database%20Template.png)

![MTP Overview](Images/MTP%20Overview.png)

Individual reports for each year were also created, with an example shown below.

![MTP Overview](Images/MTP%202022%20Report.png)

Identifying the specific data of interest, implementing a systematic way of recording this data and providing a way of visualizing and interpreting this data will ensure any metrics of interest can be tracked going forward.

## Objective 3
Looking at the overview dashboard, the transfusion ratios appear to be closer to the ideal ratio, with a significant improvement from 2019 onwards. This was to be expected following an update to the transfusion protocol in 2019 which had the specific aim of improving the ratio. Average percentage disparities were calculated for each year an used to provide a quantitative assessment of the improvement.

| Year     | Ratio Disparity (%)   | 30 Day Survival Rate (%)  |
| ---------| --------------------- | --------------------------|
| 2017     | 40.3                  | 42.9                      |
| 2018     | 24.0                  | 46.7                      |
| 2019     | 9.7                   | 56.3                      |
| 2020     | 14.0                  | 66.7                      |
| 2021     | 13.3                  | 91.7                      |
| 2022     | 8.3                   | 84.0                      |
| 2023     | 16.7                  | 69.2                      |

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

fig, axs = plt.subplots(1, 2, figsize=(16, 8))

axs[0].scatter(x, y, s=20)
axs[0].plot(x, linear_fit, color='r', linewidth=1)
axs[0].set_title('Ratio Disparity vs Survival Rate')
axs[0].set_ylim(bottom=0)
axs[0].set_ylim(top=100)
axs[0].set_xlabel('Ratio Disparity (%)')
axs[0].set_ylabel('Survival Rate (%)')


axs[1].scatter(x, resid, s=20)
axs[1].axhline(0, color='black', linestyle='--', linewidth=1)
axs[1].set_title('Residuals')
axs[1].set_xlabel('Ratio Disparity (%)')
axs[1].set_ylabel('Residuals')

```

![MTP Overview](Images/Regression%20Analysis.png)

The r value was calculated to be -0.703 (p value = 0.039) indicating a moderate to strong negative linear correlation between ratio disparity and survival rate. This relationship is statistically significant assuming an alpha value of 0.05, hence the research hypothesis can be accepted. The obtained result is in agreement with the clinical literature, further supporting the idea that major haemorrhage protocols should aim for a 1:1:1 ratio of RBC:FFP:Platelets.



