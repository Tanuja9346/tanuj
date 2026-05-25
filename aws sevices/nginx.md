why nginx, what nginx, install nginx and access nginx web page.


Feature	        NGINX	                      Apache
Architecture	Event-driven (asynchronous)	    Process/thread-based
Performance	    High concurrency, fast	         Slower with many connections
Memory usage	Low	                             High
Static content	Extremely fast	                 Good
Config format	Simple, declarative	             More flexible but complex
Use cases	    Web server, reverse proxy, LB	 Traditional web server


/etc/nginx/nginx.conf/ -- > main configuration file.
/etc/nginx/sites-available/ --> stores virtual host configs. {u can create n number of sites avaialble}
/etc/nginx/sites-enabled/ --> sysmlinks t0 active site configs
/var/www/html =--> default web root ditrectory. {our static web content}
/var/log/nginx/ --> contains access and error logs


A web server is software that serves static files (like .html, .css, .js, .png) over HTTP.
When users visit your website, the web server responds with these files.

NGINX is one of the fastest and most popular web servers used for this purpose.

ratelimiting,--> wait for 2mins ,ddos, waf, tls,ssl, security related.



