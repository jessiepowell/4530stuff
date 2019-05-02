# Prelim 2019

Name:___Jessie Powell_____

This prelim is open book and open internet. You are not allowed to discuss the questions are related concepts with anyone while you are taking this prelim. You are not allowed to post questions on discussion boards. If you have questions, send an email to Monroe (monroews@gmail.com). If the question is relevant for the whole class he will post it as an issue on the course website.

For each question make sure to use units and to give variables names that are easily understood. Use print statements to **print the answers in a sentence** (except for the multiple choice!).

Name your file "yournamePrelim.md" (2 points for this!)
Send your file to the [course email](cee_4530@cornell.edu).

Each question is worth 7 points.

```python
from aguaclara.core.units import unit_registry as u
import aguaclara.research.environmental_processes_analysis as epa
import aguaclara.core.physchem as pc
import aguaclara.core.utility as ut
import numpy as np
import matplotlib.pyplot as plt
from scipy import stats
```

## Design a sand column experiment.
A sand column has an inner diameter of 1 inch and 20 cm total internal height. The sand porosity is 0.4 (40% of the column is filled with water, 60% is filled with sand). The sand density is $2650 kg/m^3$. The target approach velocity (the velocity of the water in the column if there were no sand) is 1 mm/s. The water is pumped with a peristaltic pump that delivers 2.8 mL per revolution.


1. What is the mass of sand (in grams) that must be added to the column?
2. What flow rate (in mL/s) is required?
3. What should the pump revolutions per minute be (in rev/min)? (Note that rev is a unit!)
4. What is the hydraulic residence time of the water in the column (in seconds)?

```python
#put your code here! Remember to print your answer in a sentence.

v_a = 1 * u.mm/u.s
porosity = 0.4
L_column = 20 * u.cm
D_column = 1*u.inch
A_column = pc.area_circle(D_column)
V_column = ((A_column * L_column).to(u.m**3))
density_sand = 2650 * u.kg/u.m**3
mLperrev = 2.8 * u.mL/u.revolution

#1. What is the mass of sand (g) that must be added to the column?
M_sand = (1-porosity)*density_sand*V_column
print('The mass of sand is',ut.round_sf(M_sand,2))

#2. What flow rate (mL/s) is required?
Q_column = (v_a*A_column).to(u.mL/u.sec)
print('The flow rate of the', ut.round_sf(Q_column,2))

#3. What should the pump revolutions per minute be (rev/min)?
Pump_rpm = (Q_column/mLperrev).to(u.revolution/u.min)
print('The pump rpm is',ut.round_sf(Pump_rpm,3))

#4. What is the hydraulic residence time of the water in the column (sec)?
theta = (porosity*L_column/v_a).to(u.min)
print('The hydraulic residence time is', ut.round_sf(theta,3))

```


## Acid Neutralizing Capacity

A 50 mL water sample that contains some carbonates is titrated with 0.05 N HCl. The equivalent volume of the titrant was 2 mL.

5. What was the ANC of the sample (in meq/L)?


```python
# Because there are some carbonates, the pH is higher than 3.2
V_sample = 50*u.mL
Normality_Titrant = 0.05*u.equivalent/u.mL
V_eq = 2*u.mL
ANC_sample = (V_eq*Normality_Titrant/V_sample).to(u.milliequivalent/u.mL)
print('The ANC of the sample is', ut.round_sf(ANC_sample,2))

```


## Photometer calibration
The photometer that we use in lab measures absorbance using the following equation.

$$A = -log_{10} \frac{V_{Sample} - V_{Dark}}{V_{Blank} - V_{Dark}}$$

where $log_{10} is log base 10.

The relationship between concentration of a dissolved species and the absorbance will follow Beer's law.

$$ A = \epsilon b C $$

where $\epsilon$ is the extinction coefficient that is a property of the chemical species.

The optical path length for the photometer is 19 mm. The raw voltage data for dark voltage, blank voltage, standards, and unknown are given below.

6. Use [stats.linregress](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.linregress.html) to obtain the best fit line for the calibration with absorbance as a function of concentration.
7. What is the slope, $\epsilon b$, for this data set? (Make sure to attach the correct units after doing the linear regression!)
8. What is $\epsilon$? Make sure to simplify the units to $length^2/mass$
9. Create a function in python that returns the concentration given a sample voltage. You can derive this equation from the two equations above.
10. Plot the calibration data AND the calibration line showing absorbance as a function of concentration (note that that ProCoDA shows this plot with the axis flipped). Make sure to label the axis and include a legend. And make sure that you have the same units on both plots!
11. What is the absorbance of the unknown?
12. What is the concentration of the unknown?

```python
#write the code to answer questions 6-12 here
Dark_Volts = -1.3065 * u.V
Blank_Volts = 3.49727 * u.V
Unknown_Volts = 0.1 * u.V
Sample_C = np.array([0, 10, 20, 30, 40, 50])* u.mg/u.L
Sample_Volts = np.array([3.497270,0.554016,-0.611595,-1.045119,-1.252709,-1.283894])* u.volts


#6.
Adsorbance=[]
for i in range(Sample_Volts.size):
  Adsorbance = np.append(Adsorbance, -np.log((Sample_Volts[i]-Dark_Volts)/(Blank_Volts-Dark_Volts))/np.log(10))

fig, ax = plt.subplots()
ax.plot(Sample_C, Adsorbance, 'ro', )
plt.xlabel('Concentration (mg/L)')
plt.ylabel('Adsorbance')
plt.legend('Adsorbance')
plt.show()


slope, intercept, r_value, p_value, std_err = stats.linregress(Sample_C, Adsorbance)
intercept = intercept * Adsorbance.units


ts.units/Sample_C.units



fig, ax = plt.subplots()
ax.plot(Sample_C, Sample_Volts, 'ro', )
plt.xlabel('Concentration (mg/L)')
plt.ylabel('Volts (volts)')
plt.show()

#epilon
epsilon = -(np.log((Sample_Volts[1]-Dark_Volts)/(Blank_Volts-Dark_Volts))*u.volts/((np.log(10)).to(u.volts)/intercept/Sample_C[1])




#couldn't find a log base 10 function, so had to compromise with below.
Adsorb = -(np.log((Sample_Volts-Dark_Volts)/(Blank_Volts-Dark_Volts)))/np.log(10)
Adsorb



#7.
print('The slope of the photometer calibration is', ut.round_sf(slope,3))

#8.

#9.

#10

#11.

x=[Sample_C.magnitude,Sample_Volts[-1].magnitude ]
y=[0,(Sample_Volts[-1]*slope+intercept).magnitude]
#Now plot the data and the linear regression
plt.plot(Sample_C, Sample_Volts,'o')
plt.plot(x, y,'r')

x=[V_eq.magnitude,V_titrant[-1].magnitude ]
y=[0,(V_titrant[-1]*slope+intercept).magnitude]
#Now plot the data and the linear regression
plt.plot(V_titrant, F1_data,'o')
plt.plot(x, y,'r')

```

## Multiple Choice

Use *asterisks* to italicize the answers that you select.

13. A peristaltic pump is pumping a red dye solution from a tank on the floor through a column filled with sand and activated carbon with flow down through the column. The pump is on the laboratory bench and the column is held by clamps to a stand that is on the bench. The tube that exits from the bottom of the sand column is elevated and discharges to the atmosphere (it drips into the sink) at an elevation that is higher than the tubing entering the top of the sand column. There are air bubbles entering the sand column. Which explanation(s) for the source of the air bubbles is (are) consistent with the observed results? Use what you've learned in fluid mechanics to help guide your analysis.

    A) Sand column fittings at the top of the column are leaking.

    B) *Sand column fittings at the bottom of the column are leaking.*

    C) *Instant tube fitting (push to connect fitting) between the tank and the pump is leaking.*

    D) Instant tube fitting between the pump and the sand column is leaking.

    E) *The tank of red dye is nearly empty and the pump is beginning to pull air into the end of the inlet tube.*

14. ProCoDA is reporting a concentration of 10 mg/L of red dye with a calibrated photometer when students are pumping reverse osmosis water through the photometer. What might be causing the high readings? Which of the following explanations is (are) consistent with the observed results?

     A) *An air bubble is caught in the optical path of the photometer*

     B) The photometer LED is turned off

     C) The photometer is installed backwards
