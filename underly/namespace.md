## 名字空间

###pid 名字空间
不同用户的进程就是通过pid名字空间隔离开的，且不同名字空间中可以有相同pid。所有的LXC进程在Docker中的父进程为Docker进程，每个LXC进程具有不同的名字空间。同时由于允许嵌套，因此可以很方便的实现嵌套的Docker容器。

###net 名字空间
有了pid名字空间, 每个名字空间中的pid能够相互隔离，但是网络端口还是共享host的端口。网络隔离是通过net名字空间实现的， 每个net名字空间有独立的 网络设备, IP地址, 路由表, /proc/net 目录。这样每个容器的网络就能隔离开来。Docker默认采用veth的方式，将容器中的虚拟网卡同host上的一个Docker网桥docker0连接在一起。

###ipc 名字空间
容器中进程交互还是采用了Linux常见的进程间交互方法(interprocess communication - IPC), 包括信号量、消息队列和共享内存等。然而同 VM 不同的是，容器的进程间交互实际上还是host上具有相同pid 名字空间中的进程间交互，因此需要在IPC资源申请时加入名字空间信息，每个IPC资源有一个唯一的32位id。

###mnt 名字空间
类似chroot，将一个进程放到一个特定的目录执行。mnt 名字空间允许不同名字空间的进程看到的文件结构不同，这样每个名字空间 中的进程所看到的文件目录就被隔离开了。同chroot不同，每个名字空间中的容器在/proc/mounts的信息只包含所在名字空间的mount point。

###uts 名字空间
UTS("UNIX Time-sharing System") 名字空间允许每个容器拥有独立的hostname和domain name, 使其在网络上可以被视作一个独立的节点而非Host上的一个进程。

###user 名字空间
每个容器可以有不同的用户和组id, 也就是说可以在容器内用容器内部的用户执行程序而非Host上的用户。
