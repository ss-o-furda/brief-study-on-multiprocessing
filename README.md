# A brief study on multiprocessing

Research is based on articles [**Multiprocessing in Python | Set 1 (Introduction)**](https://www.geeksforgeeks.org/multiprocessing-python-set-1/) and [**Multiprocessing in Python | Set 2 (Communication between processes)**](https://www.geeksforgeeks.org/multiprocessing-python-set-2/)

<br />

## The first part

The first part of the study can be seen in the [The first part of the research on multiprocessing](multiprocessing_s1/README.md).

This article is a brief yet concise introduction to multiprocessing in Python programming language.

**To run these files and verify the correctness of the programs yourself, you can use the following commands**:

### First program

on **macOS**/**linux**:

```
python3 multiprocessing_s1/multiprocessing_s1_e1.py
```

on **windows**:

```
python multiprocessing_s1/multiprocessing_s1_e1.py
```

### Second program

on **macOS**/**linux**:

```
python3 multiprocessing_s1/multiprocessing_s1_e2.py
```

on **windows**:

```
python multiprocessing_s1/multiprocessing_s1_e2.py
```

## The second part

The second part of the study can be seen in the [The second part of the research on multiprocessing](multiprocessing_s2/README.md).

These articles discusses the concept of data sharing and message passing between processes while using multiprocessing module in Python.

### First program

on **macOS**/**linux**:

```
python3 multiprocessing_s2/multiprocessing_s2_e1.py
```

on **windows**:

```
python multiprocessing_s2/multiprocessing_s2_e1.py
```

### Second program

on **macOS**/**linux**:

```
python3 multiprocessing_s2/multiprocessing_s2_e2.py
```

on **windows**:

```
python multiprocessing_s2/multiprocessing_s2_e2.py
```

### Third program

on **macOS**/**linux**:

```
python3 multiprocessing_s2/multiprocessing_s2_e3.py
```

on **windows**:

```
python multiprocessing_s2/multiprocessing_s2_e3.py
```

### Fourth program

on **macOS**/**linux**:

```
python3 multiprocessing_s2/multiprocessing_s2_e4.py
```

on **windows**:

```
python multiprocessing_s2/multiprocessing_s2_e4.py
```

### Fifth program

on **macOS**/**linux**:

```
python3 multiprocessing_s2/multiprocessing_s2_e5.py
```

on **windows**:

```
python multiprocessing_s2/multiprocessing_s2_e5.py
```

## Conclusions

**Finally, here are a few advantages and disadvantages of multiprocessing**:

**Advantages**:

- The biggest advantage of a multiprocessor system is that it helps you to get more work done in a shorter period.
- The code is usually straightforward.
- Takes advantage of multiple CPU & cores
- Helps you to avoid GIL limitations for CPython
- Remove synchronization primitives unless if you use shared memory.
- Child processes are mostly interruptible/killable
- It helps you to get work done in a shorter period.
- These types of systems should be used when very high speed is required to process a large volume of data.
- Multiprocessing systems save money compared to single processor systems as processors can share peripherals and power supplies.

**Disadvantages**:

- IPC(Inter-Process Communication) a quite complicated with more overhead
- Has a larger memory footprint

As you can see, this approach has many more pros than cons, but you still need to be careful because this approach, if used incorrectly, can cause a lot of problems and waste time to solve them, since it is quite difficult to debug such code.
