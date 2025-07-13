# DMSRDE-Project
PCS Vacuum Fractionation Modeling
This project is based on modeling the vacuum fractionation process of Polycarbosilane (PCS) to estimate and control two key material properties — softening point and molecular weight. These parameters are crucial for downstream applications like electrospinning and melt spinning, where maintaining a specific flow behavior and thermal stability is essential.

What the Variables Mean
The dataset includes five input features (x1 to x5) and two output variables (y1, y2). Here's a brief description of each:

x1: Amount of PCS sample taken (in mg)

x2: Dwell time — how long the sample is held at the set vacuum temperature (in minutes)

x3: Vacuum temperature — the temperature at which the fractionation is carried out (in °C)

x4: Pressure applied (in mTorr or similar unit)

x5: Fraction yield or some experimental batch identifier (depending on context)

y1: Softening point of the resulting PCS fraction (in °C)

y2: Molecular weight of the resulting PCS fraction (in amu)

These inputs are experimentally controlled process parameters, and the outputs are measured physical properties used to determine if the PCS sample is usable for downstream processes.

What I Did
🔹 Data Understanding and Preprocessing
I started with a dataset containing several experimental runs. Each row represents a unique combination of input parameters (x1 to x5) and the resulting properties (y1, y2).

Before applying any models, I normalized the data using Min-Max scaling and Power Transformation (Yeo-Johnson) to improve model performance and ensure consistent feature ranges.

🔹 Multi-Linear Regression
My first approach was to build a multi-output linear regression model to predict both y1 and y2 from the input features.

Although the model gave quick results, it couldn’t capture the non-linear patterns present in the data, which reflected in lower R² scores, especially on the test set.

🔹 Polynomial Regression
To account for non-linearity, I used polynomial regression (degree = 2) and included squared terms.

Based on correlation analysis, I chose to focus on features like x3 (vacuum temperature) and x2 (dwell time), which had the highest correlation with the outputs.

I created simplified polynomial equations:

y1 as a function of x3 and x3²

y2 as a function of x2, x2², x3, and x3²

This model improved the accuracy, but still had some overfitting when evaluated on test data.

🔹 Random Forest-Inspired Modeling
I took a different approach here: instead of predicting outputs from inputs, I reversed the problem — predicting x2 and x3 from given y1 and y2. This would help in deciding processing conditions based on desired material behavior.

To simulate Random Forest logic, I implemented:

Row sampling (bootstrap): randomly selecting 80% of the data with replacement.

Column sampling: training each tree on a random subset of features.

I trained multiple DecisionTreeClassifier models using this strategy and then averaged their predictions to improve generalization.

Finally, I calculated the R² scores by comparing the predicted values with the actual x2 and x3 values across all samples.

Summary
Through this project, I explored and compared multiple regression techniques — starting from linear models and moving towards polynomial and ensemble-based methods. I also experimented with inverse modeling to predict processing parameters based on target material properties. This helped me understand the trade-offs between model complexity, interpretability, and performance.
