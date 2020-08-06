# Set Up Nginx Server on Ubuntu

### Create directory `example.com` in `/var/www/`
```bash
sudo mkdir -p /var/www/example.com
```

### To assign ownership
```bash
sudo chown -R $USER:$USER /var/www/example.com
```

### Create an `index.html`
```bash
sudo nano /var/www/example.com/index.html
```

#### `/var/www/example.com/index.html`
```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Set Up Nginx Server on Ubuntu</title>
</head>
<body>
    Success! Congratulations
</body>
</html>
```

### Create server block config file:
```bash
sudo nano /etc/nginx/sites-available/example.conf
```

### `/etc/nginx/sites-available/example.conf`
```bash
server {
        listen 80;

        server_name example.com www.example.com;
        root /var/www/example.com;
        index index.php index.html;

        location / {
                #try_files $uri $uri/ =404;
                try_files $uri $uri/ /index.php$is_args$args;
        }

        location ~ \.php$ {
                include fastcgi_params;
                fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
                #fastcgi_pass 127.0.0.1:9000;
                fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
                try_files $uri =404;
        }

        access_log /var/log/nginx/example_access.log;
        error_log /var/log/nginx/example_error.log;
}
```

### Crete link to `/etc/nginx/sites-enabled/`:
```bash
sudo ln -s /etc/nginx/sites-available/example.conf /etc/nginx/sites-enabled/
```

### Change `nginx.conf`:
```bash
sudo nano /etc/nginx/nginx.conf
```

### Remove the `#` symbol to uncomment the line:
#### `/etc/nginx/nginx.conf`
```bash
http {
    server_names_hash_bucket_size 64;
}
```

### Test to make sure that there are no syntax errors in any of your Nginx files:
```bash
sudo nginx -t
```

### Restart Nginx
```bash
sudo systemctl restart nginx
```

`or`

```bash
sudo service nginx restart
```

## Only for local hosts
### Open file `hosts`
```bash
sudo nano /etc/hosts
```

### Add your domain for `127.0.0.0`
```bash
127.0.0.0   example.com
```