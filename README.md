# SimpleMultithreader
Using Multithreading with Ease

The first thing the library needs to do is include the headers it needs like pthread. # 1. #include // h for thread management # 2. #include // functional for passing callable function # 3. #include // vector for thread handles and args # 4. #include // chrono for execution time measurement # 5. #include // iostream for logging These headers form the basis of the library’s powerful and reusable multithreading features.

For example, at a lower layer of the library are two basic structures of it called ThreadArgs1D and ThreadArgs2D. These structs simply wrap the arguments that threads need to work on 1D and 2D ranges. Dynamically assigned thread id (threadId), number of threads (numThreads), 1D thread index range (low, high) or 2D thread index range (low1, high1, low2, high2) to each thread. Additionally, a callable task
There is also a callable task, in the form of a lambda function ( lambda ), which defines the computation logic for the assigned indices.

The execution starts when the user calls either one of those two parallel_for functions. 1D ParallelThen generates a range of indices to set a granular_1D_parallel_for implementation. In the same way, 2D parallel_for breaks the outer loop range into a set of outer indices to execute on threads while iterating the inner loop in full range for every assigned outer index. These partitions guarantee that the work is evenly spread across the available threads.

Task Distribution

The chunks will be distributed for processing by the threads, and the library will divide the range of the indices into equal-sized chunks. For 1D iterations, indices are divided linearly, and for 2D iterations, the outer loop is split across threads, while the inner loop is computed fully for every outer index. Should the range not divide evenly, the last thread will process the remainder, so everything always runs.

Creating and Managing Threads

Threads are created with the pthread_create function and each thread runs the designated function (thread_function_1d or thread_function_2d). These are the implementations of your threads, which assume the thread arguments, calculate the minimums and maximums of their threads, then call the user lambda function with the start and end index.

Synchronization

The last step is that pthread_join is called in the main thread, waiting for all threads to finish executing. This makes sure no thread exits early, and the main thread can continue safely, as long as all parallel computations have ended.

Parallel Execution

The library achieves a great speed-up in compute-heavy operations by distributing the load over several threads. The independence of threads allows for true parallelism and more efficient CPU usage, with each thread doing its work in its own range.
