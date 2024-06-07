# Blood Transfusion in Major Haemorrhage

Major haemorrhage is defined as blood loss requiring ≥4 units of Red Blood Cells (RBC) in the past hour, or blood loss with an anticipated need for ≥4 units RBC in the next hour. During treatment of major haemorrhage, 3 main blood components need to be administered: RBC, Fresh Frozen Plasma (FFP) and Platelets. The majority of evidence from clinical literature indicates that transfusion of these 3 blood components, specifically in a 1:1:1 ratio, maximizes survival outcomes for patients. Hospitals will generally institute their own major haemorrhage protocols to ensure the appropriate blood components are administered such that this ideal ratio is met.

Data has been collected for major haemorrhage cases from 2017 - present. This data includes Date, Age, Gender, Reason for protocol activation, Time of activation, 30 Day survival as well as the number of units of each blood component that were ordered, transfused and wasted. The objectives of this project were to:

1) Provide a means of easily monitoring the ratios going forward
2) Provide a means of monitoring blood component wastage
3) Assess whether transfusion ratios have improved since 2017
4) Assess whether any improvements in transfusion ratio can be correlated with improvements in survival rate
5) Assess any changes in blood component wastage from 2017

## Objectives 1 & 2
Data for the individual cases were entered into an excel spreadsheet and transfusion ratios for each case were calculated. Average ratios and 30 day survival rates for each year were then calculated along with total units ordered, transfused, wasted and the percentage waste for each year. This information was then moved to a database with the following template and displayed on a dashboard created in powerBI.

![MTP Overview](Images/Database%20Template.png)

Overview Dashboard

![MTP Overview](Images/MTP%20Overview.png)

Individual reports for each year were also created, with an example shown below.

![MTP Overview](Images/MTP%202022%20Report.png)

Identifying the specific data of interest, implementing a systematic way of recording this data and providing a way of visualizing and interpreting this data will ensure any metrics of interest can be tracked going forward.

## Objective 3
To assess the improvement in transfusion ratios that can be seen from the Overview dashboard, the average percentage difference of transfused blood components from the optimal amount according to the 1:1:1 ratio was calculated for each year and used as a measure of disparity from the optimal ratio. These results are shown in the table below.

| Year     | Ratio Disparity (%)   | 30 Day Survival Rate (%)  |
| ---------| --------------------- | --------------------------|
| 2017     | 40.3                  | 42.9                      |
| 2018     | 24.0                  | 46.7                      |
| 2019     | 9.7                   | 56.3                      |
| 2020     | 14.0                  | 66.7                      |
| 2021     | 13.3                  | 91.7                      |
| 2022     | 8.3                   | 84.0                      |
| 2023     | 16.7                  | 69.2                      |

From this table it can be seen that transfusion ratios improve significantly from 2019 onwards. This was to be expected following an update to the transfusion protocol in 2019 which aimed specifically to improve the ratio. Overall, the ratio disparity has decreased by 23.6% from 2017 in absolute terms, with a relative reduction of 41.4%.

## Objective 4
Given that there is substantial clinical evidence to support the use of a 1:1:1 transfusion ratio, the research hypothesis is that increased disparity between administered and optimal ratios will be negatively correlated with 30 day survival rates. To assess this, linear regression analysis was performed

```python
import pandas as pd
import matplotlib.pyplot as plt
from scipy.stats import linregress

data = pd.read_csv(file_path)

df = data.sort_values(by='Ratio Disparity')

x = df['Ratio Disparity']
y = df['Survival Rate']

slope, intercept, r_value, p_value = linregress(x,y)
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

The r value was calculated to be -0.703 (p value = 0.039) indicating a moderate to strong negative linear correlation between ratio disparity and survival rate. This relationship is statistically significant assuming an alpha value of 0.05, providing some evidence in support of the research hypothesis. The obtained result is also in agreement with the clinical literature, further supporting the idea that major haemorrhage protocols should aim for a 1:1:1 ratio of RBC:FFP:Platelets to maximize survival outcomes.

The R<sup>2</sup> value is 0.494, indicating ratio disparity accounts for roughly 50% of the variance in survival rate. An R<sup>2</sup> value on the lower side would be expected given the abundance of other factors which impact survival, hence this linear model would not be sufficient for predicting survival rates. To provide stronger predictive power, a multifactorial model would need to be used accounting for Age, Gender, Injury severity and mechanism, time to first transfusion etc. in addition to the transfusion ratio, although this was beyond the scope of this project.

## Objective 5
Percentage waste for each blood component is shown below.

| Year     | RBC Waste (%)   | FFP Waste (%)  | Platelet Waste (%) |
| ---------| ----------------| ---------------|--------------------|
| 2017     | 8.5             | 39.4           | 21.4               |
| 2018     | 0.9             | 32.9           | 21.4               |
| 2019     | 16.8            | 39.5           | 10.5               |
| 2020     | 7.2             | 36.7           | 0                  |
| 2021     | 11.9            | 27.8           | 6.25               |
| 2022     | 10.2            | 26.1           | 5.1                |
| 2023     | 7.9             | 25.6           | 11.1               |

Comparing waste between 2017 and 2023, there is a 0.6%, 13.8% and 10.3% reduction in waste for RBC, FFP and Platelets respectively in absolute terms, with relative reductions of 7.1%, 35.0% and 48.1% respectively. These results, along with trends seen in the Overview dashboard, indicate consistent reductions in waste for FFP and Platelets, whereas RBC waste has remained roughly the same. These are positive findings as RBC waste is remaining consistently low, with FFP and Platelet Waste significantly downtrending from high levels.

## Conclusion
An effective means of monitoring the key metrics: transfusion ratio, blood component wastage and survival rate has been instituted. This will enable clinicians to respond to changes in these metrics with a view to improving patient care and survival outcomes. Assessment of the available data shows improvements in transfusion ratio, correlated with improvements in survival rate. Positive trends in blood component wastage have also been observed, with significant decreases in FFP and Platelet waste, and consistently low RBC waste. 


