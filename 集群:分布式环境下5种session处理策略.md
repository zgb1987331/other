转载自：[http://blog.csdn.net/u010028869/article/details/50773174?ref=myread](http://blog.csdn.net/u010028869/article/details/50773174?ref=myread)

##第一种：粘性session

**原理：**粘性Session是指将用户锁定到某一个服务器上，比如上面说的例子，用户第一次请求时，负载均衡器将用户的请求转发到了A服务器上，如果负载均衡器设置了粘性Session的话，那么用户以后的每次请求都会转发到A服务器上，相当于把用户和A服务器粘到了一块，这就是粘性Session机制。

##第二种：服务器session复制

***原理：***任何一个服务器上的session发生改变（增删改），该节点会把这个 session的所有内容序列化，然后广播给所有其它节点，不管其他服务器需不需要session，以此来保证Session同步。

##第三种：session共享机制
使用分布式缓存方案比如memcached、redis，但是要求Memcached或Redis必须是集群。
使用Session共享也分两种机制，两种情况如下：

###粘性session处理方式
***原理：***不同的 tomcat指定访问不同的主memcached。多个Memcached之间信息是同步的，能主从备份和高可用。用户访问时首先在tomcat中创建session，然后将session复制一份放到它对应的memcahed上。memcache只起备份作用，读写都在tomcat上。当某一个tomcat挂掉后，集群将用户的访问定位到备tomcat上，然后根据cookie中存储的SessionId找session，找不到时，再去相应的memcached上去session，找到之后将其复制到备tomcat上。
###非粘性session处理方式
***原理：***memcached做主从复制，写入session都往从memcached服务上写，读取都从主memcached读取，tomcat本身不存储session

##第四种：session持久化到数据库
***原理：***拿出一个数据库，专门用来存储session信息。保证session的持久化。

##第五种terracotta实现session复制
***原理：***Terracotta的基本原理是对于集群间共享的数据，当在一个节点发生变化的时候，Terracotta只把变化的部分发送给Terracotta服务器，然后由服务器把它转发给真正需要这个数据的节点。可以看成是对第二种方案的优化。
