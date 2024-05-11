```

def continuous_all(source_data, variable, lag, y_name, return_prediction):
    results, output = continuous_conditional_logistic(source_data, variable = variable, lag = lag, y_name = y_name, return_prediction = True)
    results = continuous_conditional_logistic(output, variable = 'prediction', lag = lag, y_name = y_name, return_prediction = False)
    return results

def categorical_all(source_data, variable, lag, y_name, return_prediction):
    results, output = continuous_conditional_logistic(source_data, variable = variable, lag = lag, y_name = y_name, return_prediction = True)
    results = categorical_conditional_logistic(output, variable = 'prediction', lag = lag, y_name = y_name, return_prediction = False)
    return results

def dummy_all(source_data, variable, lag, y_name, return_prediction):
    results, output = continuous_conditional_logistic(source_data, variable = variable, lag = lag, y_name = y_name, return_prediction = True)
    results = dummy_conditional_logistic(output, variable = 'prediction', lag = lag, y_name = y_name, return_prediction = False)
    return results


def worker(args):
    # Unpack arguments inside the worker
    func, kwargs, name = args
    result = func(**kwargs)
    return name, result

def run_functions(tasks, num_cores):
    with multiprocessing.Pool(processes=num_cores) as pool:
        # Pass the whole task tuple to the worker function
        results = list(tqdm(pool.imap_unordered(worker, tasks), total=len(tasks)))
    # Creating dictionary from results list
    results_dict = {name: result for name, result in results}
    return results_dict

functions = [continuous_conditional_logistic]*2 + [continuous_all] + [categorical_conditional_logistic]*2 + [categorical_all] + [dummy_conditional_logistic]*2 + [dummy_all] 
functions *= 4

lag0 = [
    {'source_data': source_data, 'variable': 'PM25', 'lag': 0, 'y_name': 'CVD_true', 'return_prediction': False},
    {'source_data': source_data, 'variable': 'PM10', 'lag': 0, 'y_name': 'CVD_true', 'return_prediction': False},
    {'source_data': source_data, 'variable': 'ALL', 'lag': 0, 'y_name': 'CVD_true', 'return_prediction': True},
]*3
lag1 = [
    {'source_data': source_data, 'variable': 'PM25', 'lag': 1, 'y_name': 'CVD_true', 'return_prediction': False},
    {'source_data': source_data, 'variable': 'PM10', 'lag': 1, 'y_name': 'CVD_true', 'return_prediction': False},
    {'source_data': source_data, 'variable': 'ALL', 'lag': 1, 'y_name': 'CVD_true', 'return_prediction': True},
]*3
lag2 = [
    {'source_data': source_data, 'variable': 'PM25', 'lag': 2, 'y_name': 'CVD_true', 'return_prediction': False},
    {'source_data': source_data, 'variable': 'PM10', 'lag': 2, 'y_name': 'CVD_true', 'return_prediction': False},
    {'source_data': source_data, 'variable': 'ALL', 'lag': 2, 'y_name': 'CVD_true', 'return_prediction': True},
]*3
lag3 = [
    {'source_data': source_data, 'variable': 'PM25', 'lag': 3, 'y_name': 'CVD_true', 'return_prediction': False},
    {'source_data': source_data, 'variable': 'PM10', 'lag': 3, 'y_name': 'CVD_true', 'return_prediction': False},
    {'source_data': source_data, 'variable': 'ALL', 'lag': 3, 'y_name': 'CVD_true', 'return_prediction': True},
]*3

kwargs_list = lag0+lag1+lag2+lag3


ns = ['con25', 'con10', 'conall', 'cat25', 'cat10', 'catall', 'dum25', 'dum10', 'dumall', ]  

names = list()
for i in range(4):
    for n in ns:
        names.append(f'{n}_{i}')

tasks = list(zip(functions, kwargs_list, names))

num_cores = 32
results = run_functions(tasks, num_cores)


```
