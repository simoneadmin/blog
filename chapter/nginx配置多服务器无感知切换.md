# 废话不说，直接撸代码


<pre>

upstream tkProxy{
      server 192.168.0.1:1111 weight=2 max_fails=1 fail_timeout=10s;
      server 192.168.0.2:2222 weight=2  max_fails=1 fail_timeout=10s;
   }
</pre>
<pre>
server {
    listen 80;
    server_name ****.com;
    root /u02/nginx;
    index index.html;
    location /sys  {
        proxy_pass http://tkProxy;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	proxy_next_upstream http_502 http_504 error timeout invalid_header; 
    }
}
</pre>

>:max_fails = 3 fail_timeout=100s  表示 ${fail_timeout}（100秒）时间内出现${max-fails}（3次）次失败，就会把这个机器状态置为down（下线），就是失败$(fail_timeout)(100秒)时间后，会重新尝试启用这服务器；


>:这样就配置好一个两台服务器负载均衡的配置了；


