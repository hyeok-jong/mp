import multiprocessing
import os
from tqdm import tqdm

def worker(func, kwargs):
    return func(**kwargs)

def run_functions(functions, kwargs_list, num_cores):
    # Ensure that each function in the list is paired with its corresponding kwargs.
    tasks = zip(functions, kwargs_list)
    with multiprocessing.Pool(processes=num_cores) as pool:
        results = list(tqdm(pool.starmap(worker, tasks), total=len(functions)))
    return results

# Define functions
functions = [continuous_conditional_logistic]*2

# Define keyword arguments for each function call
kwargs_list = [
    {'source_data': source_data, 'variable': 'PM25', 'lag' : 0, 'y_name' : 'CVD_true'},
    {'source_data': source_data, 'variable': 'PM25', 'lag' : 0, 'y_name' : 'CVD_true'},

]

# Specify the number of cores to use (e.g., 2 cores)
num_cores = 2
results = run_functions(functions, kwargs_list, num_cores)

