events {
    worker_connections 1024;  
}

http {
    server {
        listen 80;
        server_name _;

        set $swagger http://172.17.0.1:8080;
        set $krakend http://172.17.0.1:3030;

        location / {
            proxy_pass $krakend;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
        
        location /swagger/ {
            proxy_pass $swagger;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }