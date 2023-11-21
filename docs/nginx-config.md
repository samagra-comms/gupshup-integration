### Understanding NGINX Configuration Structure
1. **Main Context**: Global configuration settings, usually found in the `nginx.conf` file.
2. **Events Context**: Configures NGINX event-processing model, like worker connections.
3. **HTTP Context**: Settings for HTTP/HTTPS, where you define `server` blocks (virtual hosts).
4. **Server Context**: Defines a virtual server to handle requests; you can have multiple `server` blocks.
5. **Location Context**: Specifies how to process requests for different resources within a `server` block.

### Key Directives for Reverse Proxy
- `proxy_pass`: Specifies the protocol and address of a proxied server and an optional URI.
- `proxy_set_header`: Sets the value of a header field in the request sent to the proxied server.

### Basic Reverse Proxy Configuration
Here's a simple example of configuring a reverse proxy in NGINX:

```nginx
http {
    server {
        listen 80;
        server_name example.com;

        location / {
            proxy_pass http://backend_server_address:port;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

In this configuration, NGINX listens on port 80 and forwards all incoming HTTP requests to the specified backend server.

### Steps to Write a Reverse Proxy Config
1. **Open/Create a Config File**: Typically located at `/etc/nginx/nginx.conf` or `/etc/nginx/sites-available/yourdomain`.
2. **Set Up a Server Block**: Start by defining a `server` block inside the `http` context.
3. **Specify Listening Port and Server Name**: Use `listen` and `server_name`.
4. **Configure Reverse Proxy**: In a `location` block, use `proxy_pass` to specify the backend server, and `proxy_set_header` to handle headers.
5. **Test Configuration**: Validate your config with `nginx -t`.
6. **Reload NGINX**: Apply changes with`nginx -s reload` or `systemctl reload nginx`.

### Best Practices
- **Logging**: Set up access and error logs for debugging.
- **Securing the Proxy**: Implement SSL/TLS for secure connections.
- **Performance Tuning**: Adjust worker processes and connections based on server capacity.
- **Caching**: Consider caching static content for improved performance.

### Troubleshooting
- Use `nginx -t` to check the syntax before reloading.
- Check NGINX error logs (`/var/log/nginx/error.log`) for issues.