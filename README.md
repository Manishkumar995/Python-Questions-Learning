# Python-Questions-Learning

Nth Highest salary
' ' ' python
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
