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

3= ranking the scores by pandas=
```python
import pandas as pd

def order_scores(scores: pd.DataFrame) -> pd.DataFrame:
    scores['rank']= scores['score'].rank(method='dense', ascending=False)
    result_df=scores.drop('id',axis=1).sort_values(by='score',ascending=False)
    return result_df
4.= Delete Duplicate Emails
import pandas as pd

# Modify Person in place
def delete_duplicate_emails(person: pd.DataFrame) -> None:
    # Sort the rows based on id (Ascending order)
    person.sort_values(by='id',ascending=True,inplace=True)
    # Drop the duplicates based on email.
    person.drop_duplicates(subset='email', keep='first', inplace=True).
**
** What happens step by step**
``python sort_values(...)
-Creates a new DataFrame
-Sorted by id descending
-Assigned back to person
``pythn= drop_duplicates(...)
-Runs on the sorted DataFrame
- Keeps the first email per group
- Modifies person in place
✔ Result:
For duplicate emails, the row with highest id is kept
One-line summary-
The lines are connected only if you assign the sorted DataFrame back to person or chain the methods; otherwise the first line does nothing.
#Yes, they are connected — because the second line runs on the result of the first line.
-But how they are connected depends on how you write them.


   `
    
