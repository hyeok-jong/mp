```
import multiprocessing
import os
from tqdm import tqdm

def worker(func, kwargs, name):
    result = func(**kwargs)
    return name, result

def run_functions(tasks, num_cores):
    with multiprocessing.Pool(processes=num_cores) as pool:
        results = list(tqdm(pool.starmap(worker, tasks), total=len(tasks)))
    results_dict = {name: result for name, result in results}
    return results_dict

functions = [continuous_conditional_logistic]*3

kwargs_list = [
    {'source_data': source_data, 'variable': 'PM25', 'lag': 0, 'y_name': 'CVD_true', 'return_prediction': False},
    {'source_data': source_data, 'variable': 'PM25', 'lag': 0, 'y_name': 'CVD_true', 'return_prediction': False},
    {'source_data': source_data, 'variable': 'PM25', 'lag': 0, 'y_name': 'CVD_true', 'return_prediction': False},
]

names = ['Task 1', 'Task 2', 'Task 3']  

tasks = list(zip(functions, kwargs_list, names))

num_cores = 3
results = run_functions(tasks, num_cores)

print(results)
```
