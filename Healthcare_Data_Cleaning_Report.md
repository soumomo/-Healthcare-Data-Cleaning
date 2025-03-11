# Healthcare Data Cleaning

## AI Mid-Semester Examination (MSE)

**Name:** [My Name]  
**Roll No:** [My Roll No]  
**Course:** [Course Name]  
**Semester:** [Semester]  
**Date of Submission:** [Date]  

---

## Table of Contents
1. [Introduction](#introduction)
2. [Methodology](#methodology)
3. [Code Implementation](#code-implementation)
4. [Results](#results)
5. [References](#references)

---

## Introduction

### Problem Statement
Healthcare data often suffers from numerous quality issues that can negatively impact analysis, decision-making processes, and ultimately patient care. This project focuses on cleaning a healthcare dataset by identifying and addressing common data quality issues such as missing values, duplicates, outliers, and inconsistencies. High-quality, properly formatted healthcare data is essential for extracting accurate insights and making informed medical decisions.

### Importance in Medical Domain
In the healthcare industry, data quality is paramount for several reasons:

- **Patient Safety**: Incorrect or missing data can lead to improper treatments and endanger patient lives
- **Research Integrity**: Medical research findings are only as reliable as the data they're based upon
- **Operational Efficiency**: Clean data reduces administrative overhead and improves operational workflows
- **Regulatory Compliance**: Healthcare organizations must maintain accurate records to comply with legal requirements
- **AI/ML Applications**: The growing use of predictive analytics in healthcare requires high-quality data

### Dataset Description
The dataset (`healthcare_data.csv`) contains patient information and medical records typically found in healthcare systems. While the specific columns may vary, common fields likely include:

- **Patient Demographics**: ID, name, age, gender
- **Medical Information**: Diagnoses, medications, blood type
- **Temporal Data**: Admission date, discharge date
- **Measurements**: Vital signs (blood pressure, heart rate, etc.)
- **Administrative Data**: Doctor assignments, department codes

![Sample Healthcare Data Table](https://i.imgur.com/sample_image.png)
*Figure 1: Example visualization of typical healthcare dataset structure (Note: This is a placeholder for an actual image)*

### Common Data Quality Issues
Healthcare datasets typically suffer from several data quality problems:

1. **Missing Values**: Often due to incomplete patient records, data entry errors, or equipment malfunctions
2. **Duplicate Records**: Common when patients register multiple times or records are mistakenly entered twice
3. **Inconsistent Formats**: Particularly in text fields like gender ("M", "Male", "male") or blood types
4. **Outliers**: Abnormal values that may represent measurement errors or truly exceptional cases
5. **Data Type Mismatches**: Dates stored as strings, numeric values containing text, etc.

Addressing these issues is crucial for ensuring data reliability for downstream analysis.

---

## Methodology

The data cleaning process followed a systematic approach involving multiple stages, from initial assessment to final validation. Each step was designed to address specific data quality issues while preserving the integrity of the original information.

### Data Cleaning Framework

![Data Cleaning Framework](https://i.imgur.com/data_cleaning_framework.png)
*Figure 2: The systematic data cleaning framework implemented in this project (Note: This is a placeholder for an actual image)*

#### 1. Data Quality Assessment
Before applying any transformations, a thorough assessment was conducted to:
- Identify missing values and their patterns
- Detect duplicate records
- Examine data distributions for potential outliers
- Check for inconsistencies in categorical data
- Verify data types against expected formats

This assessment provided a roadmap for the subsequent cleaning operations.

#### 2. Handling Missing Values
Missing values were addressed using various strategies based on the nature of the data:

- **Numerical Columns**: Median imputation was used to maintain the central tendency without being affected by outliers
- **Categorical Columns**: Mode imputation (most frequent value) was employed
- **Columns with Excessive Missing Data**: If more than 30% of values were missing, the column was evaluated for potential removal
- **Optional Fields**: For fields likely to be optional (e.g., notes), missing values were replaced with "Not Provided"

This approach balanced data preservation with statistical soundness.

#### 3. Duplicate Removal
Exact duplicate rows were identified and removed, keeping only the first occurrence. This step ensured that:
- Each patient record was represented only once
- Statistical analyses wouldn't be skewed by repeated data points
- Data integrity was maintained across the dataset

#### 4. Outlier Management
The Interquartile Range (IQR) method was used to detect and handle outliers in numerical columns:
- Calculate Q1 (25th percentile) and Q3 (75th percentile)
- Compute IQR = Q3 - Q1
- Define boundaries: Lower bound = Q1 - 1.5×IQR, Upper bound = Q3 + 1.5×IQR
- Values outside these boundaries were capped rather than removed to preserve data points

This method is robust to skewed distributions and preserves more data than removal techniques.

#### 5. Standardizing Inconsistencies
Categorical variables were standardized to ensure consistent formats:
- **Gender**: Standardized to "Male" and "Female" from various formats (e.g., "M", "m", "male")
- **Blood Types**: Reformatted to consistent notation (e.g., "A+", "B-")
- **Names**: Converted to title case with consistent spacing
- **Text Fields**: Trimmed whitespace and standardized capitalization

#### 6. Data Type Conversion
Columns were converted to appropriate data types:
- Date fields were parsed into datetime objects
- Identification numbers were converted to string format (to preserve leading zeros)
- Numeric fields were verified to contain only numerical data

#### 7. Additional Validation Checks
Final validation ensured logical consistency in the data:
- Age values were verified to be within reasonable ranges (0-120 years)
- Date sequences were checked for logical order (e.g., admission date before discharge date)
- Categorical values were verified against valid domain values

### Libraries Used
The data cleaning process utilized several Python libraries:
- **pandas**: For data manipulation and transformation
- **numpy**: For numerical operations
- **matplotlib** and **seaborn**: For data visualization
- **scipy**: For statistical functions (e.g., z-score calculation)

These libraries were selected for their robust functionality, widespread usage in data science, and compatibility with the Google Colab environment.

---

## Code Implementation

The data cleaning process was implemented using Python in Google Colab. The code was structured to provide a clear, step-by-step approach to cleaning the healthcare dataset.

### Environment Setup
```python
# Import necessary libraries
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
from google.colab import files
import io
from scipy import stats

# Enable all warnings
import warnings
warnings.filterwarnings('always')

# Set plot styling
plt.style.use('ggplot')
sns.set(style="whitegrid")
```

### Data Loading and Initial Exploration
```python
# Upload the file from user's computer
uploaded = files.upload()

# Read the uploaded file
file_name = next(iter(uploaded))
healthcare_df = pd.read_csv(io.BytesIO(uploaded[file_name]))

# Display basic information about the dataset
print(f"Dataset shape: {healthcare_df.shape}")
display(healthcare_df.head())
display(healthcare_df.info())
display(healthcare_df.describe(include='all').T)
```

### Missing Value Handling
```python
def handle_missing_values(df):
    """
    Handle missing values based on column data type and nature
    - Numerical: Impute with median
    - Categorical: Impute with mode
    - Date: Impute with median date
    """
    df_cleaned = df.copy()
    
    # For each column with missing values
    for col in df.columns:
        missing_count = df[col].isnull().sum()
        
        if missing_count > 0:
            print(f"Handling missing values in '{col}'")
            
            # For numerical columns
            if pd.api.types.is_numeric_dtype(df[col]):
                # If missing values are less than 30% of the data, impute with median
                if missing_count < 0.3 * len(df):
                    median_value = df[col].median()
                    df_cleaned[col].fillna(median_value, inplace=True)
                    print(f"  - Imputed {missing_count} missing values with median: {median_value}")
                else:
                    # If too many missing values, drop the column
                    df_cleaned.drop(col, axis=1, inplace=