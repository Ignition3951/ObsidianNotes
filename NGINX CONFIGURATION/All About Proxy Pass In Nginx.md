
# 🧭 Nginx: Everything about `proxy_pass`

^4d8071

**Author:** Utkarsh Srivastava

## 🔧 What is `proxy_pass`?

The `proxy_pass` directive in Nginx is used to forward incoming requests to another server (an “upstream”). It’s commonly used for microservice routing, ingress in Kubernetes, and hybrid static/dynamic web stacks.

---

## 🔒 HTTPS Notes

- `proxy_pass` **doesn't verify SSL certs by default** — be sure to set `proxy_ssl_verify on;` if needed.
- You can also use client certificates for mutual TLS.

---

## 🧪 Common Patterns

### Basic Use Case

```nginx
location /webapp/ {
    proxy_pass http://127.0.0.1:5000/api/;
}
A request to `/webapp/foo` is forwarded to `/api/foo`.

### Slash Behavior Cheat Sheet
|location|proxy_pass|Request|Received by upstream|
|---|---|---|---|
|/webapp/|(http://localhost:5000/api/)|/webapp/foo?bar=baz|/api/foo?bar=baz|--> /webapp/ is searched in the request (/webapp/foo?bar=baz) and replaced with (/api/)

|/webapp/|(http://localhost:5000/api)|/webapp/foo?bar=baz|/apifoo?bar=baz|-->
/webapp/ is searched in the request (/webapp/foo?bar=baz) and replaced with (/api [without the leading slash as slash is missing in proxy_pass])

|/webapp|(http://localhost:5000/api/)|/webapp/foo?bar=baz|/api//foo?bar=baz|

|/webapp|(http://localhost:5000/api)|/webapp/foo?bar=baz|/api/foo?bar=baz|

|/webapp|(http://localhost:5000/api)|/webappfoo?bar=baz|/apifoo?bar=baz|

In the table it is clear that the location string is searched in the Request string and is converted into the string after the port code in ==proxy_pass=

## ⚙️ URI Preservation

Use variables to include the original request URI:

nginx

```
location /webapp/ {
    proxy_pass http://localhost:5000/api$request_uri;
}
```

- `$uri` = path only
    
- `$request_uri` = path + query string

Another way to repeat the location is to use $uri or $request_uri. The difference is that $request_uri preserves the query parameters, while $uri discards them:

|location|proxy_pass|request|received by upstream|
|---|---|---|---|
|/webapp/|[http://localhost:5000/api$request_uri](http://localhost:5000/api%24request_uri)|/webapp/foo?bar=baz|/api/webapp/foo?bar=baz|

|/webapp/|[http://localhost:5000/api$uri](http://localhost:5000/api%24uri)|/webapp/foo?bar=baz|/api/webapp/foo|

## 🎯 Regex and Named Captures

nginx

```
location ~ ^/api/cart/([a-z]*)/(.*)$ {
    proxy_pass http://localhost:5000/cart_api?$1=$2;
}


```

## 💡 try_files Fallback

Serve static files if they exist, else proxy the request:

nginx

```
location /spa/ {
    root /app/wwwroot/;
    try_files $uri @backend;
}

location @backend {
    proxy_pass http://127.0.0.1:5000;
}

## 🧠 Command Breakdown
```

**nginx.exe -p . -c conf/nginx.conf**

- **`nginx.exe`**: This is the Nginx executable file—basically, the program you're running.
- **`-p .`**: Sets the **prefix path**. The dot (`.`) means “current directory.”

- This tells Nginx to treat your current folder as the root folder for paths in the configuration file.
- For example, if your conf file references `logs/`, it resolves as `./logs/`.

- **`-c conf/nginx.conf`**: Specifies the **custom configuration file** to use.

- Instead of the default `conf/nginx.conf` inside the Nginx install folder, it points to one in a subdirectory relative to the current working directory.

🔍 Example Use Case

If you're running Nginx in a **local development setup** where:

- Your config file is at `myproject/conf/nginx.conf`
- You run the command from the `myproject/` folder

Then this lets Nginx load the correct configuration while keeping everything nice and portable. Perfect for testing new configs without changing the system-wide setup.

Need help customizing your `nginx.conf` for a dev proxy or reverse proxy setup? Happy to dive in with you, Utkarsh.