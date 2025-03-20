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
