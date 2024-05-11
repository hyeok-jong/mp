# mp


```
import multiprocessing
import os
from tqdm import tqdm


def f1(x):
    return 1

def f2(x):
    return 2

def worker(func_arg_tuple):
    func, args = func_arg_tuple
    return func(*args)

def run_functions(functions, kwargs_list, num_cores):
    # Ensure that each function in the list is paired with its corresponding kwargs.
    tasks = zip(functions, kwargs_list)
    with multiprocessing.Pool(processes=num_cores) as pool:
        results = list(tqdm(pool.starmap(worker, tasks), total=len(functions)))
    return results

# Define functions
functions = [f1, f2]

# Define keyword arguments for each function call
kwargs_list = [
    {'x': 10, 'name': 'f1'},
    {'x': 20, 'name': 'f2'}
]

# Specify the number of cores to use (e.g., 2 cores)
num_cores = 2
results = run_functions(functions, kwargs_list, num_cores)


```
