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

## 信号量

### 创建一个信号量/访问一个信号量

```c
#include <sys/sem.h>
int semget(key_t key, int nsems, int semflg);
```

参数：

key：一个非0整数，用于标识信号量集，可以使用 IPC_PRIVATE来创建一个私有的信号量集

nsems：需要创建 或 访问 的信号量的数量，在创建时，指定需要几个信号量，在访问时，只需设置为0即可

semflg：

- IPC_CREAT 如果指定信号量不存在，则创建一个新的信号量
- IPC_EXCL 如果信号量已存在，则返回错误
- 0600 访问权限设置

返回值：

- 成功：返回信号量标识符id
- 失败：-1

### 操作和查询信号量状态

```c
#include <sys/sem.h>
int semctl(int semid, int semnum, int cmd, ... /* union semun arg */);
```

参数：

semid：通过semget获取的信号量标识符id

semnum：指定要操作的信号量在信号集中的索引index，从0开始

cmd：

- IPC_STAT 获取信号量集的状态，放入semid_ds结构中
- IPC_SET 通过提供semid_ds结构来设置信号量集的状态
- IPC_RMID 删除信号量集

...：需要传递的额外参数，例如信号量的新值或一个指向semid_ds结构的指针

返回值：

- 成功：根据cmd操作决定
- 失败：-1

### 信号量加减操作（PV操作）

```c
#include <sys/sem.h>

int semop(int semid, struct sembuf *sops, unsigned int nsops);
```

参数：

semid：信号量标识符，由 semget 创建

sops：指向 sembuf结构体数组的指针，每个sembug标识一个信号量操作

nsops：要执行的操作数，即sembuf结构体数组中的元素

#### sembuf结构体

```c
struct sembuf {
    unsigned short sem_num;  // 信号量集合中的信号量编号
    short sem_op;            // 要执行的操作
    short sem_flg;           // 操作标志
};
```

- sem_num ：信号量集合的信号量编号
- sem_op ：操作类型，负值标识 P操作，正值标识V操作，0表示检查信号量是否为0
- sem_flg：操作标志，常用的标志包括 IPC_NOWAIT (操作立即返回，不等待) 和 SEM_UNDO(进程结束时撤销操作)。

返回值：

- 成功 0
- 失败 -1







## 共享内存

### 共享内存空间创建

```c
#include <sys/pic.h>
#include <sys/shm.h>

int shmget(key_t key, size_t, int shmflg);
```

参数：

key：是一个非0整数，用于标识共享内存段，可以使用IPC_PRIVATE创建，或者自行提供一个key

size：共享内存大小

shmflg：

- IPC_CREAT 如果指定的内存段不存在，则创建
- IPC_EXCL 如果已经存在，返回错误
- 0600

返回值：

- 成功：返回共享内存段的标识符（非负整数）
- 失败：返回 -1

### 连接共享内存空间

```c
#include <sys/shm.h>
void *shmat(int shmid, const void *shmaddr, int shmflg);
```

参数：

shmid：shmget返回的共享内存标识符（非负整数）

shmaddr：指定共享内存空间地址，通常设置为NULL，让系统选择一个合适的地址

shmflg：

- SHM_RND 如果设置，shmaddr必须是系统页大小的整数倍，用于对齐地址
- SHM_RDONLY 共享内存只读

返回值：

- 成功：返回指向共享内存空间第一个字节的指针
- 失败：返回 -1 实际上是（void *） -1

### 断开共享内存

```c
#include <sys/shm.h>
int shmdt(const void *shmaddr);
```

参数：

shmaddr：指向共享内存段的指针，通常为shmat成功的返回值

返回值：

- 成功 0
- 失败 -1

### 共享内存空间的控制操作

```c
#include <sys/msg.h>
int shmctl(int shmid, int cmd, struct shmid_ds *buf);
```

参数：

shmid：shmget返回的共享内存标识符

cmd：

- IPC_STAT 将共享内存段的当前相关数据结构复制到buf内，buf为shmid_ds结构
- IPC_SET 根据buf指向的shmid_ds 结构中的数据设置共享内存段的参数
- IPC_RMID 删除共享内存段

返回值：

- 成功 0
- 失败 -1

#### shmid_ds 结构

```c
struct shmid_ds {
    struct ipc_perm shm_perm;  // 权限结构
    size_t shm_segsz;          // 共享内存段的大小
    time_t shm_atime;          // 最后一次附加时间
    time_t shm_dtime;          // 最后一次分离时间
    time_t shm_ctime;          // 最后一次修改时间
    pid_t shm_cpid;            // 创建进程的 PID
    pid_t shm_lpid;            // 最后操作进程的 PID
    shmatt_t shm_nattch;       // 当前附加的进程数量
};
```

