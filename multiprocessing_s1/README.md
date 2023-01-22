# The first part of the research on multiprocessing

## Some theory

### What is multiprocessing?

Multiprocessing refers to the ability of a system to support more than one processor at the same time. Applications in a multiprocessing system are broken to smaller routines that run independently. The operating system allocates these threads to the processors improving performance of the system.

### Why multiprocessing?

Consider a computer system with a single processor. If it is assigned several processes at the same time, it will have to interrupt each task and switch briefly to another, to keep all of the processes going.
This situation is just like a chef working in a kitchen alone. He has to do several tasks like baking, stirring, kneading dough, etc.

So the gist is that: The more tasks you must do at once, the more difficult it gets to keep track of them all, and keeping the timing right becomes more of a challenge.
This is where the concept of multiprocessing arises!

**A multiprocessing system can have**:

- multiprocessor, i.e. a computer with more than one central processor.
- multi-core processor, i.e. a single computing component with two or more independent actual processing units (called “cores”).
  Here, the CPU can easily executes several tasks at once, with each task using its own processor.

It is just like the chef in last situation being assisted by his assistants. Now, they can divide the tasks among themselves and chef doesn’t need to switch between his tasks.

## Some results

### multiprocessing_s1_e1.py

As a result of the execution of the file [multiprocessing_s1_e1.py](multiprocessing_s1_e1.py) we will get:

```
Cube of 10: 1000
Square of 10: 100
Done!
```

Let us try to understand the above code:

- To import the multiprocessing module, we do:

  ```
  import multiprocessing
  ```

- To create a process, we create an object of **Process** class. It takes following arguments:

  - **target**: the function to be executed by process
  - **args**: the arguments to be passed to the target function

  ```
  p1 = multiprocessing.Process(target=print_square, args=(10, ))
  p2 = multiprocessing.Process(target=print_cube, args=(10, ))
  ```

- To start a process, we use _start_ method of _Process_ class.

  ```
  p1.start()
  p2.start()
  ```

- Once the processes start, the current program also keeps on executing. In order to stop execution of current program until a process is complete, we use **join** method.

  ```
  p1.join()
  p2.join()
  ```

As a result, the current program will first wait for the completion of p1 and then p2. Once, they are completed, the next statements of current program are executed.

### multiprocessing_s1_e2.py

Let's consider another program to understand the concept of different processes running on same python script. In example [multiprocessing_s1_e2.py](multiprocessing_s1_e2.py), we print the ID of the processes running the target functions:

```
ID of main process: 73414
ID of process p1: 73416
ID of process p2: 73417
ID of process running worker1: 73416
ID of process running worker2: 73417
Both processes finished execution!
Process p1 is alive: False
Process p2 is alive: False
```

- The main python script has a different process ID and multiprocessing module spawns new processes with different process IDs as we create **Process** objects **p1** and **p2**. In above program, we use **os.getpid()** function to get ID of process running the current target function.

  Notice that it matches with the process IDs of **p1** and **p2** which we obtain using **pid** attribute of **Process** class.

- Each process runs independently and has its own memory space.
- As soon as the execution of target function is finished, the processes get terminated. In above program we used **is_alive** method of **Process** class to check if a process is still active or not.
