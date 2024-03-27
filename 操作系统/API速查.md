# 进程管理

// 创建新进程
- pid_t fork(void);
- pid_t vfork(void);
- pid_t clone(unsigned long flags, void *child_stack, int *parent_tid, int *child_tid, unsigned long newtls);

// 执行程序
- int execve(const char *pathname, char *const argv[], char *const envp[]);
- int execv(const char *path, char *const argv[]);
- int execvp(const char *file, char *const argv[]);
- int execvpe(const char *file, char *const argv[], char *const envp[]);

// 进程退出
- void _exit(int status);
- int exit(int status);

// 获取进程信息
- pid_t getpid(void);
- pid_t getppid(void);
- pid_t gettid(void);
- pid_t getpgid(pid_t pid);
- pid_t getpgrp(void);

// 进程组管理
- int setpgid(pid_t pid, pid_t pgid);
- int setpgrp(void);

// 进程调度
- int nice(int inc);
- int sched_setscheduler(pid_t pid, int policy, const struct sched_param *param);
- int sched_getscheduler(pid_t pid);
- int sched_yield(void);
- int sched_get_priority_max(int policy);
- int sched_get_priority_min(int policy);

// 进程资源限制
- int getrlimit(int resource, struct rlimit *rlim);
- int setrlimit(int resource, const struct rlimit *rlim);
- int prlimit(pid_t pid, int resource, const struct rlimit *new_limit, struct rlimit *old_limit);

// 信号处理
- int kill(pid_t pid, int sig);
- int tkill(int tid, int sig);
- int tgkill(int tgid, int tid, int sig);
- int sigaction(int signum, const struct sigaction *act, struct sigaction *oldact);
- int signal(int signum, __sighandler_t handler);
- int pause(void);
- unsigned int alarm(unsigned int seconds);

// 进程时间
- int times(struct tms *buf);
- clock_t clock(void);

## 进程调度

- nt nice(int inc);
- nt sched_setscheduler(pid_t pid, int policy, const struct sched_param *param);
- nt sched_getscheduler(pid_t pid);
- nt sched_yield(void);
- nt sched_get_priority_max(int policy);
- nt sched_get_priority_min(int policy);
- nt sched_setparam(pid_t pid, const struct sched_param *param);
- nt sched_getparam(pid_t pid, struct sched_param *param);
- nt sched_setaffinity(pid_t pid, size_t cpusetsize, cpu_set_t *mask);
- nt sched_getaffinity(pid_t pid, size_t cpusetsize, cpu_set_t *mask);
- nt sched_rr_get_interval(pid_t pid, struct timespec *interval);

# 文件系统

// 文件描述符操作
- int open(const char *pathname, int flags, mode_t mode);
- int creat(const char *pathname, mode_t mode);
- int openat(int dirfd, const char *pathname, int flags, mode_t mode);
- int close(int fd);

// 读写操作
- ssize_t read(int fd, void *buf, size_t count);
- ssize_t write(int fd, const void *buf, size_t count);
- ssize_t pread(int fd, void *buf, size_t count, off_t offset);
- ssize_t pwrite(int fd, const void *buf, size_t count, off_t offset);

// 文件描述符操作
- int dup(int oldfd);
- int dup2(int oldfd, int newfd);
- int dup3(int oldfd, int newfd, int flags);
- int pipe(int pipefd[2]);
- int pipe2(int pipefd[2], int flags);
- 
// 文件定位
- off_t lseek(int fd, off_t offset, int whence);
- 
// 文件状态
- int stat(const char *pathname, struct stat *statbuf);
- int lstat(const char *pathname, struct stat *statbuf);
- int fstat(int fd, struct stat *statbuf);
- int fstatat(int dirfd, const char *pathname, struct stat *statbuf, int flags);

// 目录操作
- int mkdir(const char *pathname, mode_t mode);
- int rmdir(const char *pathname);
- int chdir(const char *path);
- int fchdir(int fd);
- int getcwd(char *buf, size_t size);

// 文件操作
- int link(const char *oldpath, const char *newpath);
- int linkat(int olddirfd, const char *oldpath, int newdirfd, const char *newpath, int flags);
- int unlink(const char *pathname);
- int unlinkat(int dirfd, const char *pathname, int flags);
- int rename(const char *oldpath, const char *newpath);
- int renameat(int olddirfd, const char *oldpath, int newdirfd, const char *newpath);

// 文件权限
- int chmod(const char *pathname, mode_t mode);
- int fchmod(int fd, mode_t mode);
- int chown(const char *pathname, uid_t owner, gid_t group);
- int fchown(int fd, uid_t owner, gid_t group);
- int lchown(const char *pathname, uid_t owner, gid_t group);

// 文件系统
- int mount(const char *source, const char *target, const char *filesystemtype, unsigned long mountflags, const - void *data);
- int umount(const char *target);
- int umount2(const char *target, int flags);
- int pivot_root(const char *new_root, const char *put_old);

# 设备管理

// 字符设备操作
- int mknod(const char *pathname, mode_t mode, dev_t dev);
- int mknodat(int dirfd, const char *pathname, mode_t mode, dev_t dev);

// 设备I/O操作
- ssize_t readv(int fd, const struct iovec *iov, int iovcnt);
- ssize_t writev(int fd, const struct iovec *iov, int iovcnt);
- ssize_t preadv(int fd, const struct iovec *iov, int iovcnt, off_t offset);
- ssize_t pwritev(int fd, const struct iovec *iov, int iovcnt, off_t offset);
- int ioctl(int fd, unsigned long request, ...);

// 内存映射
- void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);
- int munmap(void *addr, size_t length);
- int mprotect(void *addr, size_t len, int prot);
- int msync(void *addr, size_t length, int flags);
- int mlock(const void *addr, size_t len);
- int munlock(const void *addr, size_t len);
- int mlockall(int flags);
- int munlockall(void);

// 文件锁
- int flock(int fd, int operation);
- int fcntl(int fd, int cmd, ... /* arg */ );

// 设备信息
- int ioperm(unsigned long from, unsigned long num, int turn_on);
- int iopl(int level);

// 终端控制
- int tcgetattr(int fd, struct termios *termios_p);
- int tcsetattr(int fd, int optional_actions, const struct termios *termios_p);
- int tcsendbreak(int fd, int duration);
- int tcdrain(int fd);
- int tcflush(int fd, int queue_selector);
- int tcflow(int fd, int action);

## 网络管理

// 套接字操作
- int socket(int domain, int type, int protocol);
- int bind(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
- int connect(int sockfd, const struct sockaddr *addr, socklen_t addrlen);
- int listen(int sockfd, int backlog);
- int accept(int sockfd, struct sockaddr *addr, socklen_t *addrlen);
- int accept4(int sockfd, struct sockaddr *addr, socklen_t *addrlen, int flags);
- ssize_t send(int sockfd, const void *buf, size_t len, int flags);
- ssize_t recv(int sockfd, void *buf, size_t len, int flags);
- ssize_t sendto(int sockfd, const void *buf, size_t len, int flags, const struct sockaddr *dest_addr, socklen_t - addrlen);
- ssize_t recvfrom(int sockfd, void *buf, size_t len, int flags, struct sockaddr *src_addr, socklen_t *addrlen);
- ssize_t sendmsg(int sockfd, const struct msghdr *msg, int flags);
- ssize_t recvmsg(int sockfd, struct msghdr *msg, int flags);
- int shutdown(int sockfd, int how);
- int getsockopt(int sockfd, int level, int optname, void *optval, socklen_t *optlen);
- int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
- 
// 网络接口配置
- int socket(int domain, int type, int protocol);
- unsigned int if_nametoindex(const char *ifname);
- char *if_indextoname(unsigned int ifindex, char *ifname);
- struct if_nameindex *if_nameindex(void);
- void if_freenameindex(struct if_nameindex *ptr);
- 
// 网络路由
- int setsockopt(int sockfd, int level, int optname, const void *optval, socklen_t optlen);
- int getsockopt(int sockfd, int level, int optname, void *optval, socklen_t *optlen);
- 
// 网络信息
- int gethostname(char *name, size_t len);
- int sethostname(const char *name, size_t len);
- int getdomainname(char *name, size_t len);
- int setdomainname(const char *name, size_t len);
- 
// 网络访问控制
- int setuid(uid_t uid);
- int setgid(gid_t gid);

## 系统信息

// 系统时间
- int time(time_t *tloc);
- int stime(const time_t *t);
- int gettimeofday(struct timeval *tv, struct timezone *tz);
- int settimeofday(const struct timeval *tv, const struct timezone *tz);
- int adjtime(const struct timeval *delta, struct timeval *olddelta);
- int adjtimex(struct timex *buf);
- 
// 系统时钟
- int clock_gettime(clockid_t clk_id, struct timespec *tp);
- int clock_settime(clockid_t clk_id, const struct timespec *tp);
- int clock_getres(clockid_t clk_id, struct timespec *res);
- int clock_nanosleep(clockid_t clock_id, int flags, const struct timespec *rqtp, struct timespec *rmtp);
- 
// 系统信息
- long sysinfo(struct sysinfo *info);
- int uname(struct utsname *buf);
- int olduname(struct oldold_utsname *name);
- 
// 系统资源
- int getrusage(int who, struct rusage *usage);
- int times(struct tms *buf);
- 
// 系统控制
- int acct(const char *name);
- int reboot(int cmd, int magic, uint, uint, int);
- int sync(void);
- int syncfs(int fd);
- int syslog(int type, char *bufp, int len);
- int klogctl(int type, char *bufp, int len);
- 
// 内核模块
- int init_module(void *module_image, unsigned long len, const char *param_values);
- int delete_module(const char *name, int flags);
- 
// 内存管理
- int brk(void *end_data_segment);
- void *sbrk(intptr_t increment);
- int munmap(void *addr, size_t length);
- int mlockall(int flags);
- int munlockall(void);
- 
// 信号量和信号量集
- int semget(key_t key, int nsems, int semflg);
- int semop(int semid, struct sembuf *sops, unsigned nsops);
- int semctl(int semid, int semnum, int cmd, union semun arg);
- int semtimedop(int semid, struct sembuf *sops, unsigned nsops, const struct timespec *timeout);

# 虚拟化

- int unshare(int flags);
- int setns(int fd, int nstype);
- int clone(unsigned long flags, void *child_stack, int *parent_tid, int *child_tid, unsigned long newtls);
- int sched_setattr(pid_t pid, const struct sched_attr *attr, unsigned int flags);
- int sched_getattr(pid_t pid, struct sched_attr *attr, unsigned int size, unsigned int flags);
- int ioprio_set(int which, int who, int ioprio);
- int ioprio_get(int which, int who);