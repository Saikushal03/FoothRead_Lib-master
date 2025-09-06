# Foothread  

Foothread is a simple threading library written in C.  
It’s built on top of the Linux `clone()` system call and System V semaphores, and it gives you a lightweight way to create and manage threads without pulling in the full complexity of `pthreads`.  

The goal of this project is to show how threads, mutexes, and barriers can be built from scratch.  

---

## What it can do  

- Create threads (joinable or detached)  
- Set custom stack sizes  
- Handle thread exits and cleanups  
- Provide basic mutexes for synchronization  
- Provide barriers for coordination between multiple threads  

Basically, it’s a mini version of `pthreads`, implemented for learning and experimenting.  

---

## Project Layout  

.
├── foothread.c # Core implementation
├── foothread.h # Header file with types and function declarations

yaml
Copy code

---

## Building  

You can compile your program with `foothread` like this:  

```bash
gcc -o main main.c foothread.c -Wall
Replace main.c with your own test file that includes foothread.h.

Example Usage
Creating a thread
c
Copy code
#include "foothread.h"

int worker(void *arg) {
    printf("Hello from thread!\n");
    foothread_exit();
    return 0;
}

int main() {
    foothread_t t;
    foothread_attr_t attr;

    foothread_attr_setjointype(&attr, FOOTHREAD_JOINABLE);
    foothread_attr_setstacksize(&attr, FOOTHREAD_DEFAULT_STACK_SIZE);

    foothread_create(&t, &attr, worker, NULL);

    sleep(1); // give thread time to run
    return 0;
}
Using a mutex
c
Copy code
foothread_mutex_t mutex;

foothread_mutex_init(&mutex);

foothread_mutex_lock(&mutex);
// critical section
printf("Thread %d in critical section\n", gettid());
foothread_mutex_unlock(&mutex);

foothread_mutex_destroy(&mutex);
Barrier example
c
Copy code
foothread_barrier_t barrier;
foothread_barrier_init(&barrier, 3);

// Each thread calls this
foothread_barrier_wait(&barrier);

foothread_barrier_destroy(&barrier);
Notes & Limitations
Works only on Linux (since it uses clone() and System V IPC).

Limited to 100 threads by default (FOOTHREAD_THREADS_MAX).

You need to manually clean up mutexes and barriers.

Detached threads clean themselves up, but joinable threads need to be handled explicitly.

Why this project exists
This is not meant to replace pthreads. It’s more of an educational project to understand:

How threads are created at the system call level

How synchronization can be built using semaphores

How higher-level abstractions (like barriers) are layered on top

If you’re curious about how threading works under the hood, this library gives you a hands-on way to explore it.

License
This project is released for learning and experimentation. Feel free to use or adapt it.
