# The second part of the research on multiprocessing

## Some theory

In multiprocessing, any newly created process will do following:

- run independently
- have their own memory space.

Consider the program [multiprocessing_s2_e1.py](multiprocessing_s2_e1.py) to understand this concept:

**Result**:

```
Result(in process p1): [1, 4, 9, 16]
Result(in main program): []
```

In above example, we try to print contents of global list **result** at two places:

- In **square_list** function. Since, this function is called by process **p1**, **result** list is changed in memory space of process **p1** only.
- After the completion of process **p1** in main program. Since main program is run by a different process, its memory space still contains the empty **result** list.

## Sharing data between processes

1. **Shared memory**: **multiprocessing** module provides Array and Value objects to share data between processes.

   - **Array**: a ctypes array allocated from shared memory.
   - **Value**: a ctypes object allocated from shared memory.

   Given in [multiprocessing_s2_e2.py](multiprocessing_s2_e2.py) is a simple example showing use of Array and Value for sharing data between processes.

   **Result**:

   ```
   Result(in process p1): [1, 4, 9, 16]
   Sum of squares(in process p1): 30
   Result(in main program): [1, 4, 9, 16]
   Sum of squares(in main program): 30
   ```

   Let us try to understand the above code line by line:

   - First of all, we create an **Array** result like this:

   ```
   result = multiprocessing.Array('i', 4)
   ```

   - First argument is the **data type**. ‘i’ stands for integer whereas ‘d’ stands for float data type.
   - Second argument is the **size** of array. Here, we create an array of 4 elements.

   Similarly, we create a Value **square_sum** like this:

   ```
   square_sum = multiprocessing.Value('i')
   ```

   Here, we only need to specify data type. The value can be given an initial value(say 10) like this:

   ```
   square_sum = multiprocessing.Value('i', 10)
   ```

   - Secondly, we pass **result** and **square_sum** as arguments while creating **Process** object.

   ```
   p1 = multiprocessing.Process(target=square_list, args=(mylist, result, square_sum))
   ```

   - **result** array elements are given a value by specifying index of array element.

   ```
   for idx, num in enumerate(mylist):
       result[idx] = num * num
   ```

   **square_sum** is given a value by using its **value** attribute:

   ```
   square_sum.value = sum(result)
   ```

   - In order to print **result** array elements, we use **result[:]** to print complete array.

   ```
   print("Result(in process p1): {}".format(result[:]))
   ```

   Value of square_sum is simply printed as:

   ```
   print("Sum of squares(in process p1): {}".format(square_sum.value))
   ```

2. **Server process**: Whenever a python program starts, a **server process** is also started. From there on, whenever a new process is needed, the parent process connects to the server and requests it to fork a new process.

   A **server process** can hold Python objects and allows other processes to manipulate them using proxies.

   **multiprocessing** module provides a **Manager** class which controls a server process. Hence, managers provide a way to create data that can be shared between different processes.

   > Server process managers are more flexible than using shared memory objects because they can be made to support arbitrary object types like lists, dictionaries, Queue, Value, Array, etc. Also, a single manager can be shared by processes on different computers over a network. They are, however, slower than using shared memory.

   Consider the example given in [multiprocessing_s2_e3.py](multiprocessing_s2_e3.py):

   **Result**:

   ```
   New record added!

    Name: Sam
    Score: 10

    Name: Adam
    Score: 9

    Name: Kevin
    Score: 9

    Name: Jeff
    Score: 8
   ```

   Let us try to understand above piece of code:

   - First of all, we create a **manager** object using:

     ```
     with multiprocessing.Manager() as manager:
     ```

     All the lines under **with** statement block are under the scope of **manager** object.

   - Then, we create a list **records** in **server process** memory using

     ```
     records = manager.list([('Sam', 10), ('Adam', 9), ('Kevin',9)])
     ```

     Similarly, you can create a dictionary as **manager.dict** method.

   - Finally, we create to processes **p1** (to insert a new record in **records** list) and **p2** (to print **records**) and run them while passing **records** as one of the arguments.

## Communication between processes

Effective use of multiple processes usually requires some communication between them, so that work can be divided and results can be aggregated.

**multiprocessing** supports two types of communication channel between processes:

- Queue
- Pipe

1. **Queue**: A simple way to communicate between process with multiprocessing is to use a Queue to pass messages back and forth. Any Python object can pass through a Queue.

   **Note**: The **multiprocessing.Queue** class is a near clone of [queue.Queue](https://docs.python.org/3/library/queue.html).

   Consider the example program given in [multiprocessing_s2_e4.py](multiprocessing_s2_e4.py):

   **Result**:

   ```
   Queue elements:
   1
   4
   9
   16
   Queue is now empty!
   ```

   Let us try to understand the above code step by step:

   - Firstly, we create a **multiprocessing Queue** using:

     ```
     q = multiprocessing.Queue()
     ```

   - Then we pass empty queue **q** to **square_list** function through process **p1**. Elements are inserted to queue using **put** method.

     ```
     q.put(num * num)
     ```

   - In order to print queue elements, we use **get** method until queue is not empty.

     ```
     while not q.empty():
         print(q.get())
     ```

2. **Pipes**: A pipe can have only two endpoints. Hence, it is preferred over queue when only two-way communication is required.

   **multiprocessing** module provides **Pipe()** function which returns a pair of connection objects connected by a pipe. The two connection objects returned by **Pipe()** represent the two ends of the pipe. Each connection object has **send()** and **recv()** methods (among others).

   Consider the program given in [multiprocessing_s2_e5.py](multiprocessing_s2_e5.py):
   **Result**:

   ```
   Sent the message: hello
   Sent the message: hey
   Sent the message: hru?
   Sent the message: END
   Received the message: hello
   Received the message: hey
   Received the message: hru?
   ```

   Let us try to understand above code:

   - A pipe was created simply using:

     ```
     parent_conn, child_conn = multiprocessing.Pipe()
     ```

     The function returned two connection objects for the two ends of the pipe.

   - Message is sent from one end of pipe to another using **send** method.

     ```
     conn.send(msg)
     ```

   - To receive any messages at one end of a pipe, we use **recv** method.

     ```
     msg = conn.recv()
     ```

   - In above program, we send a list of messages from one end to another. At the other end, we read messages until we receive “END” message.
