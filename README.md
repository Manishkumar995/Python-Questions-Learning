# Python-Questions-Learning

1. Nth Highest salary= When asked to find the N-th highest salary, the key challenge is that salaries may have duplicates, so we can't simply pick the N-th row from a sorted list.

Understanding the Problem:
-If there are multiple employees with the same salary, we should consider unique salaries when ranking.

-If N is larger than the number of unique salaries, we should return null because there isn't an N-th highest salary.

-If N is zero or negative, it's an invalid input, and we should also return null.

``` python
import pandas as pd

def nth_highest_salary(employee: pd.DataFrame, N: int) -> pd.DataFrame:
    unique_salaries = employee['salary'].drop_duplicates().sort_values(ascending=False)

    # If N is invalid (negative or zero), return "null" as a string
    if N <= 0:
        return pd.DataFrame({f'getNthHighestSalary({N})': [None]})

    # If N is larger than the number of unique salaries, return None
    if len(unique_salaries) < N:
        return pd.DataFrame({f'getNthHighestSalary({N})': [None]})

    # Get the N-th highest salary
    nth_salary = unique_salaries.iloc[N-1]

2.- Find the highest salary in every department?

```python
import pandas as pd

def department_highest_salary(employee: pd.DataFrame, department: pd.DataFrame) -> pd.DataFrame:
    # Merging the employee and department DataFrames based on departmentId
    merged = employee.merge(department, left_on='departmentId', right_on='id', how='left')

    # Filtering employees whose salary equals the highest salary in their department
    highest_salary = merged.loc[merged.groupby('departmentId')['salary'].transform('max') == merged['salary']]
    
    # Renaming the columns to match the output format
    result = highest_salary[['name_x', 'salary', 'name_y']].rename(columns={ 
        'name_y': 'Department',  # department name column
        'name_x': 'Employee',    # employee name column
        'salary': 'Salary'       # salary column
    })
        
    # Returning the final result in the desired order
    return result[['Department', 'Employee', 'Salary']]


   `
    
