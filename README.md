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
```

=Q2.- Find the highest salary in every department?**

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
 ```

**3= ranking the scores by pandas=**
```
import pandas as pd

def order_scores(scores: pd.DataFrame) -> pd.DataFrame:
    scores['rank']= scores['score'].rank(method='dense', ascending=False)
    result_df=scores.drop('id',axis=1).sort_values(by='score',ascending=False)
    return result_df
```
4.= Delete Duplicate Emails
``` import pandas as pd

# Modify Person in place
def delete_duplicate_emails(person: pd.DataFrame) -> None:
    # Sort the rows based on id (Ascending order)
    person.sort_values(by='id',ascending=True,inplace=True)
    # Drop the duplicates based on email.
    person.drop_duplicates(subset='email', keep='first', inplace=True).
```

**What happens step by step
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

**#Yes, they are connected — because the second line runs on the result of the first line.
-But how they are connected depends on how you write them.**

**5. Find Total Time Spent by Each Employee**
``` import pandas as pd

def total_time(employees: pd.DataFrame) -> pd.DataFrame:
    employees['time_spent']=employees['out_time']- employees['in_time']
    result=employees.groupby(['emp_id', 'event_day'])['time_spent'].sum().reset_index()
    result.rename(columns={'event_day': 'day', 'time_spent':'total_time'},inplace=True)
    return result
```
**Step 1: employees.groupby(['emp_id', 'event_day'])**
What this does-

Splits rows into groups based on emp_id + event_day

**Each unique combination forms one group**

**Groups formed internally**

(emp_id=1, day=2024-01-01) → [3, 5]

(emp_id=1, day=2024-01-02) → [4]

(emp_id=2, day=2024-01-01) → [6, 2]

**⚠️ Nothing is calculated yet — just grouping. 
Step 2: ['time_spent']
.groupby(... )['time_spent']**


**This means:**

**“From each group, only look at the time_spent column.”**

Now each group holds only numbers:

(1, 2024-01-01) → [3, 5]

(1, 2024-01-02) → [4]

(2, 2024-01-01) → [6, 2]

**Step 3: .sum()**

.sum()


Now Pandas actually computes something.

It sums time_spent within each group.

**6. . Game Play Analysis I**- **find the earliest event date for every player**
```
import pandas as pd

def game_analysis(activity: pd.DataFrame) -> pd.DataFrame:
    # Sort the DataFrame by player_id and event_date
    activity = activity.sort_values(by=['player_id', 'event_date'])
    
    # Group by player_id and select the minimum event_date for each player
    result = activity.groupby('player_id')['event_date'].min().reset_index()
    result.rename(columns={'event_date': 'first_login'}, inplace=True)
    
    return result
```

**Step 1: activity.groupby('player_id')**
What this does

**Splits rows into groups based on player_id**

One group per unique player

Internal groups:

player 1 → [2024-01-05, 2024-01-01]

player 2 → [2024-01-03, 2024-01-02]

player 3 → [2024-01-07]

⚠️ No calculation yet.

**Step 2: ['event_date']
.groupby('player_id')['event_date']**


**This means:**

“From each player group, only keep the event_date column.”

- So now groups look like:

player 1 → dates only

player 2 → dates only

player 3 → dates only

**- Step 3: .min()**

.min()


**Now Pandas calculates.

For each player group, it finds the earliest date.**

Results (still NOT a DataFrame yet):

player_id	min(event_date)
1	2024-01-01
2	2024-01-02
3	2024-01-07

Internally this is a Series with player_id as index.

**Step 4: .reset_index()**

.reset_index()

- This converts the index (player_id) back into a normal column.

- **Final result DataFrame:**

**player_id**	**event_date**
1	2024-01-01
2	2024-01-02
3	2024-01-07
Why this is done this way
What problem this solves

“For each player, find their first activity date.”

That’s it.

SQL equivalent (important)
SELECT player_id, MIN(event_date) AS event_date
FROM activity
GROUP BY player_id;


This Pandas line is exactly that.

**Key points people miss**=
- ❌= .groupby() does not calculate
- ❌= ['event_date'] does not calculate
- ✅ =.min() does calculate
- ✅ =.reset_index() makes it usable

 **Group Sold Products By The Date**-

 ```
import pandas as pd

def categorize_products(activities: pd.DataFrame) -> pd.DataFrame:
    df = (activities.drop_duplicates()
                    .groupby('sell_date')['product'].agg(
                        num_sold = 'count',
                        products = list)
                    .reset_index())

    df.products = df.products.apply(lambda x: ','.join(sorted(x)))

    return df
```
.agg(...) — THIS IS THE CORE
.agg(
    num_sold='count',
    products=list
)

**.agg()= runs each aggregation function once per group, and each result becomes one cell in the output row.**
**This means:**

“For each group, run multiple aggregation functions on the grouped values, and store the results in named columns.”

Let’s execute it group by group.

**Step 4: How Pandas executes .agg() internally**
🔹 For group 2024-01-01

**Grouped values:**

['Apple', 'Banana']


**Now Pandas applies each aggregation:**

1️⃣ num_sold = 'count'
count(['Apple', 'Banana']) → 2

2️⃣ products = list
list(['Apple', 'Banana']) → ['Apple', 'Banana']


**So Pandas produces one row**
sell_date = 2024-01-01
num_sold = 2
products = ['Apple', 'Banana']

**Replace Employee ID With The Unique Identifier**

```
import pandas as pd

def replace_employee_id(employees: pd.DataFrame, employee_uni: pd.DataFrame) -> pd.DataFrame:
    merged=employees.merge(employee_uni, on='id', how='left')
    result=merged[['unique_id','name']]
    return result
```
**merge(...)**
merged = employees.merge(employee_uni, on='id', how='left')

**What this means in plain English:**

**“Join employees with employee_uni using id,**
keep all rows from employees,
and bring matching data from employee_uni when available.”

Break it down word by word
employees.merge(employee_uni, ...)

Merge employee_uni into employees

on='id'

Match rows where employees.id == employee_uni.id

**how='left'**

LEFT JOIN

Keep all rows from the left table (employees)

If there’s no match → fill with NaN



```import pandas as pd
def find_managers(employee: pd.DataFrame) -> pd.DataFrame:
    reports = employee.groupby('managerId').size().reset_index(name='count')
    winners = reports[reports['count'] >= 5]
    result = winners.merge(employee, left_on='managerId', right_on='id')[['name']]
    return result

   `
    
