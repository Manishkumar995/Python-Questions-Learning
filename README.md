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


   `
    
