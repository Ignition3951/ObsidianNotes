# 🚀 Local Dev Setup with Nginx, Lua, and HTTPS (`utkarsh.com`)

This guide documents how to build a local development environment using Nginx, Lua scripting (via OpenResty), Docker, and HTTPS with a custom domain `utkarsh.com`.

---
## 📦 Docker + Nginx

Run a temporary Nginx container:

```bash
docker run -it --rm -d -p 8080:80 --name web nginx
- `-it`: interactive terminal
    
- `--rm`: remove container on exit
    
- `-d`: detached/background mode
    
- `-p 8080:80`: map host port 8080 to container port 80
    
- `--name web`: names the container
    
- `nginx`: image used

Instead of using nginx we can use openresty as it enables the use of lua scripting with nginx.
```

## 🔁 Lua + Nginx Proxy for `/ping`

Using OpenResty, set up a proxy that handles only `/ping` and forwards a POST request to an upstream:
location = /ping {
    content_by_lua_block {
        local http = require "resty.http"
        local httpc = http.new()
        local res, err = httpc:request_uri("http://127.0.0.1:8083/ping", {
            method = "POST",
            body = "your=data&goes=here",
            headers = {
                ["Content-Type"] = "application/x-www-form-urlencoded"
            }
        })

        if not res then
            ngx.status = 502
            ngx.say("Failed to connect: ", err)
            return
        end

        ngx.status = res.status
        ngx.say(res.body)
    }
}

## 🏠 Local Virtual Host: `utkarsh.com`

1. **Edit your hosts file**:
    
    Add:
    127.0.0.1 utkarsh.com
- Windows: `C:\Windows\System32\drivers\etc\hosts`
    
- macOS/Linux: `/etc/hosts`


2. **Nginx HTTP configuration**:
 server {
    listen 80;
    server_name utkarsh.com;

    location / {
        root /path/to/your/project;
        index index.html;
    }
}

## 🔐 Add HTTPS with Self-Signed Certificate

1. **Generate cert and key**:
 openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
  -keyout utkarsh.key -out utkarsh.crt \
  -subj "/CN=utkarsh.com"
### 🔍 Explanation:

- `openssl req` Initiates a certificate signing request (CSR) creation process.
    
- `-x509` Generates a self-signed certificate instead of a regular CSR.
    
- `-nodes` Means "no DES encryption" on the private key. It prevents the key file from being password-protected, which is useful for automated servers like Nginx.
    
- `-days 365` Sets the validity of the certificate to **365 days**.
    
- `-newkey rsa:2048` Creates a new RSA private key with a size of **2048 bits**.
    
- `-keyout utkarsh.key` Specifies the filename for the **private key**.
    
- `-out utkarsh.crt` Specifies the filename for the **certificate** file.
    
- `-subj "/CN=utkarsh.com"` Defines the **certificate subject**. `CN` stands for _Common Name_, and it should match the domain name youre securing—here, `utkarsh.com`.


Here’s how to generate a **self-signed wildcard SSL certificate** using OpenSSL, ideal for local dev:
🛠️ Command:

	openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
	  -keyout utkarsh.key -out utkarsh.crt \
	  -subj "/CN=*.utkarsh.com"


### 🔍 What changed?

- `CN=*.utkarsh.com`: The asterisk makes it a **wildcard cert**.
    
- Everything else remains as in your original setup.
    

This cert will now work for:

- `https://utkarsh.com`
    
- `https://api.utkarsh.com`
    
- `https://anything.utkarsh.com`


### ✅ Don’t forget...

To test locally, you still need to map any subdomain to `127.0.0.1` in your `hosts` file. For example:

127.0.0.1 utkarsh.com
127.0.0.1 api.utkarsh.com
127.0.0.1 dev.utkarsh.com




1. **Configure Nginx for SSL**:
  ```js
  server {
    listen 443 ssl;
    server_name utkarsh.com;

    ssl_certificate     C:/nginx/certs/utkarsh.crt;
    ssl_certificate_key C:/nginx/certs/utkarsh.key;

    location / {
        root C:/Users/Utkarsh/Projects/website;
        index index.html;
    }
}
```

2. **Optional HTTP → HTTPS redirect**:

```js
 server {
    listen 80;
    server_name utkarsh.com;
    return 301 https://$host$request_uri;
}
```
3.  [[All About Proxy Pass In Nginx]]


## 🛡️ Trust Certificate (Browser Friendly)

- **Windows**: Double-click `.crt`, install into “Trusted Root Certification Authorities”.
    
- **macOS**: Use Keychain Access, drag the cert into login/System and “Always Trust”.
    
- **Linux**: Use `update-ca-certificates` or add to NSS store.

## ✅ Test Everything

Open in browser:
  https://utkarsh.com
