参考：[http://blog.csdn.net/u010028869/article/details/50773174?ref=myread](http://blog.csdn.net/u010028869/article/details/50773174?ref=myread)

## 第一种：粘性session
**原理：**
粘性Session是指将用户锁定到某一个服务器上，比如上面说的例子，用户第一次请求时，负载均衡器将用户的请求转发到了A服务器上，如果负载均衡器设置了粘性Session的话，那么用户以后的每次请求都会转发到A服务器上，相当于把用户和A服务器粘到了一块，这就是粘性Session机制。

## 第二种：服务器session复制
**原理：**
任何一个服务器上的session发生改变（增删改），该节点会把这个 session的所有内容序列化，然后广播给所有其它节点，不管其他服务器需不需要session，以此来保证Session同步。

## 第三种：session共享机制
**原理：**
使用分布式缓存方案比如memcached、redis，但是要求Memcached或Redis必须是集群。

## 第四种：session持久化到数据库
**原理：**
用一个数据库，专门用来存储session信息。保证session的持久化。