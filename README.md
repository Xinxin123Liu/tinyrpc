来源知乎
https://www.zhihu.com/question/25536695?q=RPC
洪春涛
小而精的RPC库 tinyrpc（hjk41/tinyrpc），对于理解RPC如何工作很有好处。

tinyrpc
=======

tinyrpc is a simple RPC library. The communication layer can be implemented with any network library. 

Currently, we suport ASIO and ZeroMQ as the network layer.

Current implementation uses C++14, so you need a new compiler to compile the code.

Tested on both Linux and Windows (Visual Studio 2017).

Programming interface
=======

```c++
    // start server
    // ...
    rpc.RegisterProtocol<int, int, int>(UniqueId("add"), 
        [](int x, int y) { return x + y; });
    rpc.StartServing();

    // now, create a client
    AsioEP ep(asio::ip::address::from_string("127.0.0.1"), port);
    int result;
    rpc.RpcCall(ep, UniqueId("add"), result, 1, 2);
    std::cout << "1 + 2 = " << result << std::endl;
```

Please refer to [/test/test.cpp](/test/test.cpp) for an example on how to use tinyrpc.


github上有那些适合学习的MPI项目？
洪春涛
做系统的
8 人赞同了该回答
MPI的同步通信用的还是最多的，用起来也很简单，就是send/recv，bcast，barrier，这些查一下手册就好

我很早以前拿MPI实现过rpc，主要靠MPI_Iprobe，有兴趣可以看看：https://github.com/hjk41/tinyrpc/blob/302a23d33a27e0e42a550cbc47075a2370315eb9/TinyRPC/tinycomm.cpp

isend/irecv这些异步的容易搞错，偶尔用用就好。有iprobe其实就够实现 master/slave了，没必要非用isend/irecv



MPI程序难点一般在逻辑，而不在MPI本身，MPI本身太过低级，所以api并不复杂。很多人觉得mpi复杂是因为在接触mpi之前没有接触过多进程编程，甚至连多线程编程都没接触过。如果只是学mpi，最有效率的方法还是直接去找个实际问题，上手敲代码
