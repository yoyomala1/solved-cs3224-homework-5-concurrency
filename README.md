Download Link: https://assignmentchef.com/product/solved-cs3224-homework-5-concurrency
<br>
<h1>Homework 5</h1>

<h1>parallel Hashtable with pthreads</h1>

In this assignment, you will take a non thread-safe version of a hash table and modify it so that it correctly supports running with multiple threads. This <strong>**does not involve xv6**</strong>; xv6 doesn’t currently support multiple threads of execution, and while it is possible to do parallel programming with processes,  it’s tricky to arrange access to some shared resource. Instead you will do this assignment on a multicore machine. Essentially any desktop machine made in the past 7 years or so should have multiple cores.

Start by downloading the attached file, <strong>parallel_hashtable.c</strong> to your local machine and you will compile it with the following command:

$ gcc -pthread parallel_hashtable.c -o parallel_hashtable

Now run it with one thread:

$ ./parallel_hashtable 1

[main] Inserted 100000 keys in 0.006545 seconds

[thread 0] 0 keys lost!

[main] Retrieved 100000/100000 keys in 4.028568 seconds

So with one thread the program is correct. But now try it with more than one thread:

<table width="0">

 <tbody>

  <tr>

   <td width="532">    $ ./parallel_hashtable 8[main] Inserted 100000 keys in 0.002476 seconds[thread 7] 4304 keys lost![thread 6] 4464 keys lost![thread 2] 4273 keys lost!     [thread 1] 3864 keys lost!     [thread 4] 4085 keys lost![thread 5] 4391 keys lost![thread 3] 4554 keys lost![thread 0] 4431 keys lost![main] Retrieved 65634/100000 keys in 0.792488 seconds</td>

  </tr>

 </tbody>

</table>




Play around with the number of threads. You should see that, in general, the program gets faster as you add more threads up until a certain point. However, sometimes items that get added to the hash table get lost.

Part 1

Find out under what circumstances entries can get lost. Update <strong>`parallel_hashtable.c`</strong> so that `<strong>insert</strong>` and `<strong>retrieve</strong>` do not lose items when run from multiple threads. Verify that you can now run multiple threads without losing any keys. Compare the speedup of multiple threads to the version that uses no mutex — you should see that there is some overhead to adding a mutex.

You will probably need:

pthread_mutex_t lock;               // declare a lock     pthread_mutex_init(&amp;lock, NULL);    // initialize the lock     pthread_mutex_lock(&amp;lock);          // acquire lock     pthread_mutex_unlock(&amp;lock);        // release lock

We will see this API in class this week. You can also use `<strong>man</strong>` to get more documentation on any of these.

Once you have a solution to this problem save it to a file called <strong>‘parallel_mutex.c’</strong>

Part 2

Make a copy of <strong>`parallel_mutex.c`</strong> and call it <strong>`parallel_spin.c`.</strong>  Replace all of the mutex APIs with the spinlock APIs in pthreads. The spinlock APIs in pthreads are:

pthread_spinlock_t spinlock;     pthread_spin_init(&amp;spinlock, 0);     pthread_spin_lock(&amp;spinlock);     pthread_spin_unlock(&amp;spinlock);




Do you see a change in the timing? Did you expect that? <strong>Write down</strong> the timing differences and your thoughts in a comment in your source file.

Part 3

For part 3 continue working with <strong>**’parallel_mutex.c’ **</strong>

Does retrieving an item from the hash table require a lock? Update the code so that multiple `retrieve` operations can run in parallel.

Part 4

For part 4 continue working with <strong>‘parallel_mutex.c’</strong>

Update the code so that some `insert` operations can run in parallel.

<strong>Hint</strong>: if two inserts are being done on *different* buckets in parallel, is a lock needed? What are the shared resources that need to be protected by mutexes?








