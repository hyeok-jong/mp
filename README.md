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

def run_functions(functions, args):
    with multiprocessing.Pool(processes=num_cores) as pool:
        results = list(tqdm(pool.imap(worker, zip(functions, args)), total=len(functions)))
    return results

functions = [f1, f2]
args = [(10,), (20,)]
results = run_functions(functions, args)
results

```
