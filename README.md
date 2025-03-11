# Healthcare Data Cleaning

## AI Mid-Semester Examination (MSE) Project

This repository contains my solution for the AI Mid-Semester Examination focused on healthcare data cleaning. The project demonstrates techniques for cleaning healthcare-related data by addressing common issues such as missing values, duplicates, outliers, and inconsistencies.

## Project Overview

Healthcare data often suffers from quality issues that can impact analysis and decision-making. This project implements a comprehensive data cleaning pipeline specifically designed for healthcare datasets, ensuring that the data is suitable for subsequent analysis or machine learning applications.

### Key Features

- **Missing Value Imputation**: Strategic handling of missing values based on data type and context
- **Duplicate Detection and Removal**: Identification and elimination of redundant records
- **Outlier Detection and Management**: Using IQR method to identify and handle statistical outliers
- **Data Standardization**: Ensuring consistency in categorical variables (e.g., gender, blood types)
- **Data Validation**: Logical checks to ensure data integrity (e.g., date sequences, value ranges)
- **Visualization**: Before/after comparisons to demonstrate cleaning effectiveness

## Repository Contents

- `Healthcare_Data_Cleaning.ipynb`: Google Colab notebook containing the complete data cleaning implementation
- `Healthcare_Data_Cleaning_Report.md`: Detailed project report explaining methodology and results
- `README.md`: This file, providing an overview of the project

## How to Use

1. **Setup**:
   - Clone this repository
   - Upload the `Healthcare_Data_Cleaning.ipynb` to Google Colab

2. **Execute the Notebook**:
   - Run the notebook in Google Colab
   - When prompted, upload your `healthcare_data.csv` file
   - The notebook will process the data and generate a cleaned version

3. **Results**:
   - The cleaned dataset will be automatically downloaded as `healthcare_data_cleaned.csv`
   - Review the visualizations and summary statistics to understand the changes made

## Technologies Used

- Python 3.x
- pandas for data manipulation
- NumPy for numerical operations
- Matplotlib and Seaborn for visualization
- Google Colab for execution environment

## Submission Information

- **Name**: [My Name]
- **Roll No**: [My Roll No]
- **Subject**: [Subject Name]
- **GitHub Repository**: [https://github.com/soumomo/-Healthcare-Data-Cleaning](https://github.com/soumomo/-Healthcare-Data-Cleaning)

## References

- Pandas documentation: https://pandas.pydata.org/docs/
- NumPy documentation: https://numpy.org/doc/
- Matplotlib documentation: https://matplotlib.org/stable/contents.html
- Seaborn documentation: https://seaborn.pydata.org/