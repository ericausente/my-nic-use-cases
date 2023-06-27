```
% kubectl exec -it ingress-plus-nginx-ingress-controller-5df497d9ff-dzddd -- sh -c "cat /etc/nginx/conf.d/vs_default_secure-app-virtual-server.conf  | awk NF"

upstream vs_default_secure-app-virtual-server_my-service {
    zone vs_default_secure-app-virtual-server_my-service 512k;
    random two least_conn;
    server nmsnim.kushikimi.xyz:443 max_fails=1 fail_timeout=10s max_conns=0 resolve;
}
server {
    listen 80;
    listen [::]:80;
    server_name main.kushikimi.xyz;
    status_zone main.kushikimi.xyz;
    set $resource_type "virtualserver";
    set $resource_name "secure-app-virtual-server";
    set $resource_namespace "default";
    listen 443 ssl;
    listen [::]:443 ssl;
    ssl_certificate /etc/nginx/secrets/default-main-kushikimi-xyz-secret;
    ssl_certificate_key /etc/nginx/secrets/default-main-kushikimi-xyz-secret;
    if ($scheme = 'http') {
        return 301 https://$host$request_uri;
    }
    server_tokens "on";
    location / {
        set $service "my-service";
        proxy_connect_timeout 60s;
        proxy_read_timeout 60s;
        proxy_send_timeout 60s;
        client_max_body_size 1m;
        proxy_buffering on;
        proxy_http_version 1.1;
        set $default_connection_header close;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection $vs_connection_header;
        proxy_pass_request_headers on;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Port $server_port;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_set_header Host "$host";
        proxy_pass https://vs_default_secure-app-virtual-server_my-service;
        proxy_next_upstream error timeout;
        proxy_next_upstream_timeout 0s;
        proxy_next_upstream_tries 0;
    }
}
