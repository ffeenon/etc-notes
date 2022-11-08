**hash router**
he browser doesn't send the information about the route to the web server. Thanks to that, the configuration is very straightforward.
The issue with the hash routing is the fact that it looks a little out of place. This is because users are often accustomed to clean and simple URLs.
Therefore, a hash in the middle might look strange

**browser router**
We need to configure our web server to respond with our React application regardless of the exact route for it to work. By doing that, we let React Router do all of the work needed to figure out what to present based on the URL.
The web server configuration will differ depending on whether you use nginx, Apache, Amazon S3, or something else.
we need to configure our web server to always respond with the same HTML for every possible URL. If we want to serve the API in the same origin, we need to create a pattern for it and don’t serve the React application if the URL contains the */api/* pattern, for example.

Here is a sample nginx config that might help:

server {
  listen 80 default_server;
  server_name /var/www/example.com;

  root /var/www/example.com;
  index index.html index.htm;      

  location ~* \.(?:manifest|appcache|html?|xml|json)$ {
    expires -1;
    # access_log logs/static.log; # I don't usually include a static log
  }

  location ~* \.(?:css|js)$ {
    try_files $uri =404;
    expires 1y;
    access_log off;
    add_header Cache-Control "public";
  }

  # Any route containing a file extension (e.g. /devicesfile.js)
  location ~ ^.+\..+$ {
    try_files $uri =404;
  }

  # Any route that doesn't have a file extension (e.g. /devices)
  location / {
    try_files $uri $uri/ /index.html;
  }
}

[link](https://wanago.io/2021/04/19/hashrouter-browserrouter-react/123123213)
