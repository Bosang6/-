## Mutex

### 声明

#### 互斥锁声明

```c
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;
```

PTHREAD_MUTEX_INITIALIZER：是一个宏，用于初始化mutex锁

#### 条件变量声明

```c
pthread_cond_t cond = PTHREAD_COND_INITIALIZER;
```

PTHREAD_COND_INITIALIZER：是一个宏，用于初始化条件变量

### 上锁

```c
pthread_mutex_lock(&mutex);
```

### 解锁

```c
pthread_mutex_unlock(&mutex);
```

### 条件变量和Mutex锁的搭配使用

我们可以在一个线程准备进入临界区（sezione critica）之前，添加一个进入临界区的条件。

可以使用一个**while循环**，在判断语句内写上进入临界区的**逆条件**，即当while循环被满足时，线程无法进入临界区。

```c
while (current_thread != thread_num) {		//当前执行线程tid 不等于 可进入临界区的tid
	pthread_cond_wait(&cond, &mutex);
}
```

循环内执行 **pthread_cond_wait** 函数，该函数将会阻塞当前线程，同时释放自己的锁，供别的线程使用。

```c
pthread_cond_broadcast(&cond);
```

一旦某个线程执行完临界区的代码后，通过 **pthread_cond_broadcast** 函数来唤醒所有在等待cond条件的线程，它们会进入下一轮的while语句判断。

### 销毁环境变量和Mutex锁

```c
pthread_mutex_destroy(&mutex);
pthread_cond_destroy(&cond);
```

