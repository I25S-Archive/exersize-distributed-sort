## Simple distribute sort algorithm



Test assignment for a core applicate.

The task is to write a program in D which can sort a list of numbers using via communication between tasks/threads.

The program should be implemented as command line only program and it should only use the D standard library (phobos and druntime) and it should be able to be compiled with the **dmd** or the **ldc** compiler.

The program should be able to be executed in a flowing manner.

```bash
./dsort input.json output.json -n 7
```

The program should take 3 arguments an input and output json file and a parameter n which is the number of tasks care of the sorting.

The main task M should start the n tasks (use the std.concurrency) and wait for the task to be ready (Using  concurrency send/read to synchronize the task).

All tasks must be connected as show in the figure the arrows shows the message connections between the tasks. 

[Task commications figure](sort_tasks.svg)

Hint: The functions locate and register (in std.concurrency) can be use to set the name of tasks and to locate the task id 'Tid'.

Hint: When a task is spawned from the main task M. The task function can be called with a immutable parameter.

When all n tasks has been started the M task should send the numbers in the list one by one to each tasks randomly.

The sorting tasks are only allowed to communicate with previous task and the next tasks as show in with the arrows in the figure.

The numbers in the list should be equally distributed between the sorting tasks. This means each sorting task should hold K or K+1 numbers.

Hint: A sorting task could keep track of how many numbers there the previous task and the next task.

When a tasks finished it should write a json file with numbers contained in the task the files should be called output_#.json where # is the task number.

The sorted numbers hold by the task should be send back to the main task M when the sorted task ends.

The main task should write output.json file with all the numbers in order.

Hint: If the main task M send a STOP signal to each sorting task from 0 to n-1 and between each STOP it wait for the results from the sorting task to send and END signal back, this makes it simple for the main task to collect the result from all sorting tasks in order.

**Example**

```bash
./dsort input.json output.json -n 7
```

input.json

```json
[51, 99, 19, 80, 2, 4, 8, 0, 60, 23, 38, 29, 85, 58, 61, 60, 14, 77, 83, 4]
```
output.json
```json
[0, 2, 4, 4, 8, 14, 19, 23, 29, 38, 51, 58, 60, 60, 61, 77, 80, 83, 85, 99]
```
output_0.json
```json
[0, 2, 4]
```
output_1.json
```json
[4, 8, 14]
```
output_2.json
```json
[19, 23, 29]
```
output_3.json
```json
[38, 51, 58]
```
output_4.json
```json
[60, 60, 61]
```
output_5.json
```json
[77, 80, 83]
```
output_6.json
```json
[85, 99]
```

Recommend to use the D modules std.getopt, std.json, std.stdio, std.path, std.concurrency, std.random and other D standard libraries.

Note. 

A D program can compile/linked like.

```bash
dmd dsort.d
```



