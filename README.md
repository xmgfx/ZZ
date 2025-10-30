# HTTPS configuration - 443 port
server {
    listen 443 ssl;
    listen [::]:443 ssl;
    server_name cfd.dosz.net.cn;

    # SSL certificate configuration
    ssl_certificate /etc/nginx/ssl/cfd.dosz.net.cn/dosz.net.cn_with_chain.crt;
    ssl_certificate_key /etc/nginx/ssl/cfd.dosz.net.cn/dosz.net.cn_server.key;

    # CORS configuration for frontend domain
    location / {
        # CORS headers
        add_header 'Access-Control-Allow-Origin' 'https://djkt.ihouse3d.com.cn' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization' always;
        add_header 'Access-Control-Allow-Credentials' 'true' always;
        add_header 'Access-Control-Max-Age' 86400 always;

        # Handle OPTIONS preflight requests
        if ($request_method = 'OPTIONS') {
            return 204;
        }

        # Proxy to Flask backend
        proxy_pass http://127.0.0.1:9004;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # Prevent backend from overriding CORS headers
        proxy_hide_header Access-Control-Allow-Origin;
        proxy_hide_header Access-Control-Allow-Methods;
    }
}

# HTTPS configuration - 9003 port
server {
    listen 9003 ssl;
    listen [::]:9003 ssl;
    server_name cfd.dosz.net.cn;

    # SSL certificate configuration
    ssl_certificate /etc/nginx/ssl/cfd.dosz.net.cn/dosz.net.cn_with_chain.crt;
    ssl_certificate_key /etc/nginx/ssl/cfd.dosz.net.cn/dosz.net.cn_server.key;

    # CORS configuration
    location / {
        # CORS headers
        add_header 'Access-Control-Allow-Origin' 'https://djkt.ihouse3d.com.cn' always;
        add_header 'Access-Control-Allow-Methods' 'GET, POST, PUT, DELETE, OPTIONS' always;
        add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization' always;
        add_header 'Access-Control-Allow-Credentials' 'true' always;
        add_header 'Access-Control-Max-Age' 86400 always;

        # Handle OPTIONS preflight requests
        if ($request_method = 'OPTIONS') {
            return 204;
        }

        # Proxy to Flask backend
        proxy_pass http://127.0.0.1:9004;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        # Prevent backend from overriding CORS headers
        proxy_hide_header Access-Control-Allow-Origin;
        proxy_hide_header Access-Control-Allow-Methods;
    }
}
