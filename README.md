# ECE 2112 - Experiment 4: Data Wrangling and Data Visualization

**Name:** Bacual, John Matthew P.

**Section:** 2ECE-A

**Date Submitted:** September 18, 2026

The content of this repository contains *Programming Assignment 4* for **ECE 2112: Advanced Computer Programming and Algorithms for S.Y. 2026–2027**. This project covers three Python data wrangling and data visualization problems under Experiment 4: Data Wrangling and Data Visualization.

## Objective

The objective of this experiment is to practice working with tabular data using Pandas and to present comparisons in the data using simple visualizations.

Specifically, the experiment aims to:

1. Filter tabular data using categorical and numerical conditions.
2. Create focused DataFrames by selecting the required columns.
3. Find the mean of `Average` for different categories.
4. Present the comparison of the category means using bar charts.

## Detailed Discussion of the Experiment

### A. Visayas Communication DataFrame

The first problem focuses on finding students who are from the Visayas and whose track is Communication. The dataset is first loaded using Pandas and stored in the DataFrame `df`.

```python
import pandas as pd

df = pd.read_csv('board2.csv')
df['Average'] = df[['Math', 'Electronics', 'GEAS', 'Communication']].mean(axis=1)
```

The `Average` is obtained by getting the mean of the recorded scores in Math, Electronics, GEAS, and Communication for each student.

The filtering is then done by applying the two required conditions to the `Track` and `Hometown` columns.

```python
VisComm = df[(df['Track'] == 'Communication') & (df['Hometown'] == 'Visayas')]
VisComm = VisComm.loc[:, ['Name', 'Gender', 'Math', 'Electronics', 'Average']]

display(VisComm)
print("Number of rows:", len(VisComm))
```

The first condition checks if the student's track is Communication, while the second checks if the student's hometown is Visayas. Both conditions must be true for the student to be included.

After filtering the students, only the required columns are retained: `Name`, `Gender`, `Math`, `Electronics`, and `Average`.

**Results:**

```text
   Name  Gender  Math  Electronics  Average
10  S11  Female    48           56    54.75
11  S12    Male    89           67    76.00
17  S18    Male    81           40    63.50
21  S22  Female    64           39    62.50
27  S28    Male    85           53    67.75

Number of rows: 5
```

The result contains five students who meet both conditions. The table also contains only the columns required in the problem.

---

### B. Visayas Female DataFrame

The second problem focuses on students who are from the Visayas and are Female.

```python
VisFemale = df[(df['Hometown'] == 'Visayas') & (df['Gender'] == 'Female')]
VisFemale = VisFemale.loc[:, ['Name', 'Track', 'GEAS', 'Electronics', 'Average']]

display(VisFemale)
display(VisFemale[VisFemale['Average'] >= 60])
```

The first line filters the students based on their hometown and gender. The required columns are then selected: `Name`, `Track`, `GEAS`, `Electronics`, and `Average`.

The complete `VisFemale` DataFrame is displayed first. A second filter is then used to display only the students whose `Average` is at least 60.

**Results:**

```text
   Name             Track  GEAS  Electronics  Average
5    S6  Microelectronics    86           45    75.50
10  S11     Communication    48           56    54.75
21  S22     Communication    89           39    62.50
23  S24  Microelectronics    60           45    57.75
25  S26   Instrumentation    83           47    65.75
```

After applying the `Average >= 60` condition, the remaining students are:

```text
   Name             Track  GEAS  Electronics  Average
5    S6  Microelectronics    86           45    75.50
21  S22     Communication    89           39    62.50
25  S26   Instrumentation    83           47    65.75
```

The result shows the female students from the Visayas and then shows which of them have an `Average` of at least 60.

---

### C. Category-Average Visualization

The third problem focuses on comparing the recorded `Average` based on `Track`, `Gender`, and `Hometown`. The `groupby()` function is used to group the students according to each category, while `mean()` is used to get the average for each group.

```python
mean_track = df.groupby('Track')['Average'].mean()
mean_gender = df.groupby('Gender')['Average'].mean()
mean_hometown = df.groupby('Hometown')['Average'].mean()

display(mean_track)
print("\n")

display(mean_gender)
print("\n")

display(mean_hometown)
print("\n")
```

The three summary results are:

```text
Track
Communication       67.975
Instrumentation     65.225
Microelectronics    67.500
Name: Average, dtype: float64
```

```text
Gender
Female    66.616667
Male      67.183333
Name: Average, dtype: float64
```

```text
Hometown
Luzon       68.083333
Mindanao    66.678571
Visayas     65.750000
Name: Average, dtype: float64
```

The results are then presented using three bar charts in one figure.

```python
import matplotlib.pyplot as plt

fig, ax = plt.subplots(1, 3, figsize=(20, 4))

ax[0].bar(mean_track.index, mean_track.values)
ax[0].set_title("Mean Average by Track")
ax[0].set_xlabel("Track")
ax[0].set_ylabel("Mean Average")
ax[0].tick_params(axis='x', rotation=20)

ax[1].bar(mean_gender.index, mean_gender.values)
ax[1].set_title("Mean Average by Gender")
ax[1].set_xlabel("Gender")
ax[1].set_ylabel("Mean Average")

ax[2].bar(mean_hometown.index, mean_hometown.values)
ax[2].set_title("Mean Average by Hometown")
ax[2].set_xlabel("Hometown")
ax[2].set_ylabel("Mean Average")

plt.tight_layout()
plt.show()
```

The first graph compares the mean `Average` according to Track. The second compares the mean `Average` according to Gender, while the third compares the mean `Average` according to Hometown.

The category with the highest sample mean for each feature is identified using `idxmax()` and `max()`.

```python
top_track = mean_track.idxmax()
top_track_val = mean_track.max()

top_gender = mean_gender.idxmax()
top_gender_val = mean_gender.max()

top_hometown = mean_hometown.idxmax()
top_hometown_val = mean_hometown.max()

print(f"For the mean average by track, the highest sample mean was from "
      f"students whose track was {top_track}, with a mean average of {top_track_val:.2f}.")
print(f"For the mean average by gender, the highest sample mean was from "
      f"students whose gender was {top_gender}, with a mean average of {top_gender_val:.2f}.")
print(f"For the mean average by hometown, the highest sample mean was from "
      f"students whose hometown was {top_hometown}, with a mean average of {top_hometown_val:.2f}.")
```

**Results:**

```text
For the mean average by track, the highest sample mean was from students whose track was Communication, with a mean average of 67.98.
For the mean average by gender, the highest sample mean was from students whose gender was Male, with a mean average of 67.18.
For the mean average by hometown, the highest sample mean was from students whose hometown was Luzon, with a mean average of 68.08.
```

The results show that Communication had the highest sample mean among the tracks, Male had the highest sample mean between the two genders, and Luzon had the highest sample mean among the hometowns. These results only describe the recorded data and do not mean that Track, Gender, or Hometown causes a higher board-exam score.

---

**Thank you for reading!**

To see the main python program for Programming Assignment 4, click this link **https://github.com/johnmatthewbacual-ECE/ECE2112_PA4/blob/cde3abdd003565e1aecef7d66ebd2020dfe38972/Bacual_2ECE-A_PA4.ipynb** and download. Open on Jupyter Notebook, then run all cells.

README file Version History:

**September 18, 2026** - README output uploaded.
