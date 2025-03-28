## Internal Working

### 1. Data Flow
The system begins by loading data from a specified CSV file or utilizing built-in sample data if no file is provided. The data consists of various factors related to students, such as demographic information, academic performance, and mental health indicators.

### 2. Data Preprocessing
Once the data is loaded, it undergoes preprocessing to prepare it for analysis:
- **Missing Values**: The system checks for and handles any missing values.
- **Categorical Encoding**: Categorical variables (e.g., Gender, Sleep Duration) are converted into numerical representations to facilitate analysis.
- **Discretization**: Continuous variables (e.g., Age, CGPA) are categorized into discrete groups for better modeling.
- **Risk Indicators**: New features are created to capture combined risk factors, enhancing the model's predictive power.

### 3. Bayesian Network Construction
The heart of the system is the Bayesian Network, which is constructed based on domain knowledge:
- **Model Structure**: The relationships between variables are defined using directed edges, indicating how one variable influences another.
- **Parameter Learning**: The model is fitted to the preprocessed data using the Bayesian Estimator with Dirichlet priors, allowing it to learn the conditional probability distributions of the variables.

### 4. User Input Collection
The system prompts the user to provide personal information through a series of questions. This input is processed and transformed into a format compatible with the Bayesian Network, creating an evidence dictionary that represents the user's current state.

### 5. Inference Process
Using the Variable Elimination algorithm, the system performs inference on the Bayesian Network:
- **Querying the Model**: The model is queried with the user's evidence to compute the probability of depression.
- **Visualizing Inference Steps**: If requested, the system visualizes the inference pathway, highlighting how different factors contribute to the final prediction.

### 6. Risk Prediction
The output of the inference process is a probability score indicating the likelihood of depression. This score is adjusted based on high-risk and protective factors identified in the user's input, enhancing the model's accuracy.

### 7. Explanation of Risk Factors
The system provides detailed explanations of the factors contributing to the user's depression risk:
- **Weighting Factors**: Each factor is assigned a weight based on its influence on the risk of depression, allowing for a nuanced understanding of the user's situation.
- **Visualization**: The system can visualize the importance of different factors, helping users understand their risk profile better.

### 8. Recommendations
Based on the identified risk factors, the system generates personalized recommendations aimed at mitigating the user's risk of depression. These recommendations are tailored to the user's specific circumstances and include suggestions for lifestyle changes, academic support, and mental health resources.

### 9. Saving Results
At the end of the assessment, users have the option to save their results, including risk assessment, contributing factors, and recommendations, to a text file for future reference.












-------------------------------------------------------------------------------------------------------------------------------




### How the Bayesian Network is Constructed

The Bayesian Network is a powerful tool that helps us understand the relationships between different factors that influence depression risk in students. Here’s a more in-depth look at how it is built:

#### 1. Model Structure
The Bayesian Network is represented as a directed acyclic graph (DAG), which consists of:
- **Nodes**: Each node represents a random variable. In our case, these variables include factors like:
  - **Demographic Factors**: Gender, Age Group
  - **Academic Factors**: Academic Pressure, CGPA Group, Study Satisfaction
  - **Mental Health Factors**: Suicidal Thoughts, Family History of Mental Illness, Depression
- **Edges**: The edges (arrows) between nodes represent the relationships and dependencies among these variables. For example:
  - An arrow from "Academic Pressure" to "Depression" indicates that higher academic pressure can lead to a higher likelihood of depression.

#### 2. Defining Relationships
The relationships between variables are defined based on:
- **Domain Knowledge**: Insights from psychology and academic research guide the connections. For instance, it is well-known that high academic pressure can negatively impact mental health.
- **Conditional Dependencies**: Each variable's probability is influenced by its parent nodes. For example, the probability of experiencing depression depends on factors like academic pressure and sleep duration.

#### 3. Parameter Learning
Once the structure is established, the next step is to learn the parameters of the model:
- **Data Fitting**: The model is trained using historical data that includes various factors affecting students' mental health. This data helps the model understand how likely each outcome is based on the input factors.
- **Probability Distributions**: For each node, we estimate the conditional probability distribution given its parents. For example, we determine the probability of having depression based on different levels of academic pressure and sleep duration.
- **Bayesian Estimator**: We use the Bayesian Estimator from the `pgmpy` library, which allows us to incorporate prior knowledge and improve the accuracy of our probability estimates. This is especially useful when we have limited data.

#### 4. Using Dirichlet Priors
- **Dirichlet Priors**: These are used to provide a way to express our beliefs about the probabilities before seeing the data. They help stabilize the estimates, especially for categories with few observations.
- **Pseudo Counts**: By adding pseudo counts (e.g., setting them to 10), we ensure that the model is confident in its predictions, even when some categories have limited data.

#### 5. Model Validation
After constructing the Bayesian Network, we need to validate it:
- **Testing**: We assess the model's performance using techniques like cross-validation, where we test the model on unseen data to ensure it makes accurate predictions.
- **Sensitivity Analysis**: This helps us understand how changes in input data affect the predictions, ensuring that the model is robust and reliable.

#### 6. Inference
Once the Bayesian Network is constructed and validated, it can be used for inference:
- **Variable Elimination**: This algorithm allows us to compute the probabilities of interest, such as the likelihood of depression given the user's input. It efficiently calculates the marginal probabilities by eliminating variables that are not of interest.

This structured approach to building the Bayesian Network ensures that the system can effectively analyze the complex interplay of factors contributing to depression risk among students, providing meaningful insights and predictions based on user input.












-------------------------------------------------------------------------------------------------------------------------------

# Student Depression Risk Assessment System

## Overview
The Student Depression Risk Assessment System evaluates the risk of depression among students based on various personal and academic factors using a Bayesian Network.

## Code Breakdown

### Step 1: Load Data (`load_data`)
- The function attempts to load data from a CSV file provided by the user.
- If loading fails, it falls back to a sample dataset embedded in the script.
- The data consists of multiple features, including:
  - Gender
  - Age
  - CGPA (Cumulative Grade Point Average)
  - Financial Stress
  - Depression

### Step 2: Data Preprocessing (`preprocess_data`)
- Drops unnecessary columns, such as `id`.
- Encodes categorical data into numerical values:
  - **Gender**: Male (0), Female (1)
  - **Sleep Duration**: Categorical (e.g., "Less than 5 hours" → 0, "5-6 hours" → 1, etc.)
  - **Suicidal Thoughts**: Yes (1), No (0)
- Discretizes continuous variables for the Bayesian Network:
  - **Age**: 3 categories (17-22, 23-28, 29-35)
  - **CGPA**: 3 categories (Low, Medium, High)
  - **Work Hours**: 4 categories (0-3, 4-6, 7-9, 10-12)
- Creates combined risk indicators:
  - **Sleep_Academic_Risk**: Indicates if there is high academic pressure and poor sleep.
  - **Academic_Performance_Risk**: Indicates if there is low CGPA and high academic pressure.
  - **Work_Sleep_Risk**: Indicates if there are high work hours and poor sleep.
- Converts all columns to integer type.

### Step 3: Build Bayesian Network (`build_bayesian_network`)
- Defines the structure of the Bayesian Network by specifying relationships (edges) between variables:
  - Academic Pressure → CGPA
  - Sleep Duration → Depression
  - Financial Stress → Depression
  - Suicidal Thoughts → Depression
- Fits the model using a Bayesian Estimator with Dirichlet priors and pseudo counts set to 10.
  - **Dirichlet Prior**: Stabilizes probability estimates when data is limited.
  - **Pseudo Counts**: Ensures that rarely occurring events still have a non-zero probability.

### Step 4: Visualize Bayesian Network (`visualize_network`)
- Uses `networkx` to draw the Bayesian Network graph.
- Highlights specific nodes and edges if needed.

### Step 5: Get User Input (`get_user_input`)
- Collects personal information from the user, including:
  - Gender
  - Age
  - Academic Pressure (1-5)
  - CGPA
  - Sleep Duration
  - Suicidal Thoughts (Yes/No)
  - Work Hours
  - Financial Stress (1-5)
  - Family History of Mental Illness
- Converts user input into a structured format for Bayesian inference.

### Step 6: Predict Depression Risk (`predict_depression_risk`)
- Uses Variable Elimination to query the Bayesian Network for the probability of depression.
- If the probability is close to 50%, it applies adjustments:
  - High-risk factors push the probability up.
  - Protective factors push the probability down.
- Returns the final probability of depression.

### Step 7: Explain Risk Factors (`explain_risk_factors`)
- Assigns weights to different factors affecting depression risk.
- Identifies the top factors contributing to the risk.
- Visualizes the impact of different factors on risk.

### Step 8: Provide Recommendations (`provide_recommendations`)
- Suggests personalized actions based on identified risk factors:
  - If suicidal thoughts are present: Urgent mental health support.
  - If sleep is poor: Improve sleep hygiene.
  - If academic pressure is high: Use stress management strategies.
  - If financial stress is high: Seek financial aid options.
  - If work hours are excessive: Reduce workload for better balance.

### Step 9: Save Results (`main`)
- Runs the entire program, which includes:
  - Loading data
  - Preprocessing it
  - Building the Bayesian Network
  - Asking for user input
  - Predicting depression risk
  - Providing recommendations
  - Offering to save results to a file.

## Conclusion
This system provides a comprehensive approach to assessing and understanding depression risk among students, utilizing advanced statistical methods and user-friendly interactions.
