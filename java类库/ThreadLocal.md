ThreadLocal是一个本地线程副本变量的工具类，主要用于将线程和该线程存放的副本对象做一个映射，各个线程之间的变量互不干扰。 

- 每个Thread线程内部都有一个Map。 
- Map里面存储线程本地对象（key）和线程的变量副本（value） 
- 但是，Thread内部的Map是由ThreadLocal维护的，由ThreadLocal负责向map获取和设置线程的变量值。 

**ThreadLocalMap解决hash冲突的方法：步长加1或减1，寻找下一个相邻的位置。 **

**带来的问题：** 由于ThreadLocalMap的key是弱引用，而Value是强引用。这就导致了一个问题，ThreadLocal在没有外部对象强引用时，发生GC时弱引用Key会被回收，而Value不会回收，如果创建ThreadLocal的线程一直持续运行，那么这个Entry对象中的value就有可能一直得不到回收，发生内存泄露。



**项目使用：**存储用户的登录信息：创建一个HostHolder组件，并在组件里自定义一个ThreadLocal变量，专门存储用户的信息，在用户每次的请求中通过拦截器来获取用户的信息，然后通过ThreadLocal的set方法将用户信息存储，使得在controller请求中可以携带用户信息，并在拦截器的afterCompletion方法中通过ThreadLocal的remove方法清除value，防止内存泄露。