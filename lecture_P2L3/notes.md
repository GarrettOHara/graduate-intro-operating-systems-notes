# Pthread Creation

Birrell's Mechanisms:

### Thread

```C
pthread_t foo_thread; // type of thread
```

### Fork

Fork(Proc, args)
*thread creation*

```C
int pthread_create(pthread_t *thread,
                   const pthread_attr_t *attr,
                   void *(start)(void *),
                   void *arg));
```

### Join

Join(thread)
*close/end thread*

```C
int pthread_join(pthread_t thread, void **status);
```

status indicates if join was succesful for failed

### Pthread Attributes

- specified in pthread_create which defines the new threads
    - stack size
    - joinable
    - priority
    - scheduling policy
    - default values
*passing NULL to the attribute argument will result in default behaviour*

### Detachable Threads

*default: joinable threads*

A parent thread can create 1 or many children threads. *If the parent thread exits before closing/joining all its child threads, it leaves **zombie** threads left over*

The child threads are now detached and cannot be joined.

If the Parent thread doesn't **NEED** to stick around to close the child threads, we can create the child thread as a *detached thread* and the parent can safely exit whenever.


