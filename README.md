# evil_ftp


本脚本配合CVE-2021-3129 laravel debug rce食用更佳

运行上述脚本，一个恶意ftp服务就起来了(注释已经很详细了）

这个脚本做的事情很简单，就是当客户端第一次连接的时候返回我们预设的payload

当客户端第二次连接的时候将客户端的连接重定向到127.0.0.1:9000,也就是我们的php-fpm服务的端口

ps:其中ftp服务端返回的状态码是比较重要的，如果状态码不对是无法连接成功的，但是这些状态码也不是特别严格
贴一篇参考文章：
https://blog.csdn.net/qq981378640/article/details/51254177

**使用该脚本需要自己修改对应的ip:port**

脚本中的payload是使用的gopherus生成的攻击fastcgi的payload,使用时也记得自己去生成一份


有了上面的ftp server,下面的两行看似无懈可击的代码也就可以被RCE了：

`$a = file_get_contents("ftp://aaa@172.16.230.146:23/123");`
`file_put_contents("ftp://aaa@172.16.230.146:23/123", $a);`

反弹回的shell:

![](https://mmbiz.qpic.cn/mmbiz_jpg/1YLJUhm0ZtlwAYYnTsn47LmZt7GOXmwgGcicmWdouVUM6a04jjUAapuCLDeW7JEINazFGL04icicXXaZRBYyuy63A/640?wx_fmt=jpeg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

关于laravle debug rce的复现在这里：

https://mp.weixin.qq.com/s/UPf62W0LoOGsFAdH4uqBcQ
