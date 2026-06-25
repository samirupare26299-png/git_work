configure nginx as load balancer

http {
    upstream balancer {
       server stapp01:3002;
       server stapp02:3002;
       server stapp03:3002;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://balancer;
        }
    }
}
